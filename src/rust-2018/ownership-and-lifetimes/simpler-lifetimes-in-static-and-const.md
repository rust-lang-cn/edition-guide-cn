# 在 `static` 和 `const` 中的更简单的生命周期

![Minimum Rust version: 1.17](https://img.shields.io/badge/Minimum%20Rust%20Version-1.17-brightgreen.svg)

在以往的 Rust 中，在需要的时候，你必须更加明确的在 `static` 或者 `const` 上写明 `'static` 生命周期。

```rust
# mod foo {
const NAME: &'static str = "Ferris";
# }
# mod bar {
static NAME: &'static str = "Ferris";
# }
```

但是，在这里 `'static` 是唯一一种可能的生命周期，所以现在你可以不用再写 `'static` 了：

```rust
# mod foo {
const NAME: &str = "Ferris";
# }
# mod bar {
static NAME: &str = "Ferris";
# }
```

在某些场景下，这个可以消除很多累赘：

```rust
# mod foo {
// old
const NAMES: &'static [&'static str; 2] = &["Ferris", "Bors"];
# }
# mod bar {

// new
const NAMES: &[&str; 2] = &["Ferris", "Bors"];
# }
```
