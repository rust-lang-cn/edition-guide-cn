# MUSL 支持完全静态二进制文件

![Minimum Rust version: 1.1](https://img.shields.io/badge/Minimum%20Rust%20Version-1.1-brightgreen.svg)

默认情况下，Rust 将静态链接所有 Rust 代码。但是，如果使用标准库，它将动态链接到系统的 `libc` 实现。

如果您想要100％静态二进制文件，可以在 Linux 上使用 [`MUSL libc`](https://www.musl-libc.org/)。

## 安装MUSL支持

要添加对MUSL的支持，您需要选择正确的目标。 [这个页面](https://forge.rust-lang.org/platform-support.html) 有完整目标支持列表，其中一些是`musl`。

如果你不确定你想要什么，对于64位 Linux，它可能是 `x86_64-unknown-linux-musl`。 我们将在本指南中使用此目标，但其他目标的说明保持不变，只需在我们提及目标的位置更改名称。

要获得对此目标的支持，请使用 `rustup`：

```console
$ rustup target add x86_64-unknown-linux-musl
```

这将安装对默认工具链的支持; 要安装其他工具链，请添加 `--toolchain` 标志。例如：

```console
$ rustup target add x86_64-unknown-linux-musl --toolchain=nightly
```

## 使用MUSL构建

要使用这个新目标，请将 `--target` 标志传递给 Cargo：

```console
$ cargo build --target x86_64-unknown-linux-musl
```

生成的二进制文件现在将使用 MUSL 构建！
