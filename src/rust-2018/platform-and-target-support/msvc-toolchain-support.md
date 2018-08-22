# MSVC toolchain 支持

![Minimum Rust version: 1.2](https://img.shields.io/badge/Minimum%20Rust%20Version-1.2-brightgreen.svg)

在 Rust 1.0 的发布中，我们只支持 Windows 上的 GNU 工具链。 随着 Rust 1.2 的发布，我们引入了对 MSVC 工具链的初始支持。
之后，随着支持的成熟，我们最终将其作为 Windows 用户的默认选择。

与 C 交互的两个问题之间的区别。如果您使用的是使用一个工具链或另一个工具链构建的库，则需要将其与相应的 Rust 工具链相匹配。 
如果您不确定，请使用 MSVC; 这是有充分理由的默认值。

要使用此功能，只需在 Windows 上使用 Rust，安装程序将默认使用它。 如果您更愿意切换到 GNU 工具链，可以使用 Rustup 进行安装：

```console
$ rustup toolchain install stable-x86_64-pc-windows-gnu
```
