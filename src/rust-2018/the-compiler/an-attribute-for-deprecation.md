# 弃用属性

![Minimum Rust version: 1.9](https://img.shields.io/badge/Minimum%20Rust%20Version-1.9-brightgreen.svg)

如果您正在编写库，并且想要弃用某些内容，则可以使用 `deprecated` 属性：

```rust
#[deprecated(
    since = "0.2.1",
    note = "Please use the bar function instead"
)]
pub fn foo() {
    // ...
}
```

如果用户使用已弃用的功能，则会向您的用户发出警告：

```text
   Compiling playground v0.0.1 (file:///playground)
warning: use of deprecated item 'foo': Please use the bar function instead
  --> src/main.rs:10:5
   |
10 |     foo();
   |     ^^^
   |
   = note: #[warn(deprecated)] on by default

```

`since` 和 `note` 都是可选的。

`since` 可以是将来的; 你可以在那放任何东西，因为那儿并没有检查。
