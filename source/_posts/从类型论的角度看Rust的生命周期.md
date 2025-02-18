---
title: 从类型论的角度看Rust的生命周期
date: 2025-02-18 16:24:13
tags:
---
Rust的生命周期尽管细化了一些编程语言中的类型，但没有细化完全，或者说在Rust中有些符号滥用，关系表示的并不是那么明晰。用不严谨的类型论的符号可以表示如下。需要注意的就是生命周期可以视作类型的“类型”，用`T : 'a`表示，而生命周期之间的关系则是同一层级的类型之间的关系用`'a <: 'b`表示`'a`的生命周期长于`'b`。

```
'static 是 'a 的子类型, 'a 是 'b 的子类型, 即: 
'static <: 'a <: 'b

i32 : 'static
String : 'static
T : 'static

若T : 'static, 则&T = &'a T : 'a. 
这里的&T似乎应该视作&'a T. 万恶的自动推导将细节隐藏起来了. 暴论: Rust的学习难度就是因为这些干扰语言一致性的语法糖造成的。

若T : 'a, 则&'static T : 'static

若'a <: 'b, 设&'a T : 'x, &'b T : 'y, 则'x = 'a <: 'b = 'y. 
```

总结一下：

```
'static <: 'a <: 'b
&'x T : 'x
&T将被自动推导为&'a T
```

具体验证请见下述代码：

```rust
fn f<T>(_: T)
where
    T: 'static,
{
}

fn g<'a, T>(_: &'static T)
where
    T: 'a,
{
}

fn h<'a, T>(_: &'a T)
where
    T: 'static,
{
}

fn hh<T>(_: &T)
where
    T: 'static,
{
}

fn main() {
    let x = 0;
    f(x); // ok
    // g(x); // error
    // h(x); // error
    // hh(x); // error
    // f(&x); // error
    // g(&x); // error
    h(&x); // ok
    hh(&x); // ok
    static Y: i32 = 0;
    f(Y); // ok
    // g(Y); // error
    // h(Y); // error
    // hh(Y); // error
    f(&Y); // ok
    g(&Y); // ok
    h(&Y); // ok
    hh(&Y); // ok
}
```

其中需要注意的是Rust允许`&'b &'a mut i32`这种类型，并且T包含这种可能性。下面再补充一下关于Rust的借用的代码：

```rust
fn main() {
	let mut x: i32 = 0;
	let mut x_mut: &mut i32 = &mut x;
	let x_mut_ref: &&mut i32 = &x_mut;
	let x_mut_mut: &mut &mut i32 = &mut x_mut;
}
```
