---
layout: post
title: "rref in GF(2) in Julia"
date: 2024-01-07 03:44:00 +0530
categories: jekyll update
---
{% include analytics.html %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>


{% highlight julia %}
using AbstractAlgebra
F = GF(2)
A = rand([0, 1], 5, 5) 
B = matrix(F, A) # Convert to GF(2) matrix
rref(B)
{% endhighlight %}
