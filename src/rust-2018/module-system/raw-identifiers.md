# 原始标识符

![Minimum Rust version: nightly](https://img.shields.io/badge/Minimum%20Rust%20Version-nightly-red.svg)

与许多编程语言一样，Rust 具有“关键字”的概念。
这些标识符对语言有意义，因此你不能在变量名，函数名和其他位置使用它们。
原始标识符允许你使用通常不允许的关键字。

举个例子，`match` 是一个关键字。如果你试图编译这个方法：

```rust,ignore
fn match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}
```

你将得到以下错误：

```text
error: expected identifier, found keyword `match`
 --> src/main.rs:4:4
  |
4 | fn match(needle: &str, haystack: &str) -> bool {
  |    ^^^^^ expected identifier, found keyword
```

你可以使用原始标识符来实现:

```rust,ignore
#![feature(rust_2018_preview)]
#![feature(raw_identifiers)]

fn r#match(needle: &str, haystack: &str) -> bool {
    haystack.contains(needle)
}

fn main() {
    assert!(r#match("foo", "foobar"));
}
```

注意 `r#` 不仅在定义的时候有，在调用的时候也得有。

## 更多的细节
此功能还是有一些用处的，但主要动机是版本间的情况。
例如，`try` 不是2015版的关键字，而是2018版的。
因此，如果你有一个用 Rust 2015 编写并具有 `try` 函数的库，要在 Rust 2018 中调用它，你需要使用原始标识符。

## 新的关键字

2018 中新定义的关键字:

### `async` and `await`

[RFC 2394]: https://github.com/rust-lang/rfcs/blob/master/text/2394-async_await.md#final-syntax-for-the-await-expression

这里, 保留 `async` 用来实现 `async fn` 或者 `async ||` 闭包 和 `async { .. }` 块。
同时， 保留 `await` 用来保持 `await!(expr)` 这种语法是一个开放的选项。有关详细信息，请参阅 [RFC 2394]。

### `try`

[RFC 2388]: https://github.com/rust-lang/rfcs/pull/2388

`do catch { .. }` 块已经被重命名为 `try { .. }` 并已经得到支持, 关键字 `try` 在2018版中将被保留. 有关详细信息，请参阅 [RFC 2388]。
