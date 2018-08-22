# Rustdoc uses CommonMark

![Minimum Rust version: 1.25](https://img.shields.io/badge/Minimum%20Rust%20Version-1.25-brightgreen.svg) for support by default

![Minimum Rust version: 1.23](https://img.shields.io/badge/Minimum%20Rust%20Version-1.23-red.svg) for support via a flag

Rustdoc 允许您在文档注释中，使用 Markdown 编写。 在 Rust 1.0 中，我们使用了 `hoedown` markdown实现，用 C 编写。
Markdown 更像是一个想法的实现系列，因此 `hoedown` 有自己的方言，就像许多解析器一样。
[CommonMark project](https://commonmark.org/) 试图定义更严格的Markdown版本，现在，Rustdoc默认使用它。

从 Rust 1.23 开始，我们仍然默认为 `hoedown`，但你可以通过标志 `--enable-commonmark` 启用 Commonmark。 今天，我们只支持 CommonMark。
