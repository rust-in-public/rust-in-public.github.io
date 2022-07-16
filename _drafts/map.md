---
layout: post
title: "Method Chaining in Rust"
tag: rustlang
---

Similar to the ```filter()``` method, Rust's Standard Library also features a ```map()``` method. While ```filter()``` allows us to return items that match our closure, ```map()``` allows us to perform a function on all elements in the iterator. Consider the example code: 

```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    println!("{:?}", v.iter().map(|x| x * x).collect::<Vec<_>>());
}
```

Knowing what we know about Rust's method chaining from our ```filter()``` example, we should be able to predict the output. Across all indices of ```v```, we multiply its value by itself, then collect those values into a vector for printing. **Note:** While it works in this example, simply multiplying a number by itself can produce overflows and is not always safe, in which case Rust provides both ```pow()``` and ```checked_pow()``` to handle raising a value to an exponential power.

```map()``` provides a flexible solution in a variety of scenarios, and it can be used on more than just integers. Let's say you wanted to convert all the words in a vector to lowercase. You could use ```map()``` to solve this: 

```rust
fn main() {
    let v = vec!["ThIs", "IS NOT", "iN", "lOwErCaSe"];
    println!("{:?}", v.iter().map(|x| x.to_lowercase()).collect::<Vec<_>>());
}
```

Despite what our vector says at its declaration, our code will print all items in ```v``` as lowercase.