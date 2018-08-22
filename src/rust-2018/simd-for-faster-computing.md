# SIMD 更快的计算

![Minimum Rust version: 1.27](https://img.shields.io/badge/Minimum%20Rust%20Version-1.27-brightgreen.svg)

[SIMD](https://en.wikipedia.org/wiki/SIMD) 的基础部分已经可用了！
SIMD 代表“单指令，多数据”。考虑这样的函数：

```rust
pub fn foo(a: &[u8], b: &[u8], c: &mut [u8]) {
    for ((a, b), c) in a.iter().zip(b).zip(c) {
        *c = *a + *b;
    }
}
```

在这里，我们采用两个切片，并将数字加在一起，将结果放在第三个切片中。最简单的方法是完成代码所做的工作，循环遍历每组元素，将它们添加到一起，并将其存储在结果中。
但是，编译器通常可以做得更好。 LLVM 通常会“自动向量化”这样的代码，这是“使用 SIMD ”的一个奇特术语。
想象一下，`a` 和 `b` 都是16个元素长。每个元素都是一个“u8”，这意味着每个切片都是128位数据。
使用SIMD，我们可以将 `a` 和 `b` 放入128位寄存器，将它们一起添加到*single*指令中，然后将得到的128位复制到 `c` 中。那要快得多！

虽然稳定的Rust总是能够利用自动向量化，但有时候，编译器并不够聪明，不能意识到我们可以做这样的事情。
此外，并非每个CPU都有这些功能，因此 LLVM 可能不会使用它们，因此您的程序可以在各种硬件上使用。 `std::arch` 模块允许我们直接使用这些指令，这意味着我们不需要依赖智能编译器。
此外，它还包含一些功能，允许我们根据各种标准选择特定的实现。例如：

```rust,ignore
#[cfg(all(any(target_arch = "x86", target_arch = "x86_64"),
      target_feature = "avx2"))]
fn foo() {
    #[cfg(target_arch = "x86")]
    use std::arch::x86::_mm256_add_epi64;
    #[cfg(target_arch = "x86_64")]
    use std::arch::x86_64::_mm256_add_epi64;

    unsafe {
        _mm256_add_epi64(...);
    }
}
```

在这里，我们使用 cfg 标志根据我们定位的机器选择正确的版本; 在 x86 上我们使用该版本，在 x86_64 上我们使用它的版本。 我们也可以在运行时选择：

```rust,ignore
fn foo() {
    #[cfg(any(target_arch = "x86", target_arch = "x86_64"))]
    {
        if is_x86_feature_detected!("avx2") {
            return unsafe { foo_avx2() };
        }
    }

    foo_fallback();
}
```

在这里，我们有两个版本的功能：一个使用 AVX2，一种特定的 SIMD 功能，可以让你进行256位操作。
`is_x86_feature_detected！` 宏将生成检测 CPU 是否支持 AVX2 的代码，如果是，则调用 foo_avx2 函数。如果没有，那么我们回到非 AVX 实现 foo_fallback。 
这意味着我们的代码将在支持 AVX2 的CPU上运行得非常快，但仍然可以在不支持 AVX2 的CPU上运行，尽管速度较慢。

如果所有这一切看起来都有点低级和狡猾，那就好了！ `std::arch` 特别适用于构建这类东西。
我们希望最终能够在更高级别的东西中稳定一个 `std::simd` 模块。
但从现在开始，这些基础点可以让生态系统开始尝试更高级别的库。

举个例子： 查阅 [faster](https://github.com/AdamNiederer/faster) 库. 这是一个没有 SIMD 的代码片段：

```rust,ignore
let lots_of_3s = (&[-123.456f32; 128][..]).iter()
    .map(|v| {
        9.0 * v.abs().sqrt().sqrt().recip().ceil().sqrt() - 4.0 - 2.0
    })
    .collect::<Vec<f32>>();
```

使用 SIMD 的代码将会更快，你需要改成这样：

```rust,ignore
let lots_of_3s = (&[-123.456f32; 128][..]).simd_iter()
    .simd_map(f32s(0.0), |v| {
        f32s(9.0) * v.abs().sqrt().rsqrt().ceil().sqrt() - f32s(4.0) - f32s(2.0)
    })
    .scalar_collect();
```

这看起来差不多： `simd_iter` 取代 `iter`, `simd_map` 取代 `map`, `f32s(2.0)` 取代 `2.0`。但是你需要一个 SIMD-ified 版本。

除此之外，您可能永远不会自己编写任何内容，但与往常一样，您依赖的库可能。
例如，正则表达式包含这些 SIMD 加速，而您根本不需要做任何事情！
