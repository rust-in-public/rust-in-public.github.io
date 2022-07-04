---
layout: post
title: "Method Chaining in Rust"
tag: rustlang
---

While not unique to Rust, there are many functions in the Rust Standard Library that return ```self```. Because of this, Rust allows all kinds of method chaining, often resulting in some intimidating-looking-at-first code. As stated in [Rust Design Patterns](https://rust-unofficial.github.io/patterns/functional/index.html), Rust is an imperative language, but it allows code to be written in a functional style.

As an example of this difference between imperative and declarative style, let's say we wanted to print all even numbers in a vector. Without making use of method chaining, we might write something that looks like this:

```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    let mut evens = Vec::new();
    for i in 0..v.len() {
        if v[i] % 2 == 0 {
            evens.push(v[i]);
        }
    }
    println!("{:?}", evens);
}
```

This code works perfectly fine, but it is imperative, as we work step-by-step through *how* the program should do something. It does not follow the [declarative goals of functional programming](https://kerkour.com/rust-functional-programming) that ask us to describe *what* the program is doing. In Rust, we can chain methods together to produce code in a functional style:

```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    println!("{:?}", v.iter().filter(|&x| x % 2 == 0).collect::<Vec<_>>());
}
```

There is a lot to unpack in these few lines of code! This will produce the same result as our imperative example, and do so without pushing values to a new vector and without the use of a ```for``` loop. But what is happening here? Let's break this code down a bit more.

First, we are making use of Rust's ```iter()``` method to create an iterable type from ```v```. Importantly, ```iter()``` creates a ```Iter<'a, T>``` type that only references ```T```, so what it is creating is only a reference to ```T```. This will become clearer as we work through our chained method.

Now that we are capable of iterating over the values of ```v```, we can use Rust's ```filter()``` method. ```filter()``` is applied to the iterator we have created with our previous ```iter()``` method, but to apply a filter we need to give it something to do. To do so, we need to create a **closure**.

### What is a closure?

Rust's documentation describes closures as "anonymous functions that can capture their environment." If this is hard to conceptualize, some code might help. Consider the following simple example of a closure:

```rust
fn main() {
    let add_one = |x| { x + 1 };
    println!("{}", add_one(5));
}
```

The variable ```add_one```, whose value is our closure, can now work like a function. Unlike a function, it can capture values from its scope. In this closure example, our closure adds one to its captured value. **Note**: unlike in functions, closures do not need type inferences, as the compiler will infer types in a closure. You could easily rewrite ```add_one``` as:

```rust
let add_one = |x| -> i32 { x + 1 };
```

But it is not necessary, and Rust will happily compile either one.

### Back to even numbers

Let's return to our previous method chaining example:

```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
    println!("{:?}", v.iter().filter(|&x| x % 2 == 0).collect::<Vec<_>>());
}
```

Since we now know what a closure does, we can figure out how ```filter()``` is working. For all of the values of our iterator, we check to see if ```x``` and 2 are perfectly divisible (a modulo of 0). Those that pass this filter are returned by ```filter()```, which we then need to add to some kind of type. Here we can rely on ```collect()``` to do that work for us, but we need to specify a type to collect it into. For this we use ```Vec<_>``` to allow the compiler to infer what type of vector to create.

One last thing to note: remember that ```iter()```  creates a *reference* to the values of ```v```, so for our filter to work we need to pass a *reference* to our captured value as ```&x``` in our closure. Without this reference to ```x```, Rust gives us the following error:

```console
error[E0369]: cannot mod `&&{integer}` by `{integer}`
 --> src/main.rs:3:44
  |
3 |     println!("{:?}", v.iter().filter(|x| x % 2 == 0).collect::<Vec<_>>());
  |                                          - ^ - {integer}
  |                                          |
  |                                          &&{integer}
  |
help: `%` can be used on `{integer}`, you can dereference `x`
  |
3 |     println!("{:?}", v.iter().filter(|x| *x % 2 == 0).collect::<Vec<_>>());
  |                                          +
  ```

This is not the case in all usages of a closure, but since we have chained our closure with ```iter()```, we need to use a reference to ```x``` for our types to match.

Let's think again about the chain we have created:

1. Iterate over all values of ```v```
2. Filter out even numbers
3. Collect even numbers in a vector
4. Print the vector

This procedure is no different than our earlier code with its ```for``` loop, but we've kept our code in a functional style enabled by Rust's method chaining. Now that we have some of these basics figured out, we'll work through the very powerful ```map()``` next.
