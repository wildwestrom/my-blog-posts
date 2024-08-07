---
title: Problems with Emacs/Spacemacs
subtitle: Emacs has got some problems, and I have no idea what to do about it.
date: 2021-07-18
tags: [programming, emacs, spacemacs, editors, tooling, IDE, emacs-lisp]
id: rant-on-emacs
---

# Why is Emacs Great?

People usually cite the same few positives when talking about how great Emacs is.

The reasons are typically:

- Extensibility to the core.
- The extension language is a LISP.
- Freedom! It's GNU GPL Licensed.
- Keyboard centric.
- Killer features:
  - Magit
  - Org-mode

As I type this out within my configuration of Spacemacs, I will rant about the problems I encounter with it. I will also explain...

- why maintaining GNU Emacs as it is, will not be a part of the solution.
- why other editors will not be a part of the solution.

First, I am not an Emacs evangelist. I came to Emacs because I was looking for a keyboard centered editor with good Clojure support. Before this I was a Vim user. Copy-pasting scripts and adding plugins. I was using Vim for everything. Documents, config files, LaTeX typesetting. I had `hjkl`{.verbatim} thoroughly embedded into my muscle memory. I love the Vim bindings. I even dabbled with the idea of maintaining a [Vimwiki](https://vimwiki.github.io/). I initially used Markdown as a way to keep it all organized, but then Org-mode caught my eye.

## ...and Then I Found Spacemacs

Once I found Spacemacs, it was all over. It had everything I could have wanted.

- Superb Vim emulation, better than Vim itself!
- Org-mode! It truly is a killer-feature.
- Magit, the best git interface; Lazygit comes close, yet so far.
- Consistent and ergonomic bindings.
- Lots of nice features we're accustomed to in IDEs.
  - Syntax highlighting
  - Autocompletion
  - Debugging and compiling easily accessible
  - Built in terminal
  - File trees: Neotree and Treemacs
  - Project management: Projectile
- Freedom. It's GNU GPL licensed. It's free software free forever!

# The Problems

## It's Slow and Buggy

Spacemacs runs okay most of the time, but the performance is a huge bottleneck, particularly when working with Clojure. If I have LSP mode on and open a Clojure buffer with 200 or more lines, scrolling slows down to a crawl.

Now this could just be a problem with the implementation of Clojure LSP as CIDER seems to have much better performance. However, with just CIDER, the performance still sometimes isn't quite where I'd like it. Every other editor has no problems with performance like Emacs does.

The reasons for this are out of scope, but nevertheless, this is nearly unacceptable.
I even installed the "native-comp" branch of Emacs 28, also known as "gccemacs". It gives marginal speed improvements. One big problem is that it halts when I try to evaluate a function that returns a very large collection or string. I believe this is a long standing issue with Emacs having trouble dealing with long lines; see [2018-06-29](http://ergoemacs.org/emacs/blog_past_2018-06.html) and [2019-06-07](http://ergoemacs.org/emacs/blog_past_2019-05.html) from Xah Lee's blog, and this [Reddit post](https://www.reddit.com/r/emacs/comments/6qpbka/elisp_for_text_processing_in_buffers/).
Even as I type now, I am feeling the lag and bugginess as I see my screen flash white or stop updating for seemingly no reason, only to go back to normal when I press a key.

## It's Arcane

Xah Lee explains this in much [more detail](http://ergoemacs.org/emacs/emacs_modernization.html). But basically, there are a lot of historical holdovers from the 1980s still in Emacs, and would take quite a lot of effort to change. This includes things like [keybindings](http://ergoemacs.org/emacs/emacs_kb_shortcuts_pain.html), [arcane terminology](http://ergoemacs.org/emacs/modernization.html), [terrible UI design](http://ergoemacs.org/emacs/modernization_menu.html), a [terrible manual](http://ergoemacs.org/emacs/emacs_manual_problem.html) (use [Xah's](http://ergoemacs.org/emacs/emacs_basics.html) instead) etc.

## There are no good alternatives

This is the worst part. Slowness, for something as simple as editing text would be unacceptable, if it weren't for all the amazing features and integration offered by the Emacs ecosystem.

# Some Potential Solutions

My frustrations with Spacemacs battle it out every day against the reasons I can't imagine leaving Spacemacs. While there are no quick and easy solutions, here are a few solutions I've thought of.

## Create a Minimal Configuration

Although the problems with performance largely stem from the age and single-threaded nature of the program, it can run faster and more stably with careful planning. I've toyed with the idea of hand-rolling my own Emacs configuration. I would rebuild Emacs with all the features I want from Spacemacs, then attempt to optimize for performance. I have no idea what packages or configurations are at play when I use the innumerable features of Spacemacs, but it doesn't seem impossible to recreate.

## Extend an Existing Editor

There are many nice editors out there that already tick off some of the items on my wishlist for a keyboard-based environment.

### VSCode

VScode has an addon called VSpaceCode which attempts to add similar ergonomics to Spacemacs within VSCode by having Vim emulation and mnemonic bindings for VSCode commands. One problem with this solution, is that it only works out-of-the-box in VSCode. I prefer to use VSCodium, which is just a binary of VSCode without all the Microsoft telemetry, logos, and other crap.

### Vim

There is a distribution of Vim called SpaceVim, that caught my attention awhile back. This has the advantage of not having to emulate Vim, as it is already Vim. It also does a really good job of emulating the behavior of Spacemacs, with its leader key and display of all available commands. One problem I did find was that it still ultimately relies on vimscript and Lua to extend it, hidden behind a verbose TOML config file. Perhaps it might be possible to use fennel instead once the backend of NeoVim is entirely Lua. Making a Lisp, any Lisp, the configuration language of Vim, may become the greatest thing to happen to Vim in the 21st century. This seems promising.

### Light Table

This IDE was written in Clojure and wholeheartedly adopts the idea of fast-feedback. Every step of the way you get feedback and documentation as your write and scroll through your codebase. As of right now, it's not quite dead, but in desperate need of emergency medical treatment. I could see this editor being extremely useful for many things that I work on. If Light Table could be made entirely controllable by the keyboard, I would be sold.

## Rewrite/Overhaul Emacs

I have not educated myself enough on the possibility of a major overhaul or rewrite of GNU Emacs. Depending on the scope of the project, it may take decades before we get something like true multi-threading. When that does happen, you can guarantee I'll be using Emacs as my window manager.

# Conclusion

I'm probably still going to be using Spacemacs until I am either skilled enough or frustrated enough to implement solutions to some of these problems. For now, I'll just keep chugging along with this decades old freedom-respecting piece of software.
