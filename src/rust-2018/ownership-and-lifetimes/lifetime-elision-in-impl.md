# 在 impl 中省略生命周期

![Minimum Rust version: nightly](https://img.shields.io/badge/Minimum%20Rust%20Version-nightly-red.svg)

在编写 `impl` 时，你可以提及生命周期而不将它们绑定在参数列表中。

在 Rust 2015 中：

```rust,ignore
impl<'a> Iterator for MyIter<'a> { ... }
impl<'a, 'b> SomeTrait<'a> for SomeType<'a, 'b> { ... }
```

在 Rust 2018 中：

```rust,ignore
impl Iterator for MyIter<'iter> { ... }
impl SomeTrait<'tcx> for SomeType<'tcx, 'gcx> { ... }
```
