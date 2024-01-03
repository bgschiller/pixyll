---
layout: post
title: "Behind the Scenes: How We Fixed Leftover Underlines in the Grammarly Text Editor Plugin"
category: blog
tags: [programming, grammarly, webdev]
---

_This blog post was originally written for the Grammarly for Developers blog, and was published there on November 8, 2022. Grammarly for Developers has been discontinued, but the post lives on :)_

![]({{ site.url }}{{ site.baseurl }}/images/g4d-editor/G4D_Blog_Thumbnail_C7-3.png)

While building the Grammarly Text Editor Plugin, we sometimes come across interesting challenges when dealing with corner cases. Often, there’s no perfect answer to the problem; instead, the right approach requires a careful blend of engineering and design to achieve the best possible outcome.

To show you how we handle these situations, here’s the behind-the-scenes story of one such challenge, the options we considered, and why we believe our approach is the best solution for developers.

## The problem: leftover underlines

Recently, while the Grammarly for Developers team was preparing for a demo, we found what looked like a glaring bug.

Our demo was a mock social network where you could write a short post in a text editor and submit it. Since the editor was using the Grammarly Text Editor Plugin, underlines would appear showing Grammarly’s writing suggestions for the user’s text.

When the user clicked “Post Inspiration,” the application would clear the text so they could make another post. However, Grammarly’s underlines from the previous post were still visible in the editor, even though the underlying text was gone.

<figure>
<img src="{{ site.url }}{{ site.baseurl }}/images/minishell_screencap.jpg">
<figcaption>Grammarly has highlighted a mistake in the entered text. When the user clicks the “Post Inspiration” button, the editor returns to its empty state with placeholder text, but the previous underlines remain.</figcaption>
</figure>

This obviously wasn’t the correct behavior, so we filed a bug report and started working on it.

The core problem is that in order to update the underlines, the plugin has to know when the text in the editor has changed. The plugin has event handlers for changes made within an editor, as the editor will automatically emit events as the user types. However, when a JavaScript function updates the text—in this case, this happens when the “Post Inspiration” button clears the box—it turns out the element doesn’t emit an event. We needed another way to handle this case.

## Potential solution 1: MutationObserver

Our first solution was to watch for changes to the text editor element’s value property, i.e., where it stores the text currently inside it.

We tried to use [MutationObserver](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver), an interface that allows you to watch for changes in the DOM tree. Unfortunately, it can only observe changes to attributes, not properties, so this didn’t work.

## Potential solution 2: Object.defineProperty

While researching why MutationObserver didn’t work, I found a clever solution on Stack Overflow. We could redefine the text editor’s value property using `Object.defineProperty` so that any updates to value also emitted an event. I wrote up some code and got it working. Great!

However, after consideration, we decided this wasn’t a viable solution for our plugin. This approach would be too invasive. What if an app used `Object.defineProperty` itself or relied on the original identity of the value property? Our plugin needs to be respectful of the host JavaScript environment. Even though this approach would work well for an application in control of its JavaScript environment, it wasn’t available to us.

## Potential solution 3: polling for changes

Next, we considered using `requestAnimationFrame` to poll the element for changes to the value. This approach would check `value` every ~16 ms, compare it to the previous value, and update the underlines if the plugin saw a change.

We also ruled out this approach, though. Our existing handlers already pick up most of the changes to the element’s value. Adding a polling loop would ask the browser to do a lot of extra work to cover a rare edge case, needlessly slowing down the browser and potentially draining the user’s battery as well.

## Our solution

It seemed no solution could completely solve the problem and fit all our constraints at once. However, if we considered user behavior, we realized that the most common situation where this bug would occur was when clicking something like a submit or clear button. Rather than cover every case with a suboptimal solution, we could cover the most important cases with a good solution.

To handle this, we added a [`blur` event](https://developer.mozilla.org/en-US/docs/Web/API/Element/blur_event) listener. This event fires after the text editor loses focus—in this case, because the user has clicked elsewhere—and then we can check if the editor’s value has changed.

This solution solves the problem for the most common use patterns without being invasive or wasting resources. Now, most developers using the plugin will never have a problem.

If you still see leftover underlines in your application, it’s possible our heuristic doesn’t cover your situation. You’re in control, though! We can’t solve this problem from within the plugin, but you can add a simple fix by dispatching an explicit [`change` event](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/change_event) whenever the code sets the editor’s value. Here’s an example function that stores the value of the text area in a database, programmatically clears the text area, and then dispatches the change event that the plugin is listening for:

```js
form.addEventListener("submit", (evt) => {
  evt.preventDefault();

  storeInDatabase(textarea.value);
  textarea.value = ""; // clear the textarea
  textarea.dispatchEvent(new Event("change")); // fire the
  //‘change’ event that the plugin is listening for
});
```

This change event ensures that the plugin will notice the programmatic change and can clean up the underlines from the previous text.

Throughout this process, we needed to balance multiple constraints and do what was best for our developers and end users. In the end, we found a solution that works great in most cases and is easy to handle in the few remaining edge cases. Whatever your application is, you can trust that we’re working hard to make it as easy as possible for you to enhance your app with Grammarly.

~~If you need help with your edge case, just ask us a question in the [Grammarly for Developers GitHub repo](https://github.com/grammarly/grammarly-for-developers/discussions). And if you’re not using Grammarly for Developers yet, learn more about it [here](https://developer.grammarly.com/)!~~ _As mentioned above, Grammarly for Developers is no more_
