# `dyn Trait` trait 对象

![Minimum Rust version: 1.27](https://img.shields.io/badge/Minimum%20Rust%20Version-1.27-brightgreen.svg)

`dyn Trait` 是使用 trait 对象的新语法，简而言之：

* `Box<Trait>` becomes `Box<dyn Trait>`
* `&Trait` and `&mut Trait` become `&dyn Trait` and `&mut dyn Trait`

因此，代码中:

```rust
trait Trait {}

impl Trait for i32 {}

// old
fn function1() -> Box<Trait> {
# unimplemented!()
}

// new
fn function2() -> Box<dyn Trait> {
# unimplemented!()
}
```

这就是了！

## 更多细节
仅仅使用特征对象的特征名其实是个糟糕的决定。目前的语法通常含糊不清，即使对于老一批人来来说也是如此，
而且竟然没有它的替代品使用的更频繁，有时速度较慢，而且当其替代品可以使用时，它将根本不会被使用。

此外，随着 `impl Trait` 的到来，`impl Trait` vs `dyn Trait` 比 `impl Trait` vs `Trait` 更好更对称。
`impl Trait`将在下一节进一步解释。

因此，在新版本中，选择使用 trait 对象时，你应该选 `dyn Trait` 而不是 `Trait`。
