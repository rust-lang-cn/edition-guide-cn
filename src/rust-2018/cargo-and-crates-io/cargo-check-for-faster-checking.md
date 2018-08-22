# `cargo check` 用于快速检查

![Minimum Rust version: 1.16](https://img.shields.io/badge/Minimum%20Rust%20Version-1.16-brightgreen.svg)

`cargo check` 是一个新的子命令，可以在很多情况下加快开发工作流程。

它有什么作用？让我们退一步说，讨论 `rustc` 如何编译代码。编译有许多“过程”，也就是说，编译器在从源代码到生成最终二进制文件的过程中有许多不同的步骤。
但是，您可以通过两个重要步骤来考虑这个过程：首先，`rustc` 执行所有安全检查，确保您的语法正确，所有这些。其次，一旦满足一切顺序，就会生成最终执行的实际二进制代码。

事实证明，第二步需要花费很多时间。而且大多数时候，这不是必要的。也就是说，当您处理一些 Rust 代码时，许多开发人员将进入这样的工作流程：

1. 写一些代码。
2. 运行 `cargo build` 以确保它编译。
3. 根据需要重复1-2。
4. 运行 `cargo test` 以确保测试通过。
5. 亲自尝试二进制文件
6. GOTO 1。

在第二步中，您实际上从未运行过您的代码。您正在寻找编译器的反馈，而不是实际运行二进制文件。 `cargo check` 正好支持这个用例：它运行所有编译器的检查，但不生成最终的二进制文件。要使用它：

```console
$ cargo check
```


在那里你通常可以 `cargo build`。 工作流现在看起来像：

1. 写一些代码。
2. 运行`cargo check`以确保它编译。
3. 根据需要重复1-2。
4. 运行`cargo test`以确保测试通过。
5. 运行`cargo build`来构建二进制文件并自己尝试
6. GOTO 1。

那么你实际获得了多少加速？与大多数相关的问题一样，答案是“它取决于”。在撰写本文时，这里有一些非科学的基准。

|  build | performance | check performance | speedup |
|--------|-------------|-------------------|---------|
| initial compile | 11s | 5.6s | 1.96x |
| second compile (no changes) | 3s | 1.9s | 1.57x |
| third compile with small change | 5.8s | 3s | 1.93x |
