# `?` 在 `main` 和 tests 中
![Minimum Rust version: 1.26](https://img.shields.io/badge/Minimum%20Rust%20Version-1.26-brightgreen.svg)

Rust的错误处理围绕返回 `Result <T，E>` 并使用 `?` 传播错误。 
对于那些编写许多小程序并且希望进行许多测试的人来说，更关注于那些复杂的入口，例如`main`和`#[test]`中的错误处理。

举个例子，你将尝试这样写：

```rust,ignore
use std::fs::File;

fn main() {
    let f = File::open("bar.txt")?;
}
```

因为 `?` 通过处理 `Result` 并提前返回函数来工作，所以上面的代码不起作用，并且导致以下错误：

```rust,ignore
error[E0277]: the `?` operator can only be used in a function that returns `Result`
              or `Option` (or another type that implements `std::ops::Try`)
 --> src/main.rs:5:13
  |
5 |     let f = File::open("bar.txt")?;
  |             ^^^^^^^^^^^^^^^^^^^^^^ cannot use the `?` operator in a function that returns `()`
  |
  = help: the trait `std::ops::Try` is not implemented for `()`
  = note: required by `std::ops::Try::from_error`
```

在 Rust 2015 中，处理这种问题，需要这样：

```rust
// Rust 2015

# use std::process;
# use std::error::Error;

fn run() -> Result<(), Box<Error>> {
    // real logic..
    Ok(())
}

fn main() {
    if let Err(e) = run() {
        println!("Application error: {}", e);
        process::exit(1);
    }
}
```

但是，在这种情况下，`run` 函数具有所有有趣的逻辑，而 `main` 只是样板。 问题更糟糕的是 `#[test]`，因为它们往往会有更多这种情况。

在 Rust 2018 中，你可以使得你的 `#[test]` 和 `main` 函数返回一个 `Result`：

```rust,no_run
// Rust 2018

use std::fs::File;

fn main() -> Result<(), std::io::Error> {
    let f = File::open("bar.txt")?;

    Ok(())
}
```

在这种情况下，如果说文件不存在并且某处有一个 `Err(err)`，那么 `main` 将以错误代码（不是`0`）退出并打印出 `Debug` 表示 `err`。

## 更多的细节

使 ` - > Result <..>` 在 `main` 和 `#[test]` 的上下文中工作并不神奇。
它全部由 `Termination` 特征支持，所有有效的返回类型的 `main` 和测试函数必须实现。 特征定义为：

```rust
pub trait Termination {
    fn report(self) -> i32;
}
```

在为应用程序设置入口点时，编译器将使用此特征并在您编写的 `main` 函数的 `Result` 上调用 `.report()`。

`Result` 和 `()` 的这个特性的两个简化示例实现是：

```rust
# #![feature(process_exitcode_placeholder, termination_trait_lib)]
# use std::process::ExitCode;
# use std::fmt;
#
# pub trait Termination { fn report(self) -> i32; }

impl Termination for () {
    fn report(self) -> i32 {
        # use std::process::Termination;
        ExitCode::SUCCESS.report()
    }
}

impl<E: fmt::Debug> Termination for Result<(), E> {
    fn report(self) -> i32 {
        match self {
            Ok(()) => ().report(),
            Err(err) => {
                eprintln!("Error: {:?}", err);
                # use std::process::Termination;
                ExitCode::FAILURE.report()
            }
        }
    }
}
```

正如您在 `()` 中看到的那样，只返回成功代码。 
在 `Result` 的情况下，成功的话交给 `()` 来执行，错误的话，交给 `Err(..)`，打印出错误消息并退出代码。

要了解有关更细节的信息，请参阅[跟踪问题](https://github.com/rust-lang/rust/issues/43301) 或者 [the RFC](https://github.com/rust-lang/rfcs/blob/master/text/1937-ques-in-main.md).
