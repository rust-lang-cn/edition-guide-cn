# Rust 版本指南

[![Build Status](https://travis-ci.org/rust-lang-cn/edition-guide-cn.svg?branch=master)](https://travis-ci.org/rust-lang-cn/edition-guide-cn)

> The Chinese Translation of [The Rust Edition Guide](https://github.com/rust-lang-nursery/edition-guide)
>
> 本文档按照 [**Rust 文档翻译指引**](https://rustwiki.org/zh-CN/rust-wiki/translate/rust-translation-guide.html)规范进行翻译。  
> 首次于 2018-08-22 翻译完全部内容，欢迎纠正——最后更新时间 2019.5.5  
> 近段时间将跟随英文版进行升级调整，欢迎大家踊跃参与，共同更新内容 —— 2019.5.5  

本书解释了 Rust “版本”（“edition”）的概念，即 Rust 开发中的全新的大代号。你可以在线[阅读本书中文版](https://rustwiki.org/zh-CN/edition-guide/)（[阅读英文版本](https://doc.rust-lang.org/nightly/edition-guide/)）。

[Rust]: https://www.rust-lang.org/

## 授权协议

本版本指南使用  `MIT`/`Apache2` 进行双重许可授权，就像 Rust 本身一样。请参阅此本仓库的的 `LICENSE-*` 文件了解更多信息。

## 在本地构建

你也可以自己构建本书并在本地阅读。

### 要求

构建本书需要 [mdBook]。执行下面命令进行安装:

[mdBook]: https://github.com/azerupi/mdBook

```bash
$ cargo install mdbook
```

### 构建

执行下面命令进行构建：

```bash
$ mdbook build
```

输出内容将在 `book` 子目录中，可使用浏览器中打开查看内容。

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

运行测试：

```bash
$ mdbook test
```
