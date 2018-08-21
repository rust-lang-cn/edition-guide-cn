# 用 `use` 进行导入嵌套

![Minimum Rust version: 1.25](https://img.shields.io/badge/Minimum%20Rust%20Version-1.25-brightgreen.svg)

在 Rust 中： 嵌套导入中添加了一种编写 `use` 语句的新方法。 
如果您曾编写过这样的一组导入：

```rust
use std::fs::File;
use std::io::Read;
use std::path::{Path, PathBuf};
```

可以这样写了：

```rust
# mod foo {
// on one line
use std::{fs::File, io::Read, path::{Path, PathBuf}};
# }

# mod bar {
// with some more breathing room
use std::{
    fs::File,
    io::Read,
    path::{
        Path,
        PathBuf
    }
};
# }
```

这可以减少一些重复，并使事情更清晰。
