# `..=` 包含取值范围

![Minimum Rust version: 1.26](https://img.shields.io/badge/Minimum%20Rust%20Version-1.26-brightgreen.svg)

在以前的 Rust 1.0 中，我们像下面这样写一个取值范围：

```
for i in 1..3 {
    println!("i: {}", i);
}
```

这将会打印 `i: 1` 然后是 `i: 2`，现在你可以这么写：

```rust
for i in 1..=3 {
    println!("i: {}", i);
}
```

这也会打印 `i: 1` 然后是 `i: 2`，最后是 `i: 3`; 最后的也将会包含在范围取值中，
当你需要包含取值范围的时候，这会十分有用。下面是一个令人惊奇的例子：

```rust
fn takes_u8(x: u8) {
    // ...
}

fn main() {
    for i in 0..256 {
        println!("i: {}", i);
        takes_u8(i);
    }
}
```

这个程序做了什么？回答是：没有任何东西。编译器将会报出警告：

```text
warning: literal out of range for u8
 --> src/main.rs:6:17
  |
6 |     for i in 0..256 {
  |                 ^^^
  |
  = note: #[warn(overflowing_literals)] on by default
```

这是正常的，因为 `i` 作为一个 `u8`，256超出了范围，这就导致效果和 `for i in 0..0` 一样, 这段代码将执行0次。

但是，我们现在可以这么写：

```rust
fn takes_u8(x: u8) {
    // ...
}

fn main() {
    for i in 0..=255 {
        println!("i: {}", i);
        takes_u8(i);
    }
}
```

这个将会执行256次你需要执行的内容。
