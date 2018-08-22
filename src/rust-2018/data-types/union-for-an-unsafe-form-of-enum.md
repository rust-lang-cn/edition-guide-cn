# `union`：一个非安全的 `enum`

![Minimum Rust version: 1.19](https://img.shields.io/badge/Minimum%20Rust%20Version-1.19-brightgreen.svg)

Rust 现在支持 `unions`:

```rust
union MyUnion {
    f1: u32,
    f2: f32,
}
```

Unions 是一种如同 enums 的结构，但是，它*没有标签*。 Enums 是带有
“标签” 的，用来存储哪个变种在运行时，正在被使用。unions并没有这个。

由于我们可以使用错误的变体解释 union 中保存的数据，并且 Rust 无法为
我们检查这一点，这意味着读取或写入 union 字段是不安全的：

```rust
# union MyUnion {
#     f1: u32,
#     f2: f32,
# }
let mut u = MyUnion { f1: 1 };

unsafe { u.f1 = 5 };

let value = unsafe { u.f1 };
```

模式匹配也一样工作：

```rust
# union MyUnion {
#     f1: u32,
#     f2: f32,
# }
fn f(u: MyUnion) {
    unsafe {
        match u {
            MyUnion { f1: 10 } => { println!("ten"); }
            MyUnion { f2 } => { println!("{}", f2); }
        }
    }
}
```

unions 什么时候有用？ 一个主要的用例是与 C 的互操作性。 C API 可以（并且取决于区域，经常这样做）公开 unions，
因此这使得为这些库编写 API 包装器变得非常容易。 此外，unions 还简化了依赖于值表示的
节省空间或高效缓存的结构的 Rust 实现，例如使用对齐指针的最低有效位来区分情况的机器字大小的 unions。

还有更多改进。 目前，unions 只能包含 `Copy` 类型，可能不会实现 `Drop`。 我们希望将来能够解除这些限制。
