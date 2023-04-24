# libc - 提供 C 标准库的rust侧FFI

[![GHA Status]][GitHub Actions] [![Cirrus CI Status]][Cirrus CI] [![Latest Version]][crates.io] [![Documentation]][docs.rs] ![License]

`libc`提供了所有必要的定义，以便在Rust轻松与C
代码（或 "类C "代码）在Rust支持的平台上的调用。这包括
包括类型定义（如`c_int`），常量（如`EINVAL`）以及
函数（例如：`malloc'）。

libc将各类平台中C库的类型、函数和常量导出在根目录下，所以所有项目都可以作为`libc::foo'访问这些API。
所有导出的API的类型和值都与 libc 编译的平台一致。

关于这个库的设计，更详细的信息可以在这里找到
[associated RFC][rfc].

[rfc]: https://github.com/rust-lang/rfcs/blob/master/text/1291-promote-libc.md

## 用法
### 使用Openharmony编译框架
在你的"BUILD.gn"中使用deps字段添加对libc crate的依赖，例如：

```BUILD.gn
ohos_rust_shared_library("foo") {
  source = [ "src/lib.rs" ]
  deps = [ "//third_party/rust/crates/libc:lib" ]
}
```

### 使用Cargo
在你的 "Cargo.toml "中添加以下内容：

```toml
[dependencies]
libc = "0.2"
```

## 特点

* `std`：默认情况下，`libc`链接到标准库。禁用这个
  来消除这一依赖关系，并能够在`#！[no_std]`中使用`libc`。
  crates。

* `extra_traits`: `libc`中实现的所有`结构'都是`Copy`和`Clone`。
  这个特性衍生出了`Debug`、`Eq`、`Hash`和`PartialEq`。

* `const-extern-fn`： 将一些`extern fn`s改为`const extern fn`s。
  如果你使用Rust >= 1.62，这个功能是隐式启用的。
  否则，它需要一个夜间的Rustc。

**已废弃**： `use_std`已被废弃，等同于`std`。

## Rust版本支持

目前支持的最小Rust工具链版本是**Rust 1.13.0**。(libc 目前没有任何计划关于改变最小支持的支持的 Rust 版本）。需要较新的 Rust 特性的 API 只在较新的 Rust 工具链上可用：

| Feature              | Version |
|----------------------|---------|
| `union`              |  1.19.0 |
| `const mem::size_of` |  1.24.0 |
| `repr(align)`        |  1.25.0 |
| `extra_traits`       |  1.25.0 |
| `core::ffi::c_void`  |  1.30.0 |
| `repr(packed(N))`    |  1.33.0 |
| `cfg(target_vendor)` |  1.33.0 |
| `const-extern-fn`    |  1.62.0 |

## 平台支持

[特定平台文档（master分支）][docs.master]。

见[`ci/build.sh`](https://github.com/rust-lang/libc/blob/master/ci/build.sh)
关于每个Rust工具链的`libc`保证可以在哪些平台上构建。
工具链的平台。在[GitHub Actions]和[Cirrus CI]中的测试矩阵显示了
`libc`测试运行的平台。
<div class="platform_docs"></div>

## 许可证

本项目的授权范围是

* [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0)
  ([LICENSE-APACHE](https://github.com/rust-lang/libc/blob/master/LICENSE-APACHE))

* [MIT License](https://opensource.org/licenses/MIT)
  ([LICENSE-MIT](https://github.com/rust-lang/libc/blob/master/LICENSE-MIT))

[GitHub Actions]: https://github.com/rust-lang/libc/actions
[GHA Status]: https://github.com/rust-lang/libc/workflows/CI/badge.svg
[Cirrus CI]: https://cirrus-ci.com/github/rust-lang/libc
[Cirrus CI Status]: https://api.cirrus-ci.com/github/rust-lang/libc.svg
[crates.io]: https://crates.io/crates/libc
[Latest Version]: https://img.shields.io/crates/v/libc.svg
[Documentation]: https://docs.rs/libc/badge.svg
[docs.rs]: https://docs.rs/libc
[License]: https://img.shields.io/crates/l/libc.svg
[docs.master]: https://rust-lang.github.io/libc/#platform-specific-documentation
