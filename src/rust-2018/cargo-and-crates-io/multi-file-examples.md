# 多个演示用例

![Minimum Rust version: 1.22](https://img.shields.io/badge/Minimum%20Rust%20Version-1.22-brightgreen.svg)

Cargo有一个 `examples` 功能，用于向人们展示如何使用您的包裹。通过将单个文件放在顶级 `examples` 目录中，您可以创建多个示例。

但是如果你的例子对于单个文件来说太大了怎么办？Cargo支持在 `examples` 中添加子目录，并在其中查找 `main.rs` 来构建示例。它看起来像这样：

```text
my-package
 └──src
     └── lib.rs // code here
 └──examples 
     └── simple-example.rs // a single-file example
     └── complex-example
        └── helper.rs
        └── main.rs // a more complex example that also uses `helper` as a submodule
```
