---
layout: post
title:  "Rust query string parser with nesting support"
date:   2014-09-16 12:00
languages: Rust
categories:
---

Just ended writing **[rust-query]** library.  Use **rust-query** library to parse query-string to corresponding [json] values.

~~~rust
use query::parse;

// will contain result as Json value
let object = parse("foo[0][a]=a&foo[0][b]=b&foo[1][a]=aa&foo[1][b]=bb");
~~~

You can work with raw Json value or decode it to Rust strucure by using serialize::json::decode function (see examples in [json]).

See more examples in the README on GitHub: [rust-query].

[json]: http://doc.rust-lang.org/serialize/json/index.html
[rust-query]: https://github.com/s-panferov/rust-query
