---
layout: post
title: Alpine.js vs Stimulus
category: blog
tags: [javascript, alpinejs, stimulus]
draft: true
---
- relax and let your json be html-encoded. Because it's in an attribute, that's the right thing to do. Not like this `x-data="{ scopes: <%= scopes.map{|s| [s, t('main_menu.' + s)]}.to_h.to_json.gsub('"', "'").html_safe %> }"`, but like this `v-data="{ scopes: <%= scopes.map{|s| [s, t('main_menu.' + s)]}.to_h.to_json %> }"`

- Use a translation `t` when necessary. Not like this https://clip.brianschiller.com/QSc4aJs-2021-10-27.png, but like this

- Keep syntax in attributes simple—they don't get transpiled so you may not be able to use spread syntax, method shorthand, etc depending on the browsers you're trying to support.

- x-cloak
