# 全局分配符

![Minimum Rust version: 1.28](https://img.shields.io/badge/Minimum%20Rust%20Version-1.28-brightgreen.svg)

分配器是 Rust 中的程序在运行时从系统获取内存的方式。以前，Rust 不允许改变获取内存的方式，这阻止了一些用例。
在某些平台上，这意味着在其他平台上使用 jemalloc，系统分配器，但用户无法控制此关键组件。 
在1.28中，`#[global_allocator]` 属性现在是稳定的，它允许 Rust 程序将它们的分配器设置为系统分配器，并通过实现 `GlobalAlloc` 特性来定义新的分配器。

某些平台上 Rust 程序的默认分配器是 jemalloc。标准库现在提供了系统分配器的句柄，可以在需要时通过声明静态并使用 `#[global_allocator]` 属性标记它来切换到系统分配器。

```rust
use std::alloc::System;

#[global_allocator]
static GLOBAL: System = System;

fn main() {
    let mut v = Vec::new();
    // This will allocate memory using the system allocator.
    v.push(1);
}
```

但是，有时您希望为给定的应用程序域定义自定义分配器。通过实现 `GlobalAlloc` 特性，这也相对容易。
您可以在 [the documentation](https://doc.rust-lang.org/std/alloc/trait.GlobalAlloc.html)。

