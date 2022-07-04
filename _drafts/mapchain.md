---
layout: post
title: "Method Chaining, and using .map()"
tag: rustlang
---

While not necessarily unique to Rust, there are many functions in the Rust Standard Library that I've encountered that return ```self```. Because of this,


Let's say we we wanted to convert the text ```MF DOOM``` to lowercase, then replace all "o" characters with zeroes. Without method chaining, we might write something like:

```rust
fn main() {
    let s = "MF DOOM"; 
    let lower = s.to_lowercase();
    let replaced = lower.replace("o", "0");
    println!("{}", replaced);
}
```

This code works perfectly fine, but it also betrays the declarative goals of functional programming. The cIn Rust, we can chain these together:

```rust
fn main() {
    let s = "MF DOOM";
    println!("{}", s.to_lowercase().replace("o", "0"));
}
```

This will give us the same result without creating unnecessary variables, and no change has been made to the value of ```s```.

Imagine a scenario where you had a vector of integers, and you wanted to multiply all of them by 2. My imperative brain tells me that we can solve it something like this: 

```rust
fn main() {
    let v = vec![1, 2, 3];
    let mut new_v = Vec::new();
    for i in 0..v.len() {
        new_v.push(v[i]*2);
    }
    println!("{:?}", new_v);
}
```

Does this work? Definitely. Is it the most functional solution? Absolutely not. Here's where the very useful ```map()``` function comes to the rescue. ```map()``` allows us to perform a function to each item in an iterable type.

```rust
fn main() {
    let v = vec![1, 2, 3];
    println!("{:?}", v.iter().map(|x| x * 2).collect::<Vec<_>>());
}
```

Woof, there's a lot to unpack here. Let's first look at the syntax of 