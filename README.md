Convert number to enum
======================

[<img alt="github" src="https://img.shields.io/badge/github-dtolnay/enumn-8da0cb?style=for-the-badge&labelColor=555555&logo=github" height="20">](https://github.com/dtolnay/enumn)
[<img alt="crates.io" src="https://img.shields.io/crates/v/enumn.svg?style=for-the-badge&color=fc8d62&logo=rust" height="20">](https://crates.io/crates/enumn)
[<img alt="docs.rs" src="https://img.shields.io/badge/docs.rs-enumn-66c2a5?style=for-the-badge&labelColor=555555&logo=docs.rs" height="20">](https://docs.rs/enumn)
[<img alt="build status" src="https://img.shields.io/github/actions/workflow/status/dtolnay/enumn/ci.yml?branch=master&style=for-the-badge" height="20">](https://github.com/dtolnay/enumn/actions?query=branch%3Amaster)

This crate provides a derive macro to generate a function for converting a
primitive integer into the corresponding variant of an enum.

The generated function is named `n` and has the following signature:

```rust
impl YourEnum {
    pub fn n(value: Repr) -> Option<Self>;
}
```

where `Repr` is an integer type of the right size as described in more
detail below.

## Example

```rust
use enumn::N;

#[derive(PartialEq, Debug, N)]
enum Status {
    LegendaryTriumph,
    QualifiedSuccess,
    FortuitousRevival,
    IndeterminateStalemate,
    RecoverableSetback,
    DireMisadventure,
    AbjectFailure,
}

fn main() {
    let s = Status::n(1);
    assert_eq!(s, Some(Status::QualifiedSuccess));

    let s = Status::n(9);
    assert_eq!(s, None);
}
```

## Signature

The generated signature depends on whether the enum has a `#[repr(..)]`
attribute. If a `repr` is specified, the input to `n` will be required to be
of that type.

```rust
#[derive(enumn::N)]
#[repr(u8)]
enum E {
    /* ... */
}

// expands to:
impl E {
    pub fn n(value: u8) -> Option<Self> {
        /* ... */
    }
}
```

On the other hand if no `repr` is specified then we get a signature that is
generic over a variety of possible types.

```rust
impl E {
    pub fn n<REPR: Into<i64>>(value: REPR) -> Option<Self> {
        /* ... */
    }
}
```

## Discriminants

The conversion respects explictly specified enum discriminants. Consider
this enum:

```rust
#[derive(enumn::N)]
enum Letter {
    A = 65,
    B = 66,
}
```

Here `Letter::n(65)` would return `Some(Letter::A)`.

<br>

#### License

<sup>
Licensed under either of <a href="LICENSE-APACHE">Apache License, Version
2.0</a> or <a href="LICENSE-MIT">MIT license</a> at your option.
</sup>

<br>

<sub>
Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in this crate by you, as defined in the Apache-2.0 license, shall
be dual licensed as above, without any additional terms or conditions.
</sub>
将数字转换为枚举
<img alt="github" src="https://img.shields.io/badge/github-dtolnay/enumn-8da0cb?style=for-the-badge&labelColor=555555&logo=github" height="20">
<img alt="crates.io" src="https://img.shields.io/crates/v/enumn.svg?style=for-the-badge&color=fc8d62&logo=rust" height="20">
<img alt="docs.rs" src="https://img.shields.io/badge/docs.rs-enumn-66c2a5?style=for-the-badge&labelColor=555555&logo=docs.rs" height="20">
<img alt="build status" src="https://img.shields.io/github/actions/workflow/status/dtolnay/enumn/ci.yml?branch=master&style=for-the-badge" height="20">

这个库提供了一个派生宏,能生成一个函数将基本整数转换成枚举的对应的变体。

生成的函数名为n,签名如下:

rust
复制
impl YourEnum {
    pub fn n(value: Repr) -> Option<Self>;
}
这里Repr是一个大小适当的整数类型,详细情况请看下文。

例子
rust
复制
use enumn::N;

#[derive(PartialEq, Debug, N)] 
enum Status {
    LegendaryTriumph,
    QualifiedSuccess,
    FortuitousRevival,
    IndeterminateStalemate,
    RecoverableSetback,
    DireMisadventure,
    AbjectFailure,
}

fn main() {
    let s = Status::n(1);
    assert_eq!(s, Some(Status::QualifiedSuccess));

    let s = Status::n(9);
    assert_eq!(s, None);
}
签名
生成的签名取决于枚举是否有#[repr(..)]属性。如果指定了repr,则n函数的参数类型需要匹配。

rust
复制
#[derive(enumn::N)]
#[repr(u8)]
enum E {
    /* ... */
}

// 展开成:
impl E {
    pub fn n(value: u8) -> Option<Self> {
        /* ... */
    }
}
如果没有指定repr,则签名会泛型于各种可能的类型。

rust
复制
impl E {
    pub fn n<REPR: Into<i64>>(value: REPR) -> Option<Self> {
        /* ... */
    }
}
区分值
转换会根据明确指定的枚举区分值来进行。例如:

rust
复制
#[derive(enumn::N)]
enum Letter {
    A = 65,
    B = 66,  
}
这里Letter::n(65)会返回Some(Letter::A)。

<br>
许可证
<sup> 根据Apache许可证2.0版或MIT许可证中的一种许可协议许可。 </sup> <br> <sub> 除非您明确表示反对,否则根据Apache-2.0许可证中的条款,由您提交用于包含在本仓库中的任何贡献都应根据上述双许可证许可,且不附带任何额外条款或条件。 </sub>
