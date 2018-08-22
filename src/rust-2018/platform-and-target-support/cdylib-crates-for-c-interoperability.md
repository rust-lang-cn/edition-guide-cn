# cdylib 与 C 交互

![Minimum Rust version: 1.10](https://img.shields.io/badge/Minimum%20Rust%20Version-1.10-brightgreen.svg) for `rustc`

![Minimum Rust version: 1.11](https://img.shields.io/badge/Minimum%20Rust%20Version-1.11-brightgreen.svg) for `cargo`

如果你正在生成一个打算从 C（或其他语言通过 C FFI）使用的库，则 Rust 不需要在最终目标代码中包含特定于 Rust 的内容。
对于像这样的库，你需要在你的 `Cargo.toml` 中使用 `cdylib` crate 类型：

```toml
[lib]
crate-type = ["cdylib"]
```

这将生成一个较小的二进制文件，其中没有特定于 Rust 的信息。
