---
title: Orgmode Parsing Experiments
subtitle: Adventures in parsing org documents.
date: 2021-04-21
tags: [programming, clojure, clojurescript, jamstack, org-mode]
id: parsing-org-experiment
---

Org mode is an amazing way to write documents. It has a plethora of features that make it just a treat to use.

# Trying different parsers.

I spent a lot of time looking for a good parser that would turn orgmode documents into usable data. Well I found a lot!

## 200ok-ch/org-parser

I figured the easiest solution would be to use a Clojure library for this. Unfortunately, [this guy's library](https://github.com/200ok-ch/org-parser) did not work as well as I had hoped. It would not properly parse basic things like unnumbered lists and source code blocks due to a bug. Since this was the best Clojure parser, I decided to try looking for a way to use a node package.

## orgzly/org-java

This parser was designed for a pretty cool android app. Check it out [here](https://github.com/orgzly/org-java). Doesn't really fit my purposes. Unfortunately, there's little to no documentation on how to use this library. I don't know how Java works so I won't be able to understand what anything does, let alone how to interop with it. 
## rasendubi/uniorg

This parser was written in typescript, available on npm, and parses in the same way org-mode parses. Luckily, after the original creator worked out a fatal bug, I finally got a parser that works really well! 

Update: 2021-7-11 I created a command line utility that converts Org documents to html and JSON or EDN, whichever you like. You can get it with a simple `npm install uniorg-util`. 

Here's links to the project:

- [npmjs.com](https://www.npmjs.com/package/uniorg-util)
- [github.com](https://github.com/wildwestrom/uniorg-util)

I like how my solution came out, but I think I could have my parsing done on the client to simplify the build process.

Also, I can't use `\\`{.verbatim} to force a line break. I'll have to
see what I can do about that.
