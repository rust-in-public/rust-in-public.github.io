---
layout: post
title: "OT: Passwords, and a work-in-progress"
tag: offtopic
---

Years ago, I started trying to find an ideal solution for better password hygiene.

With the help of Dropbox's [zxcvbn](https://github.com/dropbox/zxcvbn) we can do some simple checking of our method. In the olden days, I might've used a password like `Noise543%`--a word I could remember, a sequence of numbers, and a special character. Typically, this kind of password was selected to meet the password requirements of whatever site I was using this password on. zxvcbn lets us know that even if the password was hashed with a slow hash like bcrypt, we're not posing much of an issue:

`10k / second: 42 minutes (offline attack, slow hash, many cores)`

Given that [hashcat](https://hashcat.net/hashcat/) can do 28k hashes a second on just a 2080Ti GPU, it's clear that even the attempts to complicate this password wouldn't do much to protect it.

I wanted a method that allowed for the following:

1. Long password length
2. Unique password for every site
3. Based on open standards
4. Not tied down to any specific software
5. Relatively portable
6. Zero knowledge

To achieve this, I decided to choose the following method. First, choose a single, secret word. The word does not have to be long or particularly secure, but it should be a word known to you and simple to remember. For our purposes here, we'll just use `test` as our secret word. Then, combine this word with the domain name of whatever you are logging into, for example, `gmail`. Our phrase then is simply `testgmail`.

From there, we compute the SHA-256 hash of `testgmail`, then use this hashed output as our result. SHA-256 has been chosen for a few reasons. First, it is as open a standard hash as you can get, and nearly any crytographic library will have support for it. There are countless implementations of the algorithm in nearly every programming language imaginable. `sha256sum` is even included in the GNU coreutils. In other words, it seems fairly safe to say I will be able to generate a SHA-256 hash for as long as I need (or until quantum computing renders passwords useless).

As well, SHA-256 produces a 64 character hash, which satisfies our long password length requirement, and each hashed phrase will be unique, satisfying that requirement as well. There are certainly more complex hashing algorithms, but this method should be sufficiently complex. Our `testgmail` hash would be:

`b0d3f7a7cd0bed0cb44945e0940844153834c642c3e14a1091f5f468735f0739`

Which zxcvbn simply estimates as taking "centuries" to crack, and it could be reasonably estimated that it would take trillions of CPU years to find our initial secret phrase.

Yet, while the entropy created by this hash is certainly useful for it being a strong password, this method's other features are an even better bonus. Because no 2 SHA-256 hashes are alike, each secret phrase produces a different password to use. As well, you needn't remember anything beyond your single secret word, and you can simply copy/paste the hash with no knowledge of what the password actually is.

Notably, our SHA-256 hash will only be alphanumeric characters, which presents some annoyances with sites that require special characters. As well, not every site allows a 64 character password. Often, sites with shorter passwords will set a character field limit on their login forms, so a simple copy/paste still works fine, but some do not set this limit, making inputting a password difficult.

To fix the special character issue, I have always just appended an '@' character to the password to fit the password conditions, but this requires me to remember another thing. As well, sites with short password length allowances require me to remember the correct length for that specific site.

My current method for generating passwords is a simple Bash script:

I wanted to create a simple password generator that can also remember sites that require specific password lengths or special characters being needed. To do this, I've started rewriting my previous Bash script as a Rust program.
