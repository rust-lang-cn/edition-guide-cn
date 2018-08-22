# "Operator-equals" 现在实现了

![Minimum Rust version: 1.8](https://img.shields.io/badge/Minimum%20Rust%20Version-1.8-brightgreen.svg)

各种各样的 “等价操作符” 已经被各种各样的trait实现了, 比如 `+=` 和 `-=`。
举个例子：下面是一个 `+=` 操作符：

```rust
use std::ops::AddAssign;

#[derive(Debug)]
struct Count { 
    value: i32,
}

impl AddAssign for Count {
    fn add_assign(&mut self, other: Count) {
        self.value += other.value;
    }
}

fn main() {
    let mut c1 = Count { value: 1 };
    let c2 = Count { value: 5 };

    c1 += c2;

    println!("{:?}", c1);
}
```

这将打印 `Count { value: 6 }`.
