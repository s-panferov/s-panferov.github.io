---
layout: post
title:  "Use Option and Result type from Rust in TypeScript"
date:   2014-11-30 12:00
languages: Rust
categories:
---

When I write something in Rust, I always happy about presence of [Option](http://doc.rust-lang.org/core/option/enum.Option.html) and [Result](http://doc.rust-lang.org/core/option/enum.Result.html) type there. Together this types make errors and no-value cases vesy easy to understand.

After I started my new project on TypeScript language, I decided to re-implement these types in TypeScript. [Here](https://gist.github.com/s-panferov/575da5a7131c285c0539) is the result.

And now there are no more `null` in my code. I'm happy!

P.s. There is [one](https://github.com/Microsoft/TypeScript/issues/1212) inconvenience about TypeScript's generics. It makes to type the full types almost every time.