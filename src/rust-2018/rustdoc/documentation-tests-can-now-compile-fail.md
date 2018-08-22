# 文档测试现在可以编译失败

![Minimum Rust version: 1.22](https://img.shields.io/badge/Minimum%20Rust%20Version-1.22-brightgreen.svg)

现在可以创建 `compile-fail` 测试在 Rustdoc 中，如下:

```
/// ```compile_fail
/// let x = 5;
/// x += 2; // shouldn't compile!
/// ```
# fn foo() {}
```

请注意，这些类型的测试可能比其他测试更脆弱，因为 Rust 的添加可能导致代码在以前不会在编译时编译。
考虑使用 `?` 的第一个版本，例如：使用 `?` 的代码将无法在 Rust 1.21 上编译，但在 Rust 1.22 上成功编译，导致您的测试套件开始失败。
