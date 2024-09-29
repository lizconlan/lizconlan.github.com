---
layout: index
title: blog.lizconlan.com
---
<div markdown="1" class="intro">
  Hello. I finally nailed a "proper" blog together. I'll warn you now it's fairly geeky but hopefully reads less like an instruction manual than the previous attempt.
</div>

Speaking of previous attempts, [the sandbox is still up](http://lizconlan.github.com/sandbox). Amazingly some people are actually reading it so I've left it alone.

Anyway, for what it's worth, here's some stuff what I wrote...

## The posts

{% for post in site.posts limit:50 %}* [{{ post.title }}]({{ post.url}}) - {{ post.date | date: "%b %Y" }}
{% endfor %}

## Learning resources

{% for article in site.containers %}* [{{ article.title }}]({{ article.url}}) - {{ article.date | date: "%b %Y" }}
{% endfor %}

## New and improved - now 99% open source!

Took me a while but I eventually realised that I could hide the `_drafts` folder in its own private submodule! \*opens main repo\* (Excuse the mess - I may retrospectively merge a few commits.)
