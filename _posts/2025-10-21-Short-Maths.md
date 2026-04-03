---
layout: post
title: Some Short Math Tidbits
date: 2025-10-20 23:44:00 +0530 
categories: jekyll update
---
{% include analytics.html %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Choose one, leave one

{% raw %}
$$\binom{n}{k}= \binom{n-1}{k-1} + \binom{n-1}{k}$$
{% endraw %}

This could be thought of as choosing one item and leaving one item while choosing $$k$$ items from $$n$$ items. 

Choose one item: We have $$\binom{n-1}{k-1}$$ ways to choose remaining $$k-1$$ items from $$n-1$$ items
Leave one item: We have $$\binom{n-1}{k}$$ ways to choose $$k$$ items from remaining $$n-1$$ items. 


# Interchanging the order of summation

{% raw %}
$$\sum_{i=0}^{n} a_i \sum_{j=0}^{i} b_j = \sum_{j=0}^{n} b_j \sum_{i=j}^{n} a_i$$
{% endraw %}

