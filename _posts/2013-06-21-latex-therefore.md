---
layout: post
title: LaTeX Therefore package
description: "This LaTeX package alleviates the constant struggle in mathematical writing of finding synonyms for 'therefore'."
modified: 2013-07-24
category: work
tags: [latex, proofs, math, fun]
mathjax: true
image:
  feature: texture-feature-05.jpg
---

Tired of needing to come up with synonyms for 'Therefore,' in your mathematical writing? Ponder no longer! This LaTeX package provides a macro to carefully select one of a multitude of choices. 

This project was inspired by [this thread](http://www.reddit.com/r/math/comments/1gov71/mathematical_writing_synonyms_for_therefore/),  particularly 

```
lucasvb: Someone should collect these into a macro.
    perpetual_motion: We need a command that would just insert one at random.
```

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/latex-therefore-sqrt-2.png">
    <figcaption>As an example, here is a proof that \(\sqrt{2} \) is irrational</figcaption>
</figure>

Using the package is easy. Download the file [therefore.sty](https://raw.github.com/bgschiller/latex-therefore/master/therefore.sty). In the preamble of your LaTeX document, enter

```latex
\usepackage{therefore}
```

Wherever you would otherwise laboriously choose a synonym for 'Therefore', simply type

```latex
\Therefore there are an infinite number of primes.
```

#### Available Synonyms

* Therefore,
* Hence,
* So,
* It is trivial that
* Clearly,
* Behold!
* Ergo,
* And verily it goes that
* Thus,
* By logical extension,
* And verily,
* It is the will of the Gods that
* We find
* It can be shown that
* It transpires that
* As an exercise, prove that
* Wherefore said He unto them,
* As must be obvious to even the meanest intellect,
* The power of logic reveals the conclusion that
* This implies
* As Gauss proved,
* As Euler proved,
* And it was handed down from the heavens that
* It pleases the symmetry of the world that
* Consequently,
* Accordingly,
* For this reason,
* If there is any justice in the world,


