# 更加可见的修饰符

![Minimum Rust version: 1.18](https://img.shields.io/badge/Minimum%20Rust%20Version-1.18-brightgreen.svg)

您可以使用 `pub` 关键字将某些内容作为模块公共接口的一部分。 但此外，还有一些新形式：

```rust,ignore
pub(crate) struct Foo;

pub(in a::b::c) struct Bar;
```

第一种形式使 `Foo` 结构公开在整个crate中，但不是外部的。 
第二种形式是类似的，只在另一种模块 `a::b::c` 中，`Bar`是公开的。
