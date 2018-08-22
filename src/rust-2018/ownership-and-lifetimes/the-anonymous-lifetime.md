# `'_` — 匿名生命周期

![Minimum Rust version: nightly](https://img.shields.io/badge/Minimum%20Rust%20Version-nightly-red.svg)

Rust 2018 允许你明确标记生命周期被省略的地方，对于此省略可能不清楚的类型。
要做到这一点，你可以使用特殊的生命周期`'_`，就像你可以用语法 `let x：_ = ..;`明确标记一个类型一样。

要我们说的话，无论出于什么原因，我们在 `&'a str` 周围有一个简单的封装：

```rust
struct StrWrap<'a>(&'a str);
```

在 Rust 2015，你可能写成这样：

```rust
// Rust 2015

use std::fmt;

# struct StrWrap<'a>(&'a str);

fn make_wrapper(string: &str) -> StrWrap {
    StrWrap(string)
}

impl<'a> fmt::Debug for StrWrap<'a> {
    fn fmt(&self, fmt: &mut fmt::Formatter) -> fmt::Result {
        fmt.write_str(self.0)
    }
}
```

在 Rust 2018，你可以替换成这样：

```rust
#![feature(rust_2018_preview)]

# use std::fmt;
# struct StrWrap<'a>(&'a str);

// Rust 2018

fn make_wrapper(string: &str) -> StrWrap<'_> {
    StrWrap(string)
}

impl fmt::Debug for StrWrap<'_> {
    fn fmt(&self, fmt: &mut fmt::Formatter<'_>) -> fmt::Result {
        fmt.write_str(self.0)
    }
}
```

## 更多的细节
在上面的 Rust 2015 片段中，我们使用了 `-> StrWrap`。 但是，除非你看一下 `StrWrap` 的定义，
否则返回的值实际上是借用了什么并不清楚。 因此，从 Rust 2018 开始，不推荐使用它来省去非引用类型
的生命周期参数（除了 `＆` 和 `＆mut` 之外的类型）。 相反，你之前写过 `-> StrWrap` 的地方，
你现在应该写 `-> StrWrap <'_>`，明确说明正在进行借用。

`'_`究竟是什么意思？这取决于具体情况！在输出上下文中，与 `make_wrapper` 的返回类型一样，
它指的是所有“输出”位置的单个生命周期。在输入上下文中，为每个“输入位置”生成新的生命周期。
更具体地说，要了解输入上下文，请考虑以下示例：

```rust
// Rust 2015

struct Foo<'a, 'b: 'a> {
    field: &'a &'b str,
}

impl<'a, 'b: 'a> Foo<'a, 'b> {
    // some methods...
}
```

我们可以重写为：

```rust
#![feature(rust_2018_preview)]

# struct Foo<'a, 'b: 'a> {
#     field: &'a &'b str,
# }

// Rust 2018

impl Foo<'_, '_> {
    // some methods...
}
```

这是相同的，因为对于每个`'_`，会产生一个新的生命周期。最后，必须坚持结构所需的关系 `'a： 'b`。

更多的细节，参阅[tracking issue on In-band lifetime bindings](https://github.com/rust-lang/rust/issues/44524)。
