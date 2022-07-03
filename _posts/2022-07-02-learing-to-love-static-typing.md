---
layout: post
title: "Learning to Love Static Typing"
tag: types
---

While this might be remedial for some, I thought it would help to think a bit about types in Rust.

Let's start with some simple code:

```rust
fn main() {
    let mut test = 0; 
    test = "test";
    println!("{}", var);
}
```

If we try to compile this, Rust will give us the following error:

```console
error[E0308]: mismatched types
 --> src/main.rs:5:11
  |
3 |     let mut var = 0; 
  |                   - expected due to this value
4 |     
5 |     var = "Test";
  |           ^^^^^^ expected integer, found `&str`
```

Since ```test``` was previously set to an integer value, even though it was not declared as an integer, Rust expects it will stay an integer.

If we were to run something similar in JavaScript, we'd have no issue:

```javascript
var test = 0;
test = "test";
console.log(test);
```

JavaScript's type coercion will even allow you to do something like:

```javascript
var a = 1;
var b = "2";
console.log(a + b);
```

JavaScript will convert ```a``` to a string and output ```12``` as our result. Rust allows no such thing:

```rust
fn main() {
    let a = 1; 
    let b = "2";
    let sum = a + b;
    println!("{}", sum);
}
```

This code gives us an error of:

```console
  |
4 |     let sum = a + b;
  |                 ^ no implementation for `{integer} + &str`
```

To fix the previous example, we'd need to make use of Rust's extensive type conversion abilities. To replicate our previous JavaScript example, we could do something like:

```rust
fn main() {
    let a = 1; 
    let b = "2";
    let sum = a.to_string() + b;
    println!("{}", sum);
}
```

As its name implies, ```to_string()``` converts the integer value of ```a``` into a string, which we then concatenate to produce ```12```.

### But why so strict?

The why of this static typing is what I have found myself struggling to understand. On a technical side, there are a few reasons for preferring static typing. In a compiled language like Rust, knowing the types for all involved code will allow Rust to optimize its memory management. This is at the edge of my understanding, but if it interests you to know more, I found [this post](https://deepu.tech/memory-management-in-rust/) to be especially insightful. In the same vein, performance will be better since there is no interpretation/coercion/etc that needs to be done, as Rust will know what every type should be already.

In large-scale applications, I imagine this is of the utmost importance. But, for the tiny programs I am working on, this might seem more like a nuisance than anything else. However, I've come to realize some of the practical benefits of Rust's typing. First, it encourages good habits. Obviously, dynamic typing will allow for something to be developed more rapidly, since some decisions can be made on-the-fly. But on-the-fly thinking isn't always teaching us how to find the best solutions to problems, and the simple act of thinking about what it is that is trying to be done (and what kinds of types might be needed to solve the problem) seems like it should create better code.

As well, as a more practical example, consider the following JavaScript code:

```javascript
var grade = 93; 
grde = grade / 100 * 4;
console.log(grade);
```

Here, our typo of ```grde``` means that our returned result is not as we intended. While we might catch this typo if our code is only three lines (and modern linters might catch this as a possible typo), no error is produced. Rust helps us out with all of this. If we try to compile:

```rust
fn main() {
    let mut grade = 93; 
    grde = grade / 100 * 4;
    println!("{}", grade);
}
```

We get the following:

```console
error[E0425]: cannot find value `grde` in this scope
 --> src/main.rs:3:5
  |
3 |     grde = grade / 100 * 4;
  |     ^^^^ help: a local variable with a similar name exists: `grade`
```

Here we're getting both things caught: ```grde``` has never been initialized, so its value is not in scope. In addition, we get a typo suggestion. Nifty. Again, while this might seem obvious here, if we're working with thousands of lines of code these features add up.

There is so much more to cover that I've learned about types in Rust--since seemingly all the things I write in Rust involve string manipulation, that'll be next!
