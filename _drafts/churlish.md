---
layout: post
title: "Making something useful: churlish, part 1"
tag: app
---

One of the difficult parts of learning any programming language for me is trying to figure out the point at which I can make anything useful. If you are anything like me, you are likely learning by working your way through a book, which in my case has mostly been [*Programming Rust* by Jim Blandy, Jason Orendorff, and Leonora F. S. Tindall](https://www.oreilly.com/library/view/programming-rust-2nd/9781492052586/). Like so many others, I have long relied on O'Reilly books for learning, as they always seem like a safe bet and are filled with clear, precise writing.

### Regex and Me

The joke everyone knows goes something like: "You have a problem you want to solve with regular expressions. Now you have two problems." More often than not, this is likely to be true. But I must admit that I am a sucker for regex--if I can find something to use it on, I want to try to do it with ```sed```. Some might call this masochism, but I have found so many instances where the problem I want to solve requires regex that I have tried to keep it in my memory.

With my love of regex in mind, I thought a fun project to take up in Rust would be something only half a year or so behind the curve: a Wordle guesser. However, like most things in Rust, we need to do some planning before writing any code.

### What do we actually want to do?

What I'm looking to do is to create a program that allows a user to enter in the results of a Wordle guess, then a list of valid words is returned. So, let's enumerate the things we'll need to do to achieve this:

1. **Process input**: We will need a way to handle user input, which to this point I have done using ```env::args().collect();```. This gives me an opportunity to use the more expansive [clap](https://crates.io/crates/clap) crate.
2. **Parse Wordle guess**: We will need to develop some sort of readable pattern for our program to be able to interpret the guess. I think we have some leeway here with building a pattern that makes input handling easy enough.
3. **Build regex pattern**: The backbone of this program will be constructing a regex pattern we can use to search a word list. It feels like there will be a lot of edge cases that will also need to be handled here.
4. **Filter results**: Because of those edge cases, we may need to additionally filter our results.
