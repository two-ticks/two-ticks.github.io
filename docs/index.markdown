---
layout: default
---

{% include sidebar.html %}

## Hi there! :wave:

Welcome to my blog! Here I share my thoughts and ideas. I will be posting about my research, my projects, and my experiences.

## Posts

{% for post in site.posts %}

:bookmark_tabs: [ {{ post.title }} ]({{ post.url }})
{% endfor %}
