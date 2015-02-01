---
layout: post
title: Minishell
description: "As part of the UNIX Systems class at WWU, each student makes a basic shell program, msh."
modified: 2013-07-24
category: work
tags: [unix, minishell, c, shell, wwu]
comments: true
image:
  feature: texture-feature-05.jpg
  thumb: latex-presentation-wf.jpg
---

Phil Nelson's UNIX Systems class at WWU guides each student through a series of six assignments, each adding more functionality to a basic shell program. At it's start, <code>msh</code> can <code>fork</code> and <code>eval</code>, but not much else. By the end of the 10 week class, both the minishell and the students are much more capable. 

###Features Implemented

* Command-line parameters (inluding quoted strings and escape characters)
* Comments
* Built-in commands
* Environment variable expansion
* Nested commands-- for example, <code>envset file_contents $(cat ${FILENAME})</code>
* Signal handling
* Pipelined commands
* Input and output redirection
* If statements and While loops (this implies that the shell interpreter is [Turing Complete](http://en.wikipedia.org/wiki/Turing_Completeness)!)
* Various other expansions (for example, <code>$?</code> is the exit code of the last command and <code>$$</code> is the PID.)

Because this was a school project and the class is still being taught, I can't provide source code here. However, if you're interested (and _not_ a student at WWU!), send me an email about it.

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/minishell_screencap.jpg">
    <figcaption>A test run of the minishell exercising some of its capabilities.</figcaption>
</figure>

## Sieve of Eratosthenes
Most of the grade for each minishell assignment is determined by a test script. The scripts are provided by the professor and they exercise the features required at each step. The test scripts are famously picky.
 (For example: I once struggled for hours to figure out why a particular line of my output was getting rejected. It turned out I was outputting the NUL-character in my NUL-terminated string. Since NUL is not printed, my output looked identical but was rejected by <code>diff</code>). 

 Once I'd passed the final test script, I could have called it quits. However, I decided to play with my newly finished minishell and write a test of my own.

With the addition of If statements and While loops, the language interpreted by the minishell is now [Turing Complete](http://en.wikipedia.org/wiki/Turing_Completeness). This means that it is (theoretically) possible for <code>msh</code> to compute anything that can be computed. I decided to take advantage of this, and implemented the [Sieve of Eratosthenes](http://en.wikipedia.org/wiki/Sieve_of_Eratosthenes) in the language defined by my minishell. 

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/msh-sieve.png">
    <figcaption> A test run of the sieve program </figcaption>
</figure>
Implementing this algorithm in the restricted environment of the minishell involved some complicated workarounds. For example, the only variables we have are environment variables (strings). Since the algorithm calls for an array of numbers, I use a space-separated string of base 10 numbers. To get the first element of this 'array', I used <code>sed</code> and a regex capture for a space-terminated series of digits. Anyway, here is the code, in all its ugliness.

{% gist 6329851 %}


