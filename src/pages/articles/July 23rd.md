---
title: "Routine: July 23rd"
date: "2018-07-23T16:16:26.042Z"
layout: post
draft: false
path: "/posts/routine-2018-07-23"
tags:
- "routine"

description: "Notes: Ownership of Rust"
---


## Notes: Ownership of Rust
### What is `Ownership`
Ownership is an important and unique concept in Rust. Take some effort on it will help us learn why Rust can ensure `memory safety`.

- Always one owner for a data.
- Ownership can be `move`
- Ownership (a variable and the data it owns) will be `drop` when leaving the scope
- `clone` (address, size ...) & `copy` (value)

### Reference (&)

- To better deal with `Ownership`, we can borrow `Ownership` by `Reference`(&).
- `Mutable Reference` and `Immutable Reference`
- Either one `mutable reference` or any number of `immutable reference` to avoid `data race`
- `Reference` always valid. ( Never `dangling reference`)

### Futher Reading
- [The Rust Programming Language](https://doc.rust-lang.org/book/second-edition/ch04-02-references-and-borrowing.html)
- [中文翻译版](http://rustdoc.saigao.fun/ch04-02-references-and-borrowing.html) 