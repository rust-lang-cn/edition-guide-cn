# `T: 'a` 结构体中的推导

![Minimum Rust version: nightly](https://img.shields.io/badge/Minimum%20Rust%20Version-nightly-red.svg)

一个注释形式为 `T：'a`，其中 `T` 可以是一个类型或另一个生命周期，被称为 *“outlives”* 要求。
注意 *“outlives”* 也意味着 `'a：'a`。

2018版在编写程序时帮助您保持流程的一种方法是，不需要在 `struct` 定义中明确注释这些 `T：'a` 的要求。
相反，这些要求将从定义中的字段推断出来。

考虑下面这个 `struct` 定义，在 Rust 2015 中：

```rust
// Rust 2015

struct Ref<'a, T: 'a> {
    field: &'a T
}

// or written with a `where` clause:

struct WhereRef<'a, T> where T: 'a {
    data: &'a T
}

// with nested references:

struct RefRef<'a, 'b: 'a, T: 'b> {
    field: &'a &'b T,
}

// using an associated type:

struct ItemRef<'a, T: Iterator>
where
    T::Item: 'a
{
    field: &'a T::Item
}
```

在 Rust 2018 中， 这种需求是可以被推导的，你可以这样写：

```rust,ignore
// Rust 2018

struct Ref<'a, T> {
    field: &'a T
}

struct WhereRef<'a, T> {
    data: &'a T
}

struct RefRef<'a, 'b, T> {
    field: &'a &'b T,
}

struct ItemRef<'a, T: Iterator> {
    field: &'a T::Item
}
```

如果您希望在某些情况下更明确，那仍然是可能的。

## 更多的细节

更多信息，查阅 [the tracking issue](https://github.com/rust-lang/rust/issues/44493) 
和 [the RFC](https://github.com/rust-lang/rfcs/pull/2093).
