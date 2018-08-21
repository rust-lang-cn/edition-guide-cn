# `?` 操作符对于早期错误的处理

![Minimum Rust version: 1.13](https://img.shields.io/badge/Minimum%20Rust%20Version-1.13-brightgreen.svg) for `Result<T, E>`

![Minimum Rust version: 1.22](https://img.shields.io/badge/Minimum%20Rust%20Version-1.22-brightgreen.svg) for `Option<T>`

Rust 已经有了一个新的操作符 `?`，它通过减少视觉干扰来使错误处理变得更加愉快。 
它通过解决了一个简单问题来做到这一点。 为了说明，假设我们有段代码，从文件中读取一些数据：

```rust
# use std::{io::{self, prelude::*}, fs::File};
fn read_username_from_file() -> Result<String, io::Error> {
    let f = File::open("username.txt");

    let mut f = match f {
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut s = String::new();

    match f.read_to_string(&mut s) {
        Ok(_) => Ok(s),
        Err(e) => Err(e),
    }
}
```

> 注意: 此代码更简单，通过只需一次调用
> [`std::fs::read_to_string`](https://doc.rust-lang.org/stable/std/fs/fn.read_to_string.html),
> 但是我们在这里手动编写所有内容以获得多个错误的示例。

此代码有两个可能失败的可能，打开文件和从中读取数据。 
如果其中任何一个无法工作，我们想从 `read_username_from_file` 返回错误。 
这样做涉及对匹配I/O操作的结果。 在这种简单的情况下，我们只是在调用堆栈中传播错误，
匹配只是样板-它每次都是以相同的模式写出来，但是这并不会为读者提供更多有用的信息。

利用 `?`，上面的代码可以写成这样：

```rust
# use std::{io::{self, prelude::*}, fs::File};
fn read_username_from_file() -> Result<String, io::Error> {
    let mut f = File::open("username.txt")?;
    let mut s = String::new();

    f.read_to_string(&mut s)?;

    Ok(s)
}
```

`?` 是我们之前写的整个匹配语句的简写。 换句话说，`?` 适用于 `Result` 值，如果它是 `Ok`，它会解开它并给出内部值。 
如果它是一个 `Err`，它将从您当前所处的函数返回。在视觉上，它更直接。 
现在我们只使用单个 `?` 而不是整个匹配语句。 "?" 字符表示我们在这里以标准方式处理错误，将它们传递给调用堆栈。

经验丰富的 Rustaceans 可能会认识到这与自 Rust `1.0` 以来一直可用的 `try！` 宏相同。 
事实上，他们是一样的。以前，`read_username_from_file` 可能是这样实现的：

```rust
# use std::{io::{self, prelude::*}, fs::File};
fn read_username_from_file() -> Result<String, io::Error> {
    let mut f = try!(File::open("username.txt"));
    let mut s = String::new();

    try!(f.read_to_string(&mut s));

    Ok(s)
}
```

那么为什么在我们已经拥有宏了，还要扩展语言呢？ 原因有很多。 首先，`try!` 已被证明非常有用，并且常用于惯用的 Rust中。
因为经常被使用，所以我们认为这是值得拥有的语法糖。 这种演化是强大的宏系统的巨大优势之一： 语言语法的推测性扩展可以在不修改语言本身的情况下进行原型化和迭代，
反过来，那些表现出特别有用的宏，可以用来指导制定缺少的语言特征。这种演变，从 `try!` 到 `?` 就是一个很好的例子。

其中一个原因 `try!` 需要一个更甜的语法，当连续使用 `try!` 的多次调用时，这是非常没有吸引力的。 考虑：

```rust,ignore
try!(try!(try!(foo()).bar()).baz())
```

作为对比：

```rust,ignore
foo()?.bar()?.baz()?
```

第一个是非常难以直观阅读的，每个错误处理层都在表达式前加上一个额外的 `try!` 调用。
这会引起过度关注琐碎的错误传播，模糊主代码执行流程，在本例中调用 `foo` ， `bar` 和 `baz`。 
这种与错误处理链接的方法发生在构建器模式等情况下。

最后，专用语法将使以后更容易生成专门针对 `?` 定制的更好的错误消息，而一般来说很难为宏扩展代码产生好的错误。

您可以将 `?` 与 `Result <T，E>` 一起使用，也可以使用 `Option <T>`。 在这种情况下，`?` 将为 `Some(T)` 返回一个值，并为 `None` 返回 `None`。
一个当前的限制是你不能在同一个函数中反复使用 `?`，因为返回类型需要匹配你使用的类型 `?`。 将来，这种限制将是有限的。
