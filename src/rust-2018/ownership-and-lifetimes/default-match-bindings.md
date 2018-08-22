# 默认匹配绑定

![Minimum Rust version: 1.26](https://img.shields.io/badge/Minimum%20Rust%20Version-1.26-brightgreen.svg)

你有过借用 `Option<T>` 然后进行匹配的经历嘛？你可能写成下面这样：

```rust,ignore
let s: &Option<String> = &Some("hello".to_string());

match s {
    Some(s) => println!("s is: {}", s),
    _ => (),
};
```

在 Rust 2015，这样写是错的，你必须这样写：

```rust,ignore
// Rust 2015

let s: &Option<String> = &Some("hello".to_string());

match s {
    &Some(ref s) => println!("s is: {}", s),
    _ => (),
};
```

在 Rust 2018， 对比之下，会自动推断 `&` 和 `ref`，然后你再这样写就没问题了。

这个并不仅仅影响 `match`，也会影响到很多地方，比如 `let` 表达式，闭包以及 `for` 循环。

## 更多的细节
模式的心理预期模型随着这种变化而略有改变，使其与语言的其他方面保持一致。
例如，在编写 `for` 循环时，您可以通过借用集合本身来迭代集合的借用内容：

```rust,ignore
let my_vec: Vec<i32> = vec![0, 1, 2];

for x in &my_vec { ... }
```

这个想法是 `＆T` 可以被理解为 *`T` 的借用视图*，所以当你迭代，匹配或以其他方式构造一个 `＆T` 时，你可以借用它的内部视图。

更正式地说，模式具有“绑定模式”，它可以是值（ `x` ），引用（ `ref x` ），也可以是可变引用（ `ref mut x` ）。 
在 Rust 2015 中，`match` 总是以by-value模式启动，并要求你在模式中显式写 `ref` 或 `ref mut` 以切换到借用模式。
在 Rust 2018 中，匹配的值的类型通知绑定模式，因此如果您使用 `Some` 变量匹配 `＆Option <String>`，您将自动进入 `ref` 模式，
为您提供借用查看内部数据。 类似地，`＆mut Option <String>` 会给你一个 `ref mut` 视图。
