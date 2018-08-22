# `loop` 可以 break 并携带返回值

![Minimum Rust version: 1.19](https://img.shields.io/badge/Minimum%20Rust%20Version-1.19-brightgreen.svg)

`loop` 可以 break 并携带返回值

```rust
// old code
let x;

loop {
    x = 7;
    break;
}

// new code
let x = loop { break 7; };
```

Rust 传统上将自己定位为“面向表达式的语言”，也就是说，大多数事物都是评估价值而不是陈述表达。 
`loop` 以这种方式突然变得奇怪，因为它之前是一个声明。

现在，这只适用于 `loop`，而不适用于 `while` 或 `for`。 目前尚不清楚，但我们可能会将此添加到未来。
