---
layout: post
title: Setting up GNU pass with Alfred
category: blog
tags: [programming, tools, osx]
---

File this under the category of 'Notes for my future self so I don't have to figure this all out from scratch again'. Hopefully it will be useful for someone trying to do a similar thing.

I was trying to set up [pass](https://www.passwordstore.org/), an open source password manager, exactly the way I wanted it:

- gpg-agent should remember my master password until I put my computer to sleep, even between two different terminal sessions.
- a hotkey to search (with autocomplete) through the available accounts and put the password on my clipboard.

Really those don't seem like such tough requests. Here's what I did.

1. Install pass with `brew install pass`. Nothing too complicated here. Add some passwords so you can test out the other steps.
2. Install GPG 2.1 or higher with `brew install homebrew/versions/gnupg21` (the name might have changed by the time you're reading this). This simple step belies the difficulty of getting here. There were many false starts and attempts to read and write ENVFILEs before I found this line in [the documentation](https://www.gnupg.org/documentation/manuals/gnupg/Agent-Options.html): "Since GnuPG 2.1 the standard socket is always used." This is how we're able to avoid typing in the master password in every individual terminal window.
3. Edit your `~/.gnupg/gpg-agent.conf` (create that file if it doesn't already exist) to include the line `max-cache-ttl 7200`. Choose a number of seconds that works for you. After that much time has elapsed, gpg-agent will forget your master password.
4. Install sleepwatcher with `brew install sleepwatcher`. Sleepwatcher will let us run a script when the computer goes to sleep and when it first wakes up.
    1. As homebrew recommends at the end of the install, you'll need to create some symlinks from the LaunchAgents directory to the homebrew folder. You only need the one that ends with `-localuser.plist` and lives in yoru `~/Library/LaunchAgents`. This is because you're only setting up sleepwatcher to run for your user.
    2. Run `brew services start sleepwatcher`, again as per homebrew's recommendation. This is a replacement for `launchctl load ...` that you may see on other tutorials.
    3. Create your 'on sleep' script by running `echo 'killall gpg-agent' >> ~/.sleep && chmod 700 ~/.sleep`.
    4. Create your 'on wake' script by running `echo 'eval $(gpg-agent --daemon)' >> ~/.wakeup && chmod 700 ~/.wakeup`. I found that I also had to add `eval $(gpg-agent --daemon 2>/dev/null)` to my `~/.zshrc`.
    5. Modify your `~/.wakeup` to include a line that you'll notice, to make sure it's truly running when your machine wakes up. I used `say 'goodmorning'`. Do a similar thing for `~/.sleep`, but `say` won't work, as it seems that the audio doesn't run before the computer goes to sleep. Do something like `date >> ~/Desktop/sleepytime`. Put your computer to sleep and wake it up, and make sure that you heard/saw your sentinels to tell you that your scripts are running.

At this point, you could call it quits. The instructions so far are sufficient to avoid typing your master password more often than necessary. The rest of the instructions cover how to get the hotkey and autocomplete behavior, like 1Password has.

1. Install Alfred if you don't already have it. We're going to use [CGenie/alfred-pass](https://github.com/CGenie/alfred-pass) to get the hotkey and autocomplete behavior. It's a custom Alfred workflow, so you'll need to spring for the paid version.
2. Install pinentry-mac with `brew install pinentry-mac`. Configure gpg-agent to use this for entering your master password by adding the line `pinentry-program /usr/local/bin/pinentry-mac` to your `~/.gnupg/gpg-agent.conf`. You'll have to restart gpg-agent for this to take effect. Luckily, you can now do that by putting your computer to sleep and waking it back up.
3. Install [CGenie/afred-pass](http://www.packal.org/workflow/pass-0).
4. From the Alfred preferences, go to workflows. Right click on 'pass' and go to open in finder. Open up `pass-show.sh` in a text editor.
    1. Comment out the line at the bottom that says `pass show -c $QUERY` by putting a `#` at the front. I found that using the `pass -c` flag meant I could only get one password every 45 seconds -- I had to wait until `pass` cleared my clipboard for some reason.
    2. Uncomment the line above it and remove `| pbcopy` from the end. That is, change it from `#pass show $QUERY | awk 'BEGIN{ORS=""} {print; exit}' | pbcopy` to `pass show $QUERY | awk 'BEGIN{ORS=""} {print; exit}'`. We're doing this because one of the coolest features of Alfred is clipboard history, but using `pbcopy` this way puts your passwords into that history. That's no good! We'll get Alfred to put it on your clipboard but not your history in the next step.
    3. In the previous step, we modified the script to echo your password to stdout instead of putting it on the clipboard. Now, right click on the workflow screen in Alfred, choose Outputs > Copy to Clipboard.

![]({{ site.url }}{{ site.baseurl }}/images/alfred_pass_copy_to_clipboard.png)

Check the box for 'Mark item as transient in Clipboard' and save. Drag the little nubbin on the right of the 'run script' box to the new clipboard box. It should look like this when you're done.

![]({{ site.url }}{{ site.baseurl }}/images/alfred_pass_wire_clip_action.png)

And we're done. Now you can bring up Alfred with your hotkey, type pass, and you'll have autocompleted passwords on your clipboard, but not your clipboard history.

![]({{ site.url }}{{ site.baseurl }}/images/alfred_pass_finished_product.png)
