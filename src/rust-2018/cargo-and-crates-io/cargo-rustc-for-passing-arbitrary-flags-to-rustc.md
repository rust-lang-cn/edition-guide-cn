# `cargo rustc` 用于传递标记至 rustc

![Minimum Rust version: 1.1](https://img.shields.io/badge/Minimum%20Rust%20Version-1.1-brightgreen.svg)

`cargo rustc` 是Cargo的一个新的子命令，它允许你通过 cargo 传递任意标记到 `rustc`。

例如，Cargo 没有办法传递内置的不稳定标志。但是如果我们想使用 `print-type-sizes` 来查看我们的类型有哪些布局信息。我们可以运行这个：

```console
$ cargo rustc -- -Z print-type-sizes
```

我们将得到一堆描述我们类型大小的输出。

## 注意
`cargo rustc` 只会将这些标记传递给你的 crate 的调用，而不是用于构建依赖项的任何 `rustc` 调用。
如果你想这样做，请参阅 `$RUSTFLAGS`。
