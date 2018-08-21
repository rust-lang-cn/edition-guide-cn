# 迁移你的代码到新版本
新版本可能会改变您编写 Rust 的方式 - 它们会添加新的语法，语言和库功能，但也会删除功能。
例如，`try`，`async`和`await`是 Rust 2018 中的关键字，但不在 Rust 2015中。
尽管如此，我们试图尽可能顺利地迁移到新版本。
如果很难将您的 crates 升级到新版本，那么这可能是一个 bug 。如果您遇到困难，那么应该向 Rust 提交一个 bug。

版本之间的迁移是围绕编译器标签(lints)构建的。从根本上说，这个过程是这样的：

* 打开 lints 以指示代码与新版本不兼容的位置
* 在没有警告的情况下编译代码。
* 选择加入新版本，代码应该编译。
*（可选）在新版本中启用有关 *idiomatic* 代码的 lints。

幸运的是，我们一直致力于 Cargo 帮助完成这一过程，最终推出了一个新的内置子命令 `cargo fix`。 
它可以从编译器中获取建议并自动重新编写代码以符合新功能和习惯用法，从而大大减少手动修复所需的警告数量！

> `cargo fix` 仍然很早期，而且非常重要。但它已经适用于基础部分！我们正在努力使其变得更好，更强大，但暂时不必使用。

## 预览期
在发布版本之前，它将有一个“预览”阶段，让您可以在发布之前在 nightly 的 Rust 中试用新版本。
目前 Rust 2018 正处于预览阶段，因此，您需要采取额外的步骤来选择加入。
将此功能标志添加到您的`lib.rs`或`main.rs`以及任何示例中。如果你有一个项目的`examples`目录：

```rust
#![feature(rust_2018_preview)]
```

这将启用 [特性状态][status] 页面中列出的不稳定功能。请注意，某些功能需要最小的 Rust 2018，这些功能需要 Cargo.toml 拥有更改权限才能启用（在下面的部分中描述）。 
另请注意，在预览可用期间，我们可能会继续使用此标志来添加/启用新功能！

对于 Rust 2018 预览版2中，我们还测试了[新模块路径变体](../rust-2018/module-system/path-clarity.html)，“统一路径”，我们想要获得进一步测试和反馈。 
请尝试将以下内容添加到`lib.rs`或`main.rs`中：

```rust
#![feature(rust_2018_preview, uniform_paths)]
```

Rust 2018 的发布时候将会选择两个模块路径变体中的一个，并放弃另一个。
The release of Rust 2018 will stabilize one of the two module path variants and drop the other.

[status]: ../unstable-feature-status.html

## 修复版本兼容性警告

接下来是启用有关与新 2018 版本 不兼容的代码的编译器警告。 这是方便 `cargo fix` 这个工具进入图片的地方。 要为项目启用兼容性lints，请运行：

```shell
$ cargo fix --edition
```

如果 nightly 不是你的默认选择的话，你需要运行下面的这个命令：

```shell
$ cargo +nightly fix --edition
```

这将指示 Cargo 编译项目中的所有目标（库，二进制文件，测试等），同时启用所有 Cargo 功能并为2018版本做好准备。 
Cargo 可能会自动修复一些文件，并在其发生时通知您。 请注意，这不会启用任何新的 Rust 2018 功能; 它只能确保您的代码与 Rust 2018 兼容。

如果Cargo无法自动修复所有内容，它将打印出剩余的警告。继续运行上述命令，直到所有警告都已解决。

你可以获取更多 `cargo fix` 信息，运行：

```shell
$ cargo fix --help
```

## 切换到下一个版本
一旦您对这些更改感到满意，就可以使用新版本了。 将其添加到您的 `Cargo.toml`：

```toml
cargo-features = ["edition"]

[package]
edition = '2018'
```

那个 `cargo-features` 行应该排在最前面; `edition` 进入 `[package]` 部分。 
如上所述，现在这是 Cargo 的 nightly 特征，因此您需要启用它才能使其工作。

此时，您的项目应该使用常规的`cargo build`进行编译。 如果没有，这是一个错误！ 请[提交问题][issue]。

[issue]: https://github.com/rust-lang/rust/issues/new

## 在新版本中编写惯用代码
你的 crate 现在已经进入了2018版的 Rust，恭喜！ 回想一下，Rust 中的 Editions 表示随着时间的推移，习惯用语的转变。 
虽然很多旧代码将继续编译，但今天可能会用不同的习惯用语编写。

您可以采取的可选的后续步骤是将代码更新为新版本中的惯用语。 
这是通过一组不同的“习惯用语lints”完成的。 就像之前我们使用 `cargo fix` 来推动这个过程一样：

```shell
$ cargo fix --edition-idioms
```

与之前一样，这是一个 *简单* 的步骤。 
在这里 `cargo fix` 将自动修复任何可能的lint，所以你只会得到 `cargo fix` 无法修复的情况下的警告。 
如果您发现难以完成警告，那就是一个错误！

一旦你用这个命令警告没有了，你就可以继续了。


> `--edition-idioms` 标志仅适用于“当前 crate”，如果你想在工作空间运行它是必要的，使用 `RUSTFLAGS` 的解决方法，以便在所有工作区中执行它。
>
> ```shell
> $ RUSTFLAGS='-Wrust_2018_idioms' cargo fix --all
> ```

享受新版本吧！
