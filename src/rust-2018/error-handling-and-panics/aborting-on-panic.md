# 中止崩溃

![Minimum Rust version: 1.10](https://img.shields.io/badge/Minimum%20Rust%20Version-1.10-brightgreen.svg)

默认情况下，当发生 `panic!` 时，Rust 程序将展开堆栈。如果你更喜欢立即中止，你可以在`Cargo.toml`中配置它：

```toml
[profile.debug]
panic = "abort"

[profile.release]
panic = "abort"
```

你为什么选择这样做？通过删除对展开的支持，你将获得更小的二进制文件。你将失去捕捉崩溃的能力。哪种选择取决于你正在做什么。
