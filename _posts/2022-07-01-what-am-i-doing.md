---
layout: post
title: "What am I doing?"
tag: intro
---

This is always a useful question to ask. I created this blog to try to achieve a few things, but I want to focus on some quick details about myself first. I am not a programmer by trade--I have PhD in English, and make a living teaching English at the university level. While this seems far from Rust, I've kept an interest in programming throughout my life, and I have tried to teach myself new things whenever I have had the opportunity. I have no real great interest in creating software for other people, but I have found myself in countless situations over the years where I need to do something I cannot find a way to do, and having the ability to solve those situations with my own (usually bad) code is rewarding.

### Why Rust?

Again, a useful question! To the degree to which I understand Rust, it might be safe to argue that it is not *really* meant for the things I'm looking to do with it. It is not a scripting language, its memory and type safety can make it confusing for an amateur like myself, and the kinds of things being made in Rust are  on a level of complexity that I will likely never touch. But there are a few things that attracted me to rust--like Go, its use of [crates.io](https://crates.io) to bundle packages makes some surprisingly complex problems possible to be solved very quickly.  

Here's an example. Canvas as used as the LMS at nearly all the schools I have taught at, and without the use of a third-party tool that no IT department wants to install, there is no support for Markdown. I like to do all writing in Markdown these days, and I wanted the fastest way to go from Markdown to formatting for Canvas without creating a browser extension. There are likely to be countless simple enough ways to go from Markdown to HTML, but I wanted it to be so that I could run a program that converts the Markdown in my clipboard into HTML in my clipboard, so I could just copy and paste HTML into Canvas. With Rust, I could do this with  two crates ([clipboard-ext](https://crates.io/crates/clipboard-ext) and [markdown](https://crates.io/crates/markdown)) and only a few lines of code:

```rust
use clipboard_ext::prelude::*;
use clipboard_ext::x11_bin::ClipboardContext;

fn main () {
    let mut c = ClipboardContext::new().unwrap();
    let val = c.get_contents().unwrap();
    let html : String = markdown::to_html(&val);
    c.set_contents(html.to_owned()).unwrap();
}
```

Now I've made this tiny binary runnable with a keyboard shortcut, and all of the sudden I have a pretty nifty solution to my problem. Is this the *best* solution? Probably not. But even with my elementary understand of Rust currently, I can solve a problem the way I would like to, which I think is pretty neat!

And, while it may seem like a ridiculous reason, one of the first things I learned about Rust was that it continues to be voted the "most loved programming language" year after year. There are reasons I can understand for this, which hopefully my blog will demonstrate.

### What do I know?
