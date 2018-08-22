# 改进报错信息

![Minimum Rust version: 1.12](https://img.shields.io/badge/Minimum%20Rust%20Version-1.12-brightgreen.svg)

我们一直致力于改进错误，几乎每个 Rust 版本都没有什么改进，但在 Rust 1.12 中，创建了错误消息系统的重大改进。

例如，这里有一些产生错误的代码：

```rust,ignore
fn main() {
    let mut x = 5;

    let y = &x;

    x += 1;
}
```

这是 Rust 1.11：

```text
foo.rs:6:5: 6:11 error: cannot assign to `x` because it is borrowed [E0506]
foo.rs:6     x += 1;
             ^~~~~~
foo.rs:4:14: 4:15 note: borrow of `x` occurs here
foo.rs:4     let y = &x;
                      ^
foo.rs:6:5: 6:11 help: run `rustc --explain E0506` to see a detailed explanation
```

这是 Rust 1.28：

```text
error[E0506]: cannot assign to `x` because it is borrowed
 --> foo.rs:6:5
  |
4 |     let y = &x;
  |              - borrow of `x` occurs here
5 |
6 |     x += 1;
  |     ^^^^^^ assignment to borrowed `x` occurs here

error: aborting due to previous error
```

这个错误并没有太大的不同，但展示了格式的变化。它在上下文中显示您的代码，而不是仅显示行本身的文本。
