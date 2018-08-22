# 字段初始化简写

![Minimum Rust version: 1.17](https://img.shields.io/badge/Minimum%20Rust%20Version-1.17-brightgreen.svg)

在以往的 Rust 中，当初始化一个结构体的时候，总是需要完全按照 `key: value` 对的写法：

```rust
struct Point {
    x: i32,
    y: i32,
}

let a = 5;
let b = 6;

let p = Point {
    x: a,
    y: b,
};
```

但是，这些字段通常会是相同的名字，所以你可以把它写成这样：

```rust,ignore
let p = Point {
    x: x,
    y: y,
};
```

现在，如果变量名和结构体字段名相同，可以省略写成这样：

```rust
struct Point {
    x: i32,
    y: i32,
}

let x = 5;
let y = 6;

// new
let p = Point {
    x,
    y,
};
```
