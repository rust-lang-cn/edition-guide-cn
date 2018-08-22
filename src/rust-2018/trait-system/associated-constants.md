# 相关常数

![Minimum Rust version: 1.20](https://img.shields.io/badge/Minimum%20Rust%20Version-1.20-brightgreen.svg)

你可以定义具有“关联函数”的 traits, structs, enums ：

```rust
struct Struct;

impl Struct {
    fn foo() {
        println!("foo is an associated function of Struct");
    }
}

fn main() {
    Struct::foo();
}
```

这个叫做“关联函数”，因为它关联了相关的类型，也就是说，它们附加到类型本身，而不是任何特定的实例。

Rust 1.20 中为关联函数增加了新的功能：

```rust
struct Struct;

impl Struct {
    const ID: u32 = 0;
}

fn main() {
    println!("the ID of Struct is: {}", Struct::ID);
}
```

其中，常量 `ID` 关联到 `Struct` 上，如果函数一样，关联常量也可以工作在 trait 和 enum 上。

Trait 具有额外的能力和相关的常数，为他们提供额外的力量。使用特征，您可以像使用关联类型一样
使用关联常量： 通过声明它，但不给它一个值。然后，特征的实现者在实现时声明其值：

```rust
trait Trait {
    const ID: u32;
}

struct Struct;

impl Trait for Struct {
    const ID: u32 = 5;
}

fn main() {
    println!("{}", Struct::ID);
}
```

在此功能之前，如果要创建表示浮点数的特征，则必须如下：

```rust
trait Float {
    fn nan() -> Self;
    fn infinity() -> Self;
    // ...
}
```

这有点笨拙，但更重要的是，因为它们是函数，所以它们不能用于常量表达式，即使它们只返回常量。
因此，`Float` 的设计也必须包含常量：

```rust,ignore
mod f32 {
    const NAN: f32 = 0.0f32 / 0.0f32;
    const INFINITY: f32 = 1.0f32 / 0.0f32;

    impl Float for f32 {
        fn nan() -> Self {
            f32::NAN
        }
        fn infinity() -> Self {
            f32::INFINITY
        }
    }
}
```

关联常量让你可以更清晰的表达，如下：

```rust
trait Float {
    const NAN: Self;
    const INFINITY: Self;
    // ...
}
```

继续实现如下：

```rust,ignore
mod f32 {
    impl Float for f32 {
        const NAN: f32 = 0.0f32 / 0.0f32;
        const INFINITY: f32 = 1.0f32 / 0.0f32;
    }
}
```

更加清晰，更通用。
