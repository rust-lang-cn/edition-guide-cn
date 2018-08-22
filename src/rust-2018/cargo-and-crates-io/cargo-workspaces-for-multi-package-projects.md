# Cargo workspaces 用于有多个子包的项目

![Minimum Rust version: 1.12](https://img.shields.io/badge/Minimum%20Rust%20Version-1.12-brightgreen.svg)

Cargo 曾经有两个组织层次：

* 一个 *package* 有一个或多个 crates
* 一个 *crate* 有一个或多个 modules

Cargo 现在有一个额外的层次：

* 一个 *workspace* 包含一个或多个 packages

[the `futures` package]: https://github.com/rust-lang-nursery/futures-rs

这对于大型项目非常有用。例如，[the `futures` package] 是一个 *workspace*，包含许多相关的包：

* futures
* futures-util
* futures-io
* futures-channel

还有其他。

Workspaces 允许单独开发这些包，但它们共享一组依赖项，因此只有单个 target 目录和单个 `Cargo.lock`。

更多有关 workspaces, 请查阅 [the Cargo documentation](https://doc.rust-lang.org/stable/cargo/reference/manifest.html#the-workspace-section).
