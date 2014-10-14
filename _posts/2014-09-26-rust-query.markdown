---
layout: post
title:  "Queryst - Rust query string parser with nesting support"
date:   2014-09-26 12:00
languages: Rust
categories:
---

Just ended writing **[queryst]** library.  Use **queryst** library to parse query-string to corresponding [json] values.

~~~rust
use query::parse;

// will contain result as Json value
let object = parse("foo[0][a]=a&foo[0][b]=b&foo[1][a]=aa&foo[1][b]=bb");
~~~

You can work with raw Json value or decode it to Rust strucure by using serialize::json::decode function (see examples in [json]).

See more examples in the README on GitHub: [queryst].

[json]: http://doc.rust-lang.org/serialize/json/index.html
[queryst]: https://github.com/rustless/queryst
