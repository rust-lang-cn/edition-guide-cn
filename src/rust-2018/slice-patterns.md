# 切片模式

![Minimum Rust version: 1.26](https://img.shields.io/badge/Minimum%20Rust%20Version-1.26-brightgreen.svg)

你有没有试过，用模式匹配去匹配切片的内容和结构？ Rust 2018 将让你做到这一点。

例如，我们想要接受一个名单列表并回复问候语。使用切片模式，我们可以用以下方式轻松完成：

```rust
fn main() {
    greet(&[]);
    // output: Bummer, there's no one here :(
    greet(&["Alan"]);
    // output: Hey, there Alan! You seem to be alone.
    greet(&["Joan", "Hugh"]);
    // output: Hello, Joan and Hugh. Nice to see you are at least 2!
    greet(&["John", "Peter", "Stewart"]);
    // output: Hey everyone, we seem to be 3 here today.
}

fn greet(people: &[&str]) {
    match people {
        [] => println!("Bummer, there's no one here :("),
        [only_one] => println!("Hey, there {}! You seem to be alone.", only_one),
        [first, second] => println!(
            "Hello, {} and {}. Nice to see you are at least 2!",
            first, second
        ),
        _ => println!("Hey everyone, we seem to be {} here today.", people.len()),
    }
}
```

现在，你不必检查长度了。

你也可以匹配 array 如下：

```rust
let arr = [1, 2, 3];

assert_eq!("ends with 3", match arr {
    [_, _, 3] => "ends with 3",
    [a, b, c] => "ends with something else",
});
```

## 更多的细节

### 穷举模式
在第一个例子中，注意匹配的 `_ => ...`。 如果开始匹配，那么将会匹配一切情况，所以有“穷尽所有模式”的处理方式。
如果我们忘记使用 `_ => ...` 或者 `identifier => ...` 模式，我们会得到如下的错误提醒：

```ignore
error[E0004]: non-exhaustive patterns: `&[_, _, _]` not covered
```

如果我们再增加一项，我们将得到如下：

```ignore
error[E0004]: non-exhaustive patterns: `&[_, _, _, _]` not covered
```

如此。

### 数组和精确的长度
在第二个例子中，数组是有固定长度的，我们需要匹配所有长度项，如果只匹配2，4项的话，会报错：

```ignore
error[E0527]: pattern requires 2 elements but array has 3
```

和

```ignore
error[E0527]: pattern requires 4 elements but array has 3
```

### 管道中

[the tracking issue]: https://github.com/rust-lang/rust/issues/23121

在切片模式方面，计划采用更先进的形式，但尚未稳定。要了解更多信息，请跟踪 [the tracking issue]。
