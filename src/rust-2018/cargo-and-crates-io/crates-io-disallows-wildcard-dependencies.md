# Crates.io 不允许使用通配符

![Minimum Rust version: 1.6](https://img.shields.io/badge/Minimum%20Rust%20Version-1.6-brightgreen.svg)

Crates.io 不允许您上传具有通配符依赖关系的包。 换句话说，这些：

```toml
[dependencies]
regex = "*"
```

通配符依赖性意味着您可以使用任何可能的依赖项版本。 这极不可能是真实的，并且会在生态系统中造成不必要的破坏。

相反，取决于版本范围。例如，`^`是默认值，因此您可以使用：

```toml
[dependencies]
regex = "1.0.0"
```

相应的， `>`, `<=`, 和所有其他的非`*`范围都是可以的。
