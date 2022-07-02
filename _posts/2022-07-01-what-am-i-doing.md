---
layout: post
title: "What am I doing?"
tag: intro
---

This is always a useful question to ask. I created this blog to try to achieve a few things, but I want to focus on some quick details about myself first. I am not a programmer by trade--I have a PhD in English, and I make a living teaching English at the university level. While this might seem pretty distant from the world of Rust, I've kept an interest in programming throughout my life, and I have tried to teach myself new things whenever I have had the opportunity. I have no real great interest in creating software for other people, but I have found myself in countless situations over the years where I need to do something I cannot find a way to do, and having the ability to solve those situations with my own (usually bad) code is rewarding.

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

After binding this to a keyboard shortcut, all of the sudden I have a pretty nifty solution to my problem. Is this the *best* solution? Probably not. I am sure I can do this with a Bash script just as well. But even with my elementary understand of Rust currently, I can solve a problem with what Rust provides, which I think is pretty neat!

And, while it may seem like a ridiculous reason, one of the first things I learned about Rust was that it continues to be voted the "most loved programming language" year after year. There are reasons I can understand for this, which hopefully my blog will demonstrate.  

### What do I know?

At the time of starting this blog, I have been experimenting with extremely basic concepts in Rust for a few months, but I have been able to put more concentrated time into learning the language over the last month or so. Still, I am not exactly starting from scratch. As a much younger kid, I taught myself the basics of PHP 3/MySQL applications in the early 2000s (alongside HTML/CSS, of course), and I was taking very remedial C++ courses in high school at that time. In the years that followed, I learned some JavaScript (though nothing compared to what it looks like in 2022), some basic Bash scripting, and enough Python to be able to look at code and understand what is going on.

In that sense, I would say I understand basic concepts in programming, maybe even a fair amount of the grammar of a language, but I am very far from being fluent in anything. Given everything described here, I created this blog for the following reasons:

1. **To structure learning**: As mentioned, I don't make a living from any of this, but a slow summer schedule has given me more time to learn Rust. I am hoping to use this blog to give myself some structure, so that I can stay committed to learning.
2. **To learn in public**: In my academic life, I am a fervent proponent of learning in public--my writing classes require students to share their work, to produce writing collaboratively, to peer edit frequently, and to generally see learning not as the private work of a singular mind, but something that can occur in public for others to benefit from. If I have some breakthrough and share it with no one, what is the point? I hope this blog can serve to make this learning visible, and, in my small attempts to teach what I am learning, help me to master these concepts more fully.
3. **To help someone else**: While I doubt another person will read any of this, I hope that making my struggle with Rust known to others might help someone else struggling in the same way. If something I write helps them solve a problem or feel a bit less alone in the struggle, that sounds fantastic to me!
