---
layout: default
title: Posts
permalink: /posts/
---

## Posts

{% for post in site.posts %}
:bookmark_tabs: [ {{ post.title }} ]({{ post.url }})
{% endfor %}
