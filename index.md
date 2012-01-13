---
layout: index
title: blog.lizconlan.com
---
<div markdown="1" class="intro">
  Hello. I finally nailed a "proper" blog together. I'll warn you now it's fairly geeky but hopefully reads less like an instruction manual than the previous attempt.
</div>

Speaking of previous attempts, [the sandbox is still up](http://lizconlan.github.com/sandbox). Amazingly some people are actually reading it.

Anyway, for what it's worth, here's some stuff what I wrote...

## The posts

{% for post in site.posts limit:50 %}* [{{ post.title }}]({{ post.url}}) - {{ post.date | date: "%b %Y" }}
{% endfor %}

## wot no open source?

Sorry. The tech for this one is the same as before - Jekyll on Github; nice and simple, automagic versioning, editable anywhere (and over the web if I really have to). Might write something over the top of it for editing purposes that the web bit a little more streamlined but that's for another day. If I get it working, I'll write about it here.

I've not avoided open source because I don't want you to copy but because I want to keep my drafts in private but still in git and couldn't think of another way of doing it. Structurally it's very similar to [the Sandbox](http://github.com/lizconlan/sandbox) (see, another reason for keeping them separate!) but using MarkDown instead of Textile for reasons which escape me. Something something Github issues lists. I think. Or maybe I made that up.