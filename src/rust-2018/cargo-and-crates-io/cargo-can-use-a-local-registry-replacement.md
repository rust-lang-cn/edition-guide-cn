# Cargo 更改源

![Minimum Rust version: 1.12](https://img.shields.io/badge/Minimum%20Rust%20Version-1.12-brightgreen.svg)

Cargo 在“源”中找到它的包。默认源是 [crates.io](https://crates.io)。但是，您可以在 `.cargo/config` 中选择其他源：

```toml
[source.crates-io]
replace-with = 'my-awesome-registry'

[source.my-awesome-registry]
registry = 'https://github.com/my-awesome/registry-index'
```

这种配置意味着不是使用 crates.io，而是 Cargo 将查询 `my-awesome-registry` 源（在此处配置为不同的索引）。此备用源 *必须与 crates.io 索引完全相同*。
Cargo 假设替换源在这方面是精确的 1：1 镜像，并且围绕该假设设计了以下支持。

使用替换源为 crate 生成锁定文件时，原始源将编码到锁定文件中。例如，在上面的配置中，所有锁定文件仍然会提到 crates.io 作为包源的信息。
这在语义上代表了 crates.io 如何成为所有 crates 的真实来源，并且由于所有替换都具有 1：1 的对应性，因此这是坚持的。

总的来说，这意味着无论你使用何种替换源，您都可以将锁文件发送给其他任何人，并且你仍然可以获得可验证的可重现的构建！

这个工具有 [`cargo-vendor`](https://github.com/alexcrichton/cargo-vendor) 和 [`cargo-local-registry`](https://github.com/alexcrichton/cargo-local-registry),
这对于“离线构建”通常很有用。他们提前准备所有 Rust 依赖项的列表，这使您可以轻松地将它们发送到构建计算机。
