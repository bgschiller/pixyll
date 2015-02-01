---
layout: post
title: Wolfram Presentation at Strange Loop
description: "Wolfram Language has so much baked-in -- what's left to do?"
category: blog
tags: [programming, future, wolfram]
image:
  feature: Textile_cone.jpg
---

At Strange Loop today, Stephen Wolfram presented on his Wolfram Language. With its "over 5000 functions", the philosophy is to have as much as possible baked-in. This certainly makes for a good demo. Applause broke out at a number of points in his presentation, such as when he projected an edge-detected video of himself taken from his laptop's webcam.

```
Dynamic[EdgeDetect[CurrentImage[]]]
```

Another high point was when he queried Wolfram Alpha for a selection of paintings from Van Gogh and Picasso, labelled them with their creator, and called a function to produce a machine learning classifier using nothing but those 40 labelled examples. The Wolfram Language decided on using a decision tree (if I recall correctly), and apparently did its own feature extraction.

I might have spent the better part of a day (or several days) creating a similar classifier. And while my tuning and tweaking of the algorithm and meta-parameters might have produced a better classifier, maybe the defaults are good enough.

The existence of the Wolfram Language could mark a milestone for software. It nicely divides problems into solved and not-yet-solved. The solutions to solved problems can become reusable components, and we can reach for the stock solutions if that's all we need. Then we can give more attention and thought to the unsolved problems. (Though of course, its fun to build things from scratch, even if they exist already. But I think of the Markov text generation library I wrote as more of a craft project than as serious work.)

Maybe this is no different from using a library instead of writing your own sort routine. But the Wolfram Language pushes the boundaries so much farther that it's difficult to ignore.

I absolutely have some reservations about the platform, and they seemed to be shared by many folks in attendance today. Obviously, there is an issue of costs and licences. Also, it feels very totalitarian to have so much functionality wrapped up with a bow on it. Wolfram Language doesn't seem designed to have a community of people building things on top of it and sharing them on GitHub. When this was raised in the Q&A, Wolfram pointed to the [Wolfram Demonstrations Project](http://demonstrations.wolfram.com/), which is not at all the same thing as something like `npm` or `pip`. He elaborated that down the road, they might create some sort of "marketplace" for third-party code. Describing it as a "marketplace" seems to be missing the point.

Additionally, much of the functionality of the Wolfram Language seems to be available only with a strong internet connection. This was especially apparent on the shaky conference internet connection. 

On my way home today, I mentally sketched a nice python interface to mimic the `CloudDeploy[]` functionality that Wolfram demonstrated today (`CloudDeploy[]` hosts a bit of code and makes it available via a REST or web form interface). The Wolfram Language looks like part of the future of programming to me, and I'm excited to see some of the ideas it pioneers be picked up by open-source projects. And I'm _definitely_ excited to play with the language.

