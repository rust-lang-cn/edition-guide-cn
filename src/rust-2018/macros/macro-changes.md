# 宏的变化

![Minimum Rust version: nightly](https://img.shields.io/badge/Minimum%20Rust%20Version-nightly-red.svg)

在 Rust 2018 中，您可以通过 `use` 语句从外部包中导入特定的宏，而不是旧的 `#[macro_use]` 属性。

举个例子，考虑 `bar` 包中实现了一个 `!bar` 宏，在 `src/lib.rs` 中：

```rust
#[macro_export]
macro_rules! baz {
    () => ()
}
```

在你的包中，你可以这样写：

```rust,ignore
// Rust 2015

#[macro_use]
extern crate bar;

fn main() {
    baz!();
}
```

现在你可以这样：

```rust,ignore
// Rust 2018
#![feature(rust_2018_preview)]

use bar::baz;

fn main() {
    baz!();
}
```

这会使 `macro_rules` 宏更接近其他类型的项目。


## 程序宏
使用过程宏来派生特征时，您必须命名提供自定义派生的宏。
这通常与特征的名称相匹配，但请查阅提供派生的包的文档以确认。

举个例子，在 Serde 中，你可以这样写：

```rust,ignore
// Rust 2015
extern crate serde;
#[macro_use] extern crate serde_derive;

#[derive(Serialize, Deserialize)]
struct Bar;
```

现在你可以这样：

```rust,ignore
// Rust 2018
#![feature(rust_2018_preview)]
use serde_derive::{Serialize, Deserialize};

#[derive(Serialize, Deserialize)]
struct Bar;
```


## 更多细节：
这仅适用于外部包中定义的宏。 对于本地定义的宏，`#[macro_use] mod foo;` 还是需要的，如同 Rust 2015 一样。
