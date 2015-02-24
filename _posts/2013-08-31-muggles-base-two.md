---
layout: post
title: Muggles and Base Two
description: "You can be half muggle, quarter muggle, or even 13/16 muggle. But can you be 1/3 muggle?"
category: blog
tags: [math, wizards, fun, muggles, teaching]
mathjax: true
feature_image: hogwarts-express.jpg
lighten_text: true
---

One day, my friend Matt Eschbach asked me, "So you can be 1/2 muggle-born, or 1/4 muggle-born. But can you be any proportion? What about 1/3?" The question turns out to have a really elegant answer, but let's work our way up to it. (Note: In order to simplify the math, we will not consider squibs as part of the analysis. I hope those readers will not feel too marginalized by this decision.)

<figure class="half">
    <img src="{{ site.url }}{{ site.baseurl }}/images/muggles/half.jpg">
    <img src="{{ site.url }}{{ site.baseurl }}/images/muggles/quarter.jpg">
    <figcaption> A possible parentage resulting in 1/2 and 1/4 muggles.</figcaption>
</figure>
 
It is straightforward to come up with a lineage resulting in someone who is 1/2 muggle, such as Seamus Finnegan, or even 1/4 muggle. In fact, any fraction of the form \\(\dfrac{a}{2^n}\\), where \\(a\\) is an integer has some lineage that produces it. To see why, consider 13/16.

<figure class="seventy" >
    <img src="{{ site.url }}{{ site.baseurl }}/images/muggles/three_sixteenths.jpg">
    <figcaption> A lineage resulting in a wizard who is 13/16 muggle </figcaption>
</figure>

This isn't the only way a person could end up 13/16 muggle. For example, the parentage below would also work. However, it's not as neat. For one thing, it has more people in the picture. Also, it is repetitive: the grandparent who is 1/4 muggle is the child of two great-grandparents, each 1/4 muggle.

<figure class="seventy" >
    <img src="{{ site.url }}{{ site.baseurl }}/images/muggles/three_sixteenths_alt.jpg">
    <figcaption> A more complicated lineage, also resulting in a wizard who is 13/16 muggle</figcaption>
</figure>

Since two parents who are each 1/4 muggle will have children who are 1/4 muggle, this can go on and on-- farther back than even Bathilda Bagshot can keep track! So let's stick to the minimal lineage.

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/muggles/three_sixteenths_alt2.jpg">
    <figcaption> A lineage so complicated not even Bathilda Bagshot can keep track.</figcaption>
</figure>

But *what* (Ron might ask) does any of this have to do with base 2? Well, it's not much more than the fact that we each have two parents. And each of those parents has two parents. And... so on. At level \\(n\\), the number of ancestors you have is \\(2^n\\). So if you're looking for a person who is 13/16 muggle-born, you need someone for whom 13 of their 16 great-great-grandparents were muggles.

Maybe it's easier to look at it from another perspective. Let's go back up the family tree far enough that everyone on that level is either 100% wizard, or 100% muggle. Consider Miracle Max, a wizard at this level on the family tree. Max can see into the future, and wants to calculate what his contribution to his descendants' wizardliness will be.

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/muggles/miracle_max.jpg">
    <figcaption>Great-great-great grandad Miracle Max and his wife Valerie.</figcaption>
</figure>

His children will derive half of the wizardliness from him (and half from his wife Valerie). Those children will pass along half again of that half to their own kids. Miracle Max's descendants will get 1/2, 1/4, 1/8, ... of his wizardliness. Those numbers are all of the form \\(\frac{1}{2^n}\\). 

This is true for *each* ancestor at this level: they contribute \\(\frac{1}{2^n}\\) to your wizardliness. Your total wizardliness is the sum of all these contributions. If we add up a bunch of \\(\frac{1}{2^n}\\) terms, the result will have the form \\(\frac{a}{2^n}\\), or 'some number over a power of two'.

### A Refresher on Fractional Binary

We're used to writing numbers in decimal (base 10), where \\(0.abc\\) would mean \\(\frac{a}{10} + \frac{b}{10^2} + \frac{c}{10^3}\\). In binary (base 2), \\(0.abc\\) would mean \\(\frac{a}{2} + \frac{b}{2^2} + \frac{c}{2^3}\\). So we could write 5/8 as \\(\frac{1}{2} + \frac{0}{4} + \frac{1}{8} = 0.101_2\\). (The little subscript 2 is saying 'this number is written in base 2')

There are some numbers that are hard to write down in decimal form (eg, \\(1/3 = 0.3333 \ldots \\)). We need a dot-dot-dot in order to fully capture the number. Binary also has numbers like this: \\(2/5 = 0.0110011001100\ldots \_2\\). In fact, the only numbers that you _can_ write without a dot-dot-dot in binary are \\(\dfrac{a}{2^n}\\). 

### An Easier Way
If you were paying close attention, you may have noticed that I didn't include 16 great-great-grandparents in the first genealogy tree. To save myself the drawing, I used the trick of writing 13/16 in binary:
\\[
\frac{13}{16} = 0.1101_2
\\]
The binary form tells us that in order to be 13/16 muggle-born, we need \\(\frac{1}{2} + \frac{1}{4} + \frac{0}{8} + \frac{1}{16}\\) of the ancestry to be of muggle origin. In other words, we need 1 parent, 1 additional grandparent, no additional great-grandparents, and 1 additional great-great-grandparent.

<figure class="seventy">
    <img src="{{ site.url }}{{ site.baseurl }}/images/muggles/three_sixteenths_num.jpg">
    <figcaption>The binary representation of 13/16 corresponds to a simple way to draw the family tree of someone who is 13/16</figcaption>
</figure>

### So what about 1/3?
We'll use the trick we've developed and look at the binary representation of the fraction. To write 1/3 in binary, we need to keep writing 1s and 0s forever. Specifically, 
\\[
\frac{1}{3} = 0.01010101010\ldots\_2 = \frac{0}{2} + \frac{1}{4} + \frac{0}{8} + \frac{1}{16} + \ldots
\\]
A wizard who was 1/3 muggle-born would need to have an infinite ancestry!

