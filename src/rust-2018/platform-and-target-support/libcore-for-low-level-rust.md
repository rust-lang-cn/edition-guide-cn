# libcore 给低层 Rust 使用

![Minimum Rust version: 1.6](https://img.shields.io/badge/Minimum%20Rust%20Version-1.6-brightgreen.svg)

Rust 的标准库是双层的： 有一个小的核心库，`libcore`，以及构建在它之上的完整标准库 `libstd`。 
`libcore` 完全与平台无关，只需要定义少量外部符号。Rust 的 `libstd` 建立在 `libcore` 之上，增加了对内存分配和I/O等内容的支持。
在嵌入式空间中使用 Rust 的应用程序以及编写操作系统的应用程序通常只使用 `libcore` 来避免 `libstd`。

另外需要注意的是，虽然今天支持使用 `libcore` 构建 *库*，但构建完整的应用程序还不稳定。

要使用 `libcore`，请将此标志添加到你的 crate 根目录：

```rust,ignore
#![no_std]
```

这将删除标准库，并将 `core` crate 带入您的命名空间以供使用：

```rust,ignore
#![no_std]

use core::cell::Cell;
```

你可以找到 `libcore` 的文档 [here](https://doc.rust-lang.org/core/)。
