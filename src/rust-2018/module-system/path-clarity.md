# 路径清晰化

![Minimum Rust version: nightly](https://img.shields.io/badge/Minimum%20Rust%20Version-nightly-red.svg)

对于刚接触 Rust 的人来说，模块系统通常是最困难的事情之一。 
当然，每个人掌握东西的时间都不同，但是有一个根本原因，导致了为什么对许多人来说模块系统如此混乱：
尽管模块系统有了简单而一致的定义规则，但它们给人的感觉可能不一致，甚至是违反直觉的，神秘的。

因此，Rust 2018 引入了一些新的模块系统功能，它们将*简单化*模块系统，使其更加清晰。

注意：在2018版预览期间，正在考虑的模块系统有两种变体，“统一路径(uniform paths)”变体和“锚定使用路径(anchored use paths)”变体。 
这些变化大多数是适用于两种变体的; 两个变体部分也列出了两者之间的差异。
我们鼓励使用预览2版本的用户，引入新的“统一路径”变体。Rust 2018 的稳定发布时，将只会选择其一。

要使用新的“统一路径”变体测试 Rust 2018 ，请将 `#![feature(rust_2018_preview, uniform_paths)]` 放在 `lib.rs` 或 `main.rs` 的顶部。

这是一个简短的总结：

* `extern crate` 不再需要。
* `crate` 关键字指的是当前的 crate。
* 统一路径变体： 路径在 `use` 声明和其他代码中统一工作。路径在顶级模块和子模块中统一工作。任何路径都可以以crate开头，包括 `crate` ，`super` 或 `self` ，或者具有相对于当前模块的本地名称。
* 锚定使用路径变量： `use` 声明中的路径始终以包名称开头，或者以 `crate` ， `super` 或 `self` 开头。 除了 `use` 声明之外的代码中的路径也可以从相对于当前模块的名称开始。
* `foo.rs` 和 `foo /` 子目录可以共存; 将子模块放在子目录中时不再需要`mod.rs`。

这样看起来就像是新的规则，但现在心理模型整体上大大简化了。 请阅读以获得更多详情！

## 更多的细节
让我们依次讨论每个新功能。

### 不再需要 `extern crate`
这个非常简单：您不再需要编写 `extern crate` 来将crate导入到您的项目中。 之前：

```rust,ignore
// Rust 2015

extern crate futures;

mod submodule {
    use futures::Future;
}
```

现在:

```rust,ignore
// Rust 2018

mod submodule {
    use futures::Future;
}
```

现在，要为项目添加一个新的包，你可以将它添加到你的 `Cargo.toml`，然后没有第二步。 
如果你没有使用Cargo，你必须通过 `--extern` 标志给 `rustc` 提供外部crate的位置，然后继续做其他的事前吧。

`extern crate`的另一个用途是导入宏; 那也不再需要了。 查看[宏章节](../macros/macro-changes.html)以获取更多信息。

### `crate` 指当前crate.
在 `use` 声明和其他代码中，您可以使用 `crate::` 前缀来引用当前包的根。
例如，`crate::foo::bar` 将始终引用模块 `foo` 中的名称 `bar`，来自同一个crate中的任何其他位置。

前缀 `::` 以前称为crate root或外部crate; 它现在毫无疑问的是指外部crate。
例如，`::foo::bar`总是指外部crate中 `foo` 中的 `bar`。

### 统一路径变体
与 Rust 2015 相比，Rust 2018 的统一路径变体简化并统一了路径处理。 
在 Rust 2015 中，路径在 `use` 声明中的工作方式与在其他地方的工作方式不同。 
特别地，`use` 声明中的路径总是从包根开始，而其他代码中的路径隐含地从当前模块开始。
这些差异在顶级模块中没有任何影响，这意味着在处理足够大的子模块项目之前，所有内容都会显得简单明了。

在 Rust 2018 的统一路径变体中，`use` 声明和其他代码中的路径始终以相同的方式工作，无论是在顶级模块还是在任何子模块中。 
您始终可以使用当前模块的相对路径，从外部包名称开始的路径，或以 `crate` ， `super` 或 `self` 开头的路径。

代码长这样：

```rust,ignore
// Rust 2015

extern crate futures;

use futures::Future;

mod foo {
    pub struct Bar;
}

use foo::Bar;

fn my_poll() -> futures::Poll { ... }

enum SomeEnum {
    V1(usize),
    V2(String),
}

fn func() {
    let five = std::sync::Arc::new(5);
    use SomeEnum::*;
    match ... {
        V1(i) => { ... }
        V2(s) => { ... }
    }
}
```
在 Rust 2018 中看起来完全一样，除了删除 `extern crate` 行：

```rust,ignore
// Rust 2018 (uniform paths variant)

use futures::Future;

mod foo {
    pub struct Bar;
}

use foo::Bar;

fn my_poll() -> futures::Poll { ... }

enum SomeEnum {
    V1(usize),
    V2(String),
}

fn func() {
    let five = std::sync::Arc::new(5);
    use SomeEnum::*;
    match ... {
        V1(i) => { ... }
        V2(s) => { ... }
    }
}
```

但是，使用 Rust 2018，相同的代码也可以在子模块中完全不修改：

```rust,ignore
// Rust 2018 (uniform paths variant)

mod submodule {
    use futures::Future;

    mod foo {
        pub struct Bar;
    }

    use foo::Bar;

    fn my_poll() -> futures::Poll { ... }

    enum SomeEnum {
        V1(usize),
        V2(String),
    }

    fn func() {
        let five = std::sync::Arc::new(5);
        use SomeEnum::*;
        match ... {
            V1(i) => { ... }
            V2(s) => { ... }
        }
    }
}
```

这样可以轻松地在项目中移动代码，并避免为引入多模块项目增加额外的复杂性。

如果路径不明确，例如，如果您有外部包和本地模块或具有相同名称的项目，您将收到错误，并且您需要重命名其中一个冲突名称或明确消除路径歧义。
要明确消除路径歧义，请使用 `::name` 作为外部包名，或使用 `self::name` 作为本地模块或项目。

### 锚定使用路径
在 Rust 2018 的锚定使用路径变体中，`use` 声明 *必须* 必须以包名开头，开头包括 `crate`， `self` 或 `super`。

以前代码长这样：

```rust,ignore
// Rust 2015

extern crate futures;

use futures::Future;

mod foo {
    pub struct Bar;
}

use foo::Bar;
```

现在长这样:

```rust,ignore
// Rust 2018 (anchored use paths variant)

// 'futures' is the name of a crate
use futures::Future;

mod foo {
    pub struct Bar;
}

// 'crate' means the current crate
use crate::foo::Bar;
```

此外，所有这些路径形式也可以在 `use` 声明之外使用，这消除了许多混淆的来源。 在Rust 2015中考虑以下代码：

```rust,ignore
// Rust 2015

extern crate futures;

mod submodule {
    // this works!
    use futures::Future;

    // so why doesn't this work?
    fn my_poll() -> futures::Poll { ... }
}

fn main() {
    // this works
    let five = std::sync::Arc::new(5);
}

mod submodule {
    fn function() {
        // ... so why doesn't this work
        let five = std::sync::Arc::new(5);
    }
}
```
在 `futures` 示例中，`my_poll` 函数签名不正确，因为 `submodule` 不包含名为 `futures` 的项目; 也就是说，这条路径被认为是相对的。 
但是因为 `use` 是锚定的，`use futures::` 即使单独的 `futures::` 也不行！ 
使用 `std` 它可能会更加令人困惑，因为你从来没有写过 `extern crate std;` 行。 
那么为什么它在 `main` 中工作但不在子模块中工作？ 
同样的事情：它是一个相对路径，因为它不在 `use` 声明中。 
`extern crate std;`被插入到crate root中，所以它在 `main` 中很好，但它根本不存在于子模块中。

让我们来看看这种变化如何影响：

```rust,ignore
// Rust 2018 (anchored use paths variant)

// no more `extern crate futures;`

mod submodule {
    // 'futures' is the name of a crate, so this is anchored and works
    use futures::Future;

    // 'futures' is the name of a crate, so this is anchored and works
    fn my_poll() -> futures::Poll { ... }
}

fn main() {
    // 'std' is the name of a crate, so this is anchored and works
    let five = std::sync::Arc::new(5);
}

mod submodule {
    fn function() {
        // 'std' is the name of a crate, so this is anchored and works
        let five = std::sync::Arc::new(5);
    }
}
```

更加的直截了当。


### 不再需要 `mod.rs`

在 Rust 2015 中，子模块如下：

```rust,ignore
mod foo;
```

它可以是 `foo.rs` 或者 `foo/mod.rs`。如果是一个子模块，那么它 *必须* 有一个 `foo/mod.rs`。 这样的化， `bar` 存在于子模块 `foo` 中，将表现为 `foo/bar.rs`。

在 Rust 2018 中，`mod.rs` 不再需要，`foo.rs`仅仅表示 `foo.rs`，子模块仍然是 `foo/bar.rs`。 
这消除了特殊名称，如果你在编辑器中打开了一堆文件，你可以清楚地看到它们的名字，而不是有一堆名为 `mod.rs` 的标签。
