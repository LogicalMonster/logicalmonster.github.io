---
title: Cpp指针、引用 VS Rust所有权、借用
date: 2025-02-13 21:23:25
tags:
---
Rust的所有权与变量遮蔽有关，与C++的移动语义还是有一些区别。比如在C++下述操作中const变量`d`和`j`并不会随`std::move`而清空：

```cpp
std::string a{"1"};
std::string b{"12"};
std::string c{"123"};
std::string const d{"1234"};
std::string i{std::move(a)};
i = std::move(b);
i = std::move(d);
std::string const j{std::move(c)};
std::string const k{std::move(j)};
```

对于引用：

```cpp
std::string a{"1"};
std::string b{"12"};
std::string c{"123"};
std::string const d{"1234"};
std::string &g = a;
// std::string &g = d; // error
std::string const& h = a;
// std::string const& h = d;
std::string &&i{std::move(a)};
i += "5";
i = std::move(b);
i += "6";
// std::string &&i = std::move(d); // error
i = std::move(d);
i += "7";
std::string const &&j{std::move(c)};
std::string const &&k{std::move(j)};
std::string const &&l{std::move(d)};
```

而rust则通过所有权消除了模糊的部分：

```rust
let mut a = String::from("1");
let mut b = String::from("12");
let mut c = String::from("123");
let d = String::from("1234");
let g = &mut a;
// let g = &mut d; // error
let h = &a;
println!("h: {}", h);
// let h = &c; // ok
let mut i = a;
i += "5";
// println!("a: {}", a); // error
println!("i: {}", i);
i = b;
i += "6";
// println!("b: {}", b); // error
println!("i: {}", i);
// let mut i = d; // ok
i = d;
i += "7";
// println!("d: {}", d); // error
let j = c;
// println!("c: {}", c); // error
let k = j;
// println!("j: {}", j); // error
```

通过对比，可以感觉到Rust的所有权机制的确是一个比较不错的设计，在所有权的原语下，我们有更为彻底的移动语义，我们不必再引入右值引用，由此而产生的引用折叠，万能引用，完美转发、RVO、NRVO、复制省略、临时量实质化、值类别、左值、将亡值、纯右值、泛左值（左值+将亡值）、右值（将亡值+纯右值）、将不必再过分考虑。不过unsafe则像一个深渊，又要包装又要安全性，造成的上手难度比单纯的C指针似乎更大。或许我们需要的是一种可选性的渐进安全的语言标准。或许一个语言如果太像C++就可能会分裂与复杂。或许人们希望的是一门简洁但具有表达能力同时又具有一致性的语言，就像类型论与集合论一样。不知道这些问题会不会随着类型论和线性类型的发展与理解而得到解决。当然以上都可以看作是一个名词党的胡言乱语。

## C++ 指针、引用

```cpp
#include <iostream>
#include <string>

void test0() {
    std::string a{"1"};
    a += "2";
    std::string b{a};
    b += "3";
    std::string c{b};
    c += "4";
    std::string const d{c};
    std::string *e = &a;
    printf("*e: %s\n", e->c_str());
    e = &b;
    printf("*e: %s\n", e->c_str());
    std::string const *f = &a;
    printf("*f: %s\n", f->c_str());
    f = &c;
    printf("*f: %s\n", f->c_str());
    std::string *const g = &a;
    printf("*g: %s\n", g->c_str());
    // std::string *const g = d; // error
    // printf("*g: %s\n", g->c_str()); // ok
    std::string const *const h = &a;
    printf("*h: %s\n", h->c_str());
    // std::string const *const h = &d;
    // printf("*h: %s\n", h->c_str()); // ok
}

void test1() {
    std::string a{"1"};
    a += "2";
    std::string b{a};
    b += "3";
    std::string c{b};
    c += "4";
    std::string const d{c};
    std::string &g = a;
    printf("g: %s\n", g.c_str());
    // std::string &g = d; // error
    // printf("g: %s\n", g.c_str()); // ok
    std::string const& h = a;
    printf("h: %s\n", h.c_str());
    // std::string const& h = d;
    // printf("h: %s\n", h.c_str()); // ok
    std::string i{std::move(a)};
    i += "5";
    printf("a: %s\n", a.c_str());
    // printf("i: %s\n", i.c_str());
    i = std::move(b);
    i += "6";
    printf("b: %s\n", b.c_str());
    printf("i: %s\n", i.c_str());
    // std::string i{std::move(d)};
    i = std::move(d);
    i += "7";
    printf("d: %s\n", d.c_str());
    printf("i: %s\n", i.c_str());
    std::string const j{std::move(c)};
    printf("c: %s\n", c.c_str());
    printf("j: %s\n", j.c_str());
    std::string const k{std::move(j)};
    printf("j: %s\n", j.c_str());
    printf("k: %s\n", k.c_str());
}

void test2() {
    std::string a{"1"};
    a += "2";
    std::string b{a};
    b += "3";
    std::string c{b};
    c += "4";
    std::string const d{c};
    std::string &g = a;
    printf("g: %s\n", g.c_str());
    // std::string &g = d; // error
    // printf("g: %s\n", g.c_str()); // ok
    std::string const& h = a;
    printf("h: %s\n", h.c_str());
    // std::string const& h = d;
    // printf("h: %s\n", h.c_str()); // ok
    std::string &&i{std::move(a)};
    i += "5";
    printf("a: %s\n", a.c_str());
    printf("i: %s\n", i.c_str());
    i = std::move(b);
    i += "6";
    printf("a: %s\n", a.c_str());
    printf("i: %s\n", i.c_str());
    // std::string &&i = std::move(d); // error
    i = std::move(d);
    i += "7";
    printf("d: %s\n", d.c_str());
    printf("i: %s\n", i.c_str());
    std::string const &&j{std::move(c)};
    printf("c: %s\n", c.c_str());
    printf("j: %s\n", j.c_str());
    std::string const &&k{std::move(j)};
    printf("j: %s\n", j.c_str());
    printf("k: %s\n", k.c_str());
    std::string const &&l{std::move(d)};
    printf("d: %s\n", d.c_str());
    printf("l: %s\n", l.c_str());
}

int main()
{
    test0();
    test1();
    test2();
}
```

## Rust 所有权、借用

```rust
fn main() {
    let mut a = String::from("1");
    // println!("a: {}", a);
    a += "2";
    let mut b = a.clone();
    // println!("b: {}", b);
    b += "3";
    let mut c = b.clone();
    c += "4";
    // println!("c: {}", c);
    let d = c.clone();
    let mut e = &mut a;
    println!("e: {}", e);
    e = &mut b;
    println!("e: {}", e);
    let mut f = &a;
    println!("f: {}", f);
    f = &c;
    println!("f: {}", f);
    let g = &mut a;
    println!("g: {}", g);
    // let g = &mut d; // error
    // println!("g: {}", g);
    let h = &a;
    println!("h: {}", h);
    // let h = &c; // ok
    // println!("h: {}", h); // ok
    let mut i = a;
    i += "5";
    // println!("a: {}", a); // error
    println!("i: {}", i);
    i = b;
    i += "6";
    // println!("b: {}", b); // error
    println!("i: {}", i);
    // let mut i = d; // ok
    i = d;
    i += "7";
    // println!("d: {}", d); // error
    println!("i: {}", i);
    let j = c;
    // println!("c: {}", c); // error
    println!("j: {}", j);
    let k = j;
    // println!("j: {}", j); // error
    println!("k: {}", k);
}
```
