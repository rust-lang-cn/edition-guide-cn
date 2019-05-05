# Rust 版本指南

[![Build Status](https://travis-ci.org/rust-lang-cn/edition-guide-cn.svg?branch=master)](https://travis-ci.org/rust-lang-cn/edition-guide-cn)

> The Chinese Translation of [The Rust Edition Guide](https://github.com/rust-lang-nursery/edition-guide)
>
> 本文档按照 [**Rust 文档翻译指引**](https://rustwiki.org/zh-CN/rust-wiki/translate/rust-translation-guide.html)规范进行翻译。  
> 首次于 2018-08-22 翻译完全部内容，欢迎纠正——最后更新时间 2019.5.5  
> 近段时间将跟随英文版进行升级调整，欢迎大家踊跃参与，共同更新内容 —— 2019.5.5  

本书解释了 Rust “版本”（“edition”）的概念，即 Rust 开发中的全新的大代号。你可以在线[阅读本书中文版](https://rustwiki.org/zh-CN/edition-guide/)（[阅读英文版本](https://doc.rust-lang.org/nightly/edition-guide/)）。

[Rust]: https://www.rust-lang.org/

## License

The Edition Guide is dual licensed under `MIT`/`Apache2`, just like Rust itself.
See the `LICENSE-*` files in this repository for more details.

## Building locally

You can also build the book and read it locally if you'd like.

### Requirements

Building the book requires [mdBook]. To get it:

[mdBook]: https://github.com/azerupi/mdBook

```bash
$ cargo install mdbook
```

### Building

To build the book, do this:

```bash
$ mdbook build
```

The output will be in the `book` subdirectory. To check it out, open it in
your web browser.

_Firefox:_

```shell
$ firefox book/index.html                       # Linux
$ open -a "Firefox" book/index.html             # OS X
$ Start-Process "firefox.exe" .\book\index.html # Windows (PowerShell)
$ start firefox.exe .\book\index.html           # Windows (Cmd)
```

_Chrome:_

```shell
$ google-chrome book/index.html                 # Linux
$ open -a "Google Chrome" book/index.html       # OS X
$ Start-Process "chrome.exe" .\book\index.html  # Windows (PowerShell)
$ start chrome.exe .\book\index.html            # Windows (Cmd)
```

To run the tests:

```bash
$ mdbook test
```
