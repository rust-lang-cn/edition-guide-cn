# `impl Trait` 轻松返回复杂的类型

![Minimum Rust version: 1.26](https://img.shields.io/badge/Minimum%20Rust%20Version-1.26-brightgreen.svg)

`impl Trait` 是指定实现特定特征的未命名但有具体类型的新方法。 你可以把它放在两个地方：参数位置和返回位置。

```rust,ignore
trait Trait {}

// argument position
fn foo(arg: impl Trait) {
}

// return position
fn foo() -> impl Trait {
}
```

## 参数位置
在参数位置上，这个特性是十分简单的，下面这两种写法几乎相同：

```rust,ignore
trait Trait {}

fn foo<T: Trait>(arg: T) {
}

fn foo(arg: impl Trait) {
}
```

也就是说，它是泛型类型参数的简短的语法。这意味着，“ `arg` 是一个参数，它可以是实现了 `Trait` 特征的任何类型。”

但是，在技术上，`T: Trait` 和 `impl Trait` 有着一个很重要的不同点。
当你编写前者时，可以使用turbo-fish语法在调用的时候指定`T`的类型，如 `foo::<usize>(1)`。 
在 `impl Trait` 的情况下，只要它在函数定义中使用了，不管什么地方，都不能再使用turbo-fish。 
因此，您应该注意，更改两者和切换到 `impl Trait` 都会对代码的用户构成重大变化。

## 返回参数
在返回位置，此功能更有趣。这意味着“我正在返回一些实现了 `Trait` 特征的类型，但我不打算告诉你究竟是什么类型。” 
在 `impl Trait` 之前，你可以用特征对象做到这一点：

```rust
trait Trait {}

impl Trait for i32 {}

fn returns_a_trait_object() -> Box<dyn Trait> {
    Box::new(5)
}
```

但是，这会产生一些开销： `Box <T>` 表示这里有堆分配，这将使用动态分配。
有关此语法的说明，请参阅 `dyn Trait` 部分。 但是我们在这里只返回一个可能的东西，即 `Box <i32>`。
这意味着我们即使我们不使用动态分配，但是依旧为它而付出了代价！

使用 `impl Trait`，上面的代码如下：

```rust
trait Trait {}

impl Trait for i32 {}

fn returns_a_trait_object() -> impl Trait {
    5
}
```

在这里，我们没有 `Box <T>`，没有特征对象，也没有动态分配。但我们仍然可以实现 `i32` 的返回类型。

使用 `i32`，这看起来并不是非常有用。但 Rust 中有一个主要运用的地方，它更有用： 闭包。

### `impl Trait` 和闭包

> 如果你想要了解闭包，参阅 [their chapter in the book](https://doc.rust-lang.org/book/second-edition/ch13-01-closures.html).

在 Rust 中，闭包具有独特的，不可写的类型。然而，他们确实实现了 `Fn` 系列的特征。
这意味着，在以前，从函数处返回闭包的唯一方法是，使用trait对象：

```rust
fn returns_closure() -> Box<dyn Fn(i32) -> i32> {
    Box::new(|x| x + 1)
}
```

你不能写明闭包的类型，仅仅是使用 `Fn` 特性。这意味着特征对象是必须的，但是，利用 `impl Trait` 呢：

```rust
fn returns_closure() -> impl Fn(i32) -> i32 {
    |x| x + 1
}
```

我们现在可以直接返回闭包类型，就像其他的返回值那样！

## 更多的细节
以上是你需要了解和使用 `impl Trait` 的所有内，但是还有一些更细微的细节： 类型参数和参数位置 `impl Trait` 的普遍性（普遍量化的类型）。
同时，`impl Trait` 在返回位置是存在的（存在量化的类型）。 好吧，也许这有点太行话了。 我们退一步吧。

考虑下面这个函数:

```rust,ignore
fn foo<T: Trait>(x: T) {
```

当你调用它时，你设置类型，`T`。 “你”是这里的调用者。 这个签名说“我接受任何实现Trait的类型”。（“任何类型” == 行话中的*通用*）

这个版本:

```rust,ignore
fn foo<T: Trait>() -> T {
```
相似但是有些不同，你这个调用者，提供一个你想要的返回类型 `T`, 你可以看一下现在 Rust 中的 parse 和 collect :

```rust,ignore
let x: i32 = "5".parse()?;
let x: u64 = "5".parse()?;
```

这里， `.parse` 有如下的签名：

```rust,ignore
pub fn parse<F>(&self) -> Result<F, <F as FromStr>::Err> where
    F: FromStr,
```

如你所想，虽然结果类型和 `FromStr` 有一个相关的类型... 无论如何，你可以看到 `F` 在这里的返回位置。所以你有能力去选择了。

使用 `impl Trait`，你会说“嘿，有些类型存在实现这个特性，但我不会告诉你它是什么。” （术语中的“存在主义”，“某种类型存在”）。
所以现在，调用者无法选择，函数本身可以选择。如果我们尝试使用 `Result <impl F，...` 定义解析作为返回类型，它将无效。

### 使用 `impl Trait` 在更多的地方
如前所述，作为一个开始，您将只能使用 `impl Trait` 作为自由或固有函数的参数或返回类型。
但是，现在 `impl Trait` 不能在traits的实现中使用，也不能用作let绑定的类型或类型别名。未来其中一些限制将被取消。
获取更多的信息，查看[tracking issue on `impl Trait`](https://github.com/rust-lang/rust/issues/34511).
