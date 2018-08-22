# patch 替换依赖

![Minimum Rust version: 1.21](https://img.shields.io/badge/Minimum%20Rust%20Version-1.21-brightgreen.svg)

当你想要覆盖依赖图的某些部分时，可以使用你的 `Cargo.toml` 的 `[patch]` 部分。

> cargo 有一个类似的 `[replace]` 功能; 虽然我们不打算弃用或删除 `[replace]`，但在任何情况下都应该更喜欢 `[patch]`。

那么它看起来像什么？ 假设我们有一个看起来像这样的 Cargo.toml：

```toml
[dependencies]
foo = "1.2.3"
```

另外，我们的 `foo` 包依赖于 `bar` 包，我们在 `bar` 中发现了一个错误。为了测试这个，我们下载了 `bar` 的源代码，然后更新我们的 `Cargo.toml`：

```toml
[dependencies]
foo = "1.2.3"

[patch.crates-io]
bar = { path = '/path/to/bar' }
```

现在，当你 `cargo build` 时，它将使用本地版本的 `bar`，而不是来自 crates.io 的 `foo` 所依赖的版本。然后，您可以尝试更改，并修复该错误！

更多细节，查阅 [the documentation for `patch`](https://doc.rust-lang.org/cargo/reference/manifest.html#the-patch-section).
