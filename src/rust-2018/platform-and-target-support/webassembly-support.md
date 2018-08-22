# WebAssembly support

![Minimum Rust version: 1.14](https://img.shields.io/badge/Minimum%20Rust%20Version-1.14-brightgreen.svg) for `emscripten`

![Minimum Rust version: nightly](https://img.shields.io/badge/Minimum%20Rust%20Version-nightly-red.svg) for `wasm32-unknown-unknown`

Rust 已经有了对 [WebAssembly](https://webassembly.org/) 的支持，这意味着你可以在浏览器客户端中运行 Rust 代码。

在 Rust 1.14 中，我们通过 [emscripten](http://kripken.github.io/emscripten-site/index.html) 获得了支持。
安装它后，您可以编写 Rust 代码并生成 [asm.js](http://asmjs.org/)（the precusor to wasm）或 WebAssembly。

以下是使用此支持的示例：

```console
$ rustup target add wasm32-unknown-emscripten
$ echo 'fn main() { println!("Hello, Emscripten!"); }' > hello.rs
$ rustc --target=wasm32-unknown-emscripten hello.rs
$ node hello.js
```

然而，与此同时，Rust 也增加了自己的支持，独立于 Emscripten。 这被称为 “未知目标”，因为它不是 `wasm32-unknown-emscripten`，而是 `wasm32-unknown-unknown`。
这将是它准备好后首选使用的目标，但就目前而言，它实际上只能在 nightly版 中得到很好的支持。

