---
layout: post
title: Putnam Boxes and Balls
description: "Every December, undergrads across the country take the Putnam Exam, regarded as one of the toughest math competitions around. This is a walkthrough of a problem from the 2010 test."
category: blog
tags: [math, putnam, teaching]
mathjax: true
image:
  feature: texture-feature-02.jpg
  thumb: all-the-balls2.png
---

School starts soon at my alma mater, [WWU](http://www.wwu.edu/). For me, Fall quarter always meant I would be taking the Putnam Exam, regarded as one of the most difficult (or _the_ most difficult) math competitions for undergraduates. It consists of two three-hour sessions with six problems each. Most years, the median score on the exam is zero out of 120 points.

The first year I sat the exam, in 2010, this was one of the problems:

> There are 2010 boxes labeled \\(B\_{1}, B\_{2}, \ldots, B\_{2010}\\) and \\(2010n\\) balls have been distributed among them, for some positive integer \\(n\\). You may redistribute the balls by a sequence of moves, each of which consists of choosing an \\(i\\) and moving _exactly_ \\(i\\) balls from box \\(B\_i\\) into any one other box. For which values of \\(n\\) is it possible to reach the distribution with exactly \\(n\\) balls in each box, regardless of the initial distribution of balls?


### Break it down
Whew, what a mouthful! Let's break that into pieces.

> There are 2010 boxes labeled \\(B\_{1}, B\_{2}, \ldots, B\_{2010}\\)

So far, so good.

> ... and \\(2010n\\) balls have been distributed among them, for some positive integer \\(n\\).

Okay, so if \\(n\\) is 2, for example, there are 4020 balls. And all the balls are in some box or other.

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/all-the-balls2.png">
    <figcaption>"My game's got a basketball, a football, a kickball, and a tetherball: it's got <em>all</em> the balls. So I call it 'All The Balls'. You get it?" -Lawson</figcaption>
</figure>

> You may redistribute the balls by a sequence of moves,

That sounds good to me. What are the moves?

> each of which consists of choosing an \\(i\\) and moving _exactly_ \\(i\\) balls from box \\(B\_i\\) into any one other box.

So I could take 3 balls out of box \\(B\_3\\) and put them in box \\(B\_5\\). But if I wanted to take balls out of box \\(B\_5\\), I'd have to take them out 5 at a time. And I can do this as many times as I want.

> For which values of \\(n\\) is it possible to reach the distribution with exactly \\(n\\) balls in each box,

We're looking to divide the \\(2010n\\) balls evenly among all the boxes, but maybe it doesn't work for every \\(n\\)?

> regardless of the initial distribution of balls?

Okay, so my worst enemy could be stacking the deck against me, setting things up so in the worst way they know how, and I still have to be able to evenly distribute the balls.

### A smaller version of the same problem

It's a little hard to visualize 2010 boxes, so lets go with something smaller. How about five? Play with the demo below for a bit. You can move \\(i\\) balls from box \\(B\_i\\) by dragging them to another box. Can you get all of the balls distributed evenly? If so, try making it harder by lowering the 'Ball Multiplier' (that's the \\(n\\) from the \\(5n\\) balls we're working with).

<figure>
    <iframe src="{{ site.url }}{{ site.baseurl }}/extras/putnam_boxes_demo.html" width="100%" class="auto-height" style="min-width:513px;"></iframe>
    <figcaption>If you want to play with the number of boxes, this demo is <a href="{{ site.url }}{{ site.baseurl }}/extras/putnam_boxes_demo.html" target="_blank">also available here</a>, where it has a bit more room to breathe.</figcaption>
</figure>

If you played that for a bit, you probably developed some sort of strategy. (If you didn't, [go ahead]({{ site.url }}{{ site.baseurl }}/extras/putnam_boxes_demo.html); we'll wait). 

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/cos_b.jpg">
    <figcaption>We'll continue after a short JELL-O Pudding break.</figcaption>
</figure>

### Rescue missions
For me, a strategy that seems to work is to move balls towards box \\(B\_1\\), and redistribute from there one at a time. We can also use the balls in box \\(B\_1\\) to 'rescue' balls in another box. This is most easily understood with an example.

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/boxes/rescue_1.png">
    <figcaption>How are we going to get a ball out of box \(B_3\) </figcaption>
</figure>

In the game above, our \\(n\\) is 1, so we're trying to get 1 ball in each box. Box \\(B\_3\\), with two balls, has too many! We need to take one out, but the rules are that we have to take out 3 at a time. So, we will send a ball to 'rescue' the balls in \\(B\_3\\).

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/boxes/rescue_2.png">
    <figcaption>We moved a ball from \(B_1\) to \(B_3\). Now we have enough in \(B_3\) to move them all to \(B_1\).</figcaption>
</figure>

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/boxes/rescue_3.png">
    <img src="{{ site.url }}{{ site.baseurl }}/images/boxes/rescue_4.png">
    <figcaption>We move the balls from boxes \(B_2\) and \(B_3\) into \(B_1\).</figcaption>
</figure>

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/boxes/rescue_5.png">
    <figcaption>Now that we can move the balls individually, we evenly distribute them.</figcaption>
</figure>

Okay, so everything's peachy then, right? Not so fast. Are we always going to be able to execute a rescue mission like that? What could go wrong? I'll leave you to contemplate that while I admire this orange.

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/tan_gerine.png">
    <figcaption>Actually, I don't think it's an orange.</figcaption>
</figure>

### A wrench in the works

What if everyone needs a rescue, *and no one is there to help*? Box \\(B\_1\\) has 0 balls, \\(B\_2\\) has fewer than 2, \\(B\_3\\) has fewer than 3, and so on. 

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/boxes/stuck_1.png">
    <figcaption>Nobody here is planning on moving before the spring thaw.</figcaption>
</figure>

What can we do about this? Well, we could make sure that there are so many balls that this never happens. For this, it is helpful to think of a worst-case scenario. In the very worst case, with the most stuck balls, every box \\(B\_i\\) has \\( i -1\\) balls. There is no slack anywhere in the system.

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/boxes/stuck_2.png">
    <figcaption>Every box has one ball too few to be able to move them.</figcaption>
</figure>

In the 5 boxes case, the worst-case scenario involves 10 balls: \\( 0 + 1 + 2 + 3 + 4\\). We know that if the game comes to us in that state, we'll *never* get the balls evenly distributed. But if there's even one more ball, everything frees up. Wherever that extra ball lands, we can move the balls in that box into \\(B\_1\\), and from there execute rescue missions.

How many balls do we need for 2010 boxes? We still need one more than worst case of \\(0 + 1 + 2 + 3 + \\ldots + 2009 \\), but what is that sum? It turns out there's an easy way to count up all the numbers from 1 to 2009 (or really anything you like).

### How to count

Again, let's work with a smaller version of the problem. Let's add up the numbers from 1 to 10.

```
     1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + 10
```
Now, add another row below it, but reversed.

```
     1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + 10
    10 + 9 + 8 + 7 + 6 + 5 + 4 + 3 + 2 +  1
```
Each column sums to 11!

```
     1 +  2 +  3 +  4 +  5 +  6 +  7 +  8 +  9 + 10
    10 +  9 +  8 +  7 +  6 +  5 +  4 +  3 +  2 +  1
    -----------------------------------------------
    11 + 11 + 11 + 11 + 11 + 11 + 11 + 11 + 11 + 11 
```

Since there are 10 columns, we have \\(11 \cdot 10\\). But we needed two rows of \\( 1 + 2 + \\ldots + 10\\) to get to that, so we have to cut our \\(11 \cdot 10\\) in half: \\(\\frac{11 \cdot 10}{2} = 55\\).

Now, to get \\(1 + 2 + 3 + \\ldots + 2009 \\), we can imagine lining up our rows and sum each column.

```
        1 +    2 + ... + 2008 + 2009
     2009 + 2008 + ... +    2 +    1
     -------------------------------
     2010 + 2010 + ... + 2010 + 2010
```

So now we can say with surety, the greatest number of balls you can have where it's still possible to get stuck is \\(\frac{2010 \cdot 2009}{2} = 2019045\\). If you want to be sure you can always move balls around, you need at least 201904*6* of them.

### Wrapping up

We now know how many balls we need to have. But if you remember back, the question asked "For which values of \\(n\\)" is it possible to rearrange the balls. So we have \\(2010n\\) balls, and we need \\(2010n > 2019045 \\). Dividing both sides by 2010, we get \\(n > 1004.5\\). If \\(n\\) has to be an integer (we can't put 1004 *and one half* balls in each box!), we get \\(n \\geq 1005\\).

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/all-the-balls.png">
    <figcaption>No half balls here</figcaption>
</figure>

So there you have it. The only values of \\(n\\) for which it is possible to reach the distribution with exactly \\(n\\) balls in each of 2010 boxes are \\(n \\geq 1005\\). In case anyone asks.
