# 自定义 Derive

![Minimum Rust version: 1.15](https://img.shields.io/badge/Minimum%20Rust%20Version-1.15-brightgreen.svg)

在 Rust 中，你始终可以能够通过derive属性来自动实现一些特性：

```rust
#[derive(Debug)]
struct Pet {
    name: String,
}
```

`Pet` 实现了 `Debug` 特性， 使用了相当少的代码，非常醒目。举个例子，如果没有 `derive`，你需要这样写：

```rust
use std::fmt;

struct Pet {
    name: String,
}

impl fmt::Debug for Pet {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        match self {
            Pet { name } => {
                let mut debug_trait_builder = f.debug_struct("Pet");

                let _ = debug_trait_builder.field("name", name);

                debug_trait_builder.finish()
            }
        }
    }
}
```

哈!

但是，这仅适用于作为标准库的一部分提供的特征; 它不可定制。 但是现在，当有人想要推导出你的特质时，你可以告诉Rust要做什么。
这在[serde](https://serde.rs/)， [Diesel](http://diesel.rs/)等流行的crate中大量使用。

获取更多信息，包括如果构建你自己的derive，查阅 [The Rust Programming Language](https://doc.rust-lang.org/book/second-edition/appendix-04-macros.html#procedural-macros-for-custom-derive).
