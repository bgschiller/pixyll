---
layout: post
title: Where's the Facebook app?
description: "Disappointed by the lack of an official Facebook app for your Mac? Make your own! (download provided for the lazy)"
category: blog
tags: [programming, fun, facebook]
image:
  feature: texture-feature-cobbles.jpg
---

Some time ago, my (now) mother-in-law switched to a mac. She had already been using an iPhone for a while, so it was a familiar interface. One thing that was unfamiliar: there's no Facebook app! 

It does seem a serious oversight on Facebook's part. Well, luckily it's not too difficult to rectify. We'll walk through creating a very comprehensive Facebook app. This is an OS X-specific tutorial, though I may make a cross-platform version in the future.

Open up a terminal and type `open https://www.facebook.com`. Facebook should open in a new tab in your default browser. Great! 

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/fbt/mvp.png">
    <figcaption>Looks like we've got our minimum viable product!</figcaption>
</figure>

Open up the AppleScript Editor and type

```applescript
do shell script "open https://www.facebook.com"
```

It should look like this:

<figure>
    <img src="{{ site.url }}{{ site.baseurl }}/images/fbt/applescript.png">
    <figcaption>Not only is the code clean, but it scales well too.</figcaption>
</figure>

Now save that file, but where it says File Format, choose Application.

<figure>
	<img src="{{ site.url }}{{ site.baseurl }}/images/fbt/save_applescript.png">
</figure>

This is a working application &mdash; try opening it. Facebook should spring fully-formed into your web browser. We could ship this now, but I think it lacks a certain aesthetic touch.

Find a nice looking version of the facebook logo. Here's one that's 800x800 pixels:

<figure>
	<img src="{{ site.url }}{{ site.baseurl }}/images/fbt/fbl.png">
</figure>

Open up this logo, select-all (⌘ + a), and copy (⌘ + c). Now right-click (or control-click) your app, and choose 'Get Info'. 

<figure>
	<img src="{{ site.url }}{{ site.baseurl }}/images/fbt/get_info_highlight.png">
</figure>

Click the icon in the top left corner. This should give it a blue border, indicating that it's highlighted. Now paste the logo (⌘ + v). Close out of the 'Get Info' window, and you're done!

All joking aside, this is could be a useful technique for setting up a computer for a relative who's not very computer-savvy. You can even drag the icon to the dock to make it easy to find.
<figure>
	<img src="{{ site.url }}{{ site.baseurl }}/images/fbt/dock.png">
</figure>

<br>
<br>
As promised, a link for the lazy: <a href="{{ site.url }}{{ site.baseurl }}/extras/facebook.zip" class="btn">download the facebook app</a>