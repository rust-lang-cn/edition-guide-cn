# 支持 trait 对象的更多容器类型

![Minimum Rust version: 1.2](https://img.shields.io/badge/Minimum%20Rust%20Version-1.2-brightgreen.svg)

在 Rust 1.0 中，只有某些特殊的类型可以创建成 [trait objects](https://doc.rust-lang.org/book/second-edition/ch17-02-trait-objects.html).

在 Rust 1.2 中，这种限制被解除了，更多的类型可以做到这一点。 例如，
`Rc<T>`，Rust 的引用计数类型之一：

```rust
use std::rc::Rc;

trait Foo {}

impl Foo for i32 {
    
}

fn main() {
    let obj: Rc<dyn Foo> = Rc::new(5);
}
```

这段代码在 Rust 1.0 中无法执行，但是现在可以了。

> 如果您之前没有看过 `dyn` 语法，请参阅相关章节。对于不支持它的版本，将 `Rc <dyn Foo>` 替换为 `Rc <Foo>`。
