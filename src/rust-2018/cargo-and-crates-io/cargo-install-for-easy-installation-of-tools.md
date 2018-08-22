# `cargo install` 用于自动安装

![Minimum Rust version: 1.5](https://img.shields.io/badge/Minimum%20Rust%20Version-1.5-brightgreen.svg)

Cargo 已经发展了一种新的 `install` 命令。 这旨在用于为 Cargo 安装新的子命令，或者为 Rust 开发人员安装工具。
这并不能取代为您支持的平台上的最终用户构建真实的本机程序包的需要。

例如，本指南是使用 [`mdbook`](https://crates.io/crates/mdbook)。 你可以将它安装在你的系统上:

```console
$ cargo install mdbook
```

然后使用：

```console
$ mdbook --help
```

作为扩展 Cargo 的示例，你可以使用[`cargo-update`](https://crates.io/crates/cargo-update)包。 要安装它：

```console
$ cargo install cargo-update
```

这将允许你使用此命令，该命令检查你 `cargo install` 的所有内容并将其更新为最新版本：

```console
$ cargo install-update -a
```
