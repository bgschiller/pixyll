---
layout: post
title: Programming on MacOS in 2018
category: blog
tags: [programming, macos]
---

Many of my coworkers are about to switch to macs for their development machines, so this seems like a good opportunity to make recommendations for MacOS software that I find useful.

### Homebrew

The missing package manager for mac. This is essential for programmers. Many software you'll want to download is easiest to install using homebrew. Get it at [brew.sh](https://brew.sh/).

### Spectacle

Super fast keyboard-driven window manager. Free yourself from the tyranny of manually resizing windows. This is the app I miss the most when I'm using someone else's computer. Get it at [spectacleapp.com](https://www.spectacleapp.com/).

### oh-my-zsh

A better terminal experience. I have a hard time describing precisely what's better about zsh over bash. It's almost the same, but just works... better. Tab completion in particular is more intelligent.

I use [oh my zsh](https://ohmyz.sh/) with pretty much all defaults, and it gives me

- Autocomplete for my git branch names
- Git info on my prompt: whether there are uncommitted changes, and the name of my current branch.
- A symbol that is green if the last command I ran was successful, red if it failed. This saves me from nervously checking `echo $?` to see exit codes.

I also installed [fzf](https://remysharp.com/2018/08/23/cli-improved#fzf--ctrlr) as a replacement for the built-in ctrl+r behavior (searching history of recently used commands). Install with `brew install fzf && $(brew --prefix)/opt/fzf/install`.

### Alfred

This is one of the only paid software on this list. There's a free version, but it doesn't do any of the good stuff. I use alfred primarily for

- Clipboard history. ⇧⌘v (shift + command + v) opens a dialog to let me choose from any of my recently copied items.
- [Clipbox](https://github.com/bgschiller/alfred-clipbox). Press a shortcut to quickly take a screenshot (⇧⌘x). It will be uploaded to trello, and the direct url placed on your clipboard. Also works for files and text (⇧⌘c), and screen recordings (⇧⌘g).
- [Password management](https://brianschiller.com/blog/2016/08/31/gnu-pass-alfred). Really GNU Pass is the brains behind my password management setup, but Alfred provides the UI.

There's a ton of other extensions, but I haven't taken the time to learn any of them.

## Depending on your work

These may not be necessary, depending on what you're working on.

### Python3

There are a ton of different ways to install python on a mac, as captured in [this xkcd](https://xkcd.com/1987/). I use

- homebrewed python3 (`brew install python`).
- pyenv (`brew install pyenv`), if I need a different version of python for a specific project.
- pipenv (`brew install pipenv`), to keep different projects dependencies separate from one another (eg, you might need to use Flask v0.12 for one project, and v1.0.2 for another).

To set up a pipenv using a specific version of python:

```bash
pyenv local 3.6.6 # or whatever version you need for this project
pipenv --python $(pyenv which python)
```

### Postico

The best darn database GUI I've ever used. Only for Postgres, though.