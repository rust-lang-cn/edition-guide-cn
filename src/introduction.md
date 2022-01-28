# 简介

> 中文翻译注（The Chinese translation of [The Rust Edition Guide][website]）：
>
> - 👉 查看更多 <a href="https://rustwiki.org/" style="color:#97ca00;font-weight:bold;">Rust 官方文档中英文双语教程</a>，包括双语版[《Rust 程序设计语言》][book-cn]（出版书名为《Rust 权威指南》），本站还提供了 [Rust 标准库中文版][std]。
> - 《Rust 版本指南》（The Rust Edition Guide 中文版）翻译自 [The Rust Edition Guide][website]，内容已全部翻译完成，查看此书的 [Github 翻译项目和源码][home]。本文版最后更新时间：2019-05-05。
> - 本文档已加入到 [Rust 中文翻译项目组][rust-lang-cn]，主要译者：[*Sun*](https://github.com/sunhuachuang)，[Rust 中文翻译项目组][rust-lang-cn]成员。
> - <a href="https://rustwiki.org/en/edition-guide/" style="color:red;">本站支持文档中英文切换</a>，点击页面右上角语言图标可切换到相同章节的英文页面，**英文版每天都会自动同步一次官方的最新版本**。
> - 若发现当前页表达错误或帮助我们改进翻译，可点击右上角的编辑按钮打开该页对应源码文件进行编辑和修改，Rust 中文资源的开源组织发展离不开大家，感谢您的支持和帮助！
> - 注意：**此文档已较长时间没更新，内容可能比英文滞后较多**。期待您加入 [Rust 中文翻译项目组](https://github.com/rust-lang-cn)，协助我们，一起更新完善中文版，感激不尽！

欢迎来到 Rust 版本(Edition)使用指南！ "Editions" 是通过编写 Rust 代码来传达巨大改变的一种方式。

在指南中，我们将讨论：

- 什么是版本(editions)
- 每个版本什么样
- 如何将你的代码从一个版本迁移到另一个版本

请注意，标准库随每个Rust版本的增长而增长; 标准库中有**许多**添加的内容，本指南未对其进行说明。
只包含那些主要的变化，当然同时也有大量的中小型的改变也很棒。
您可能还想查看[标准库文档][std]。

[website]: https://doc.rust-lang.org/nightly/edition-guide/
[book-cn]: https://rustwiki.org/zh-CN/book/
[std]: https://rustwiki.org/zh-CN/std/
[rust-lang-cn]: https://github.com/rust-lang-cn
[home]: https://github.com/rust-lang-cn/edition-guide-cn
[std]: https://doc.rust-lang.org/std/
