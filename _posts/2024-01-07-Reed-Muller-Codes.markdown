---
layout: post
title: "Reed Muller Codes"
date: 2024-01-07 03:44:00 +0530
categories: jekyll update
---
{% include analytics.html %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

### Introduction 


$$RM(r, m)$$ is a binary linear block code $$(n, k, d)$$ of length $$2^m$$. $$RM(m, m)$$ is defined as the universe $$(2^m, 2^m, 1)$$ code and $$RM(0, m)$$ is defined as repetition code of length $$2^m$$. The remaining RM codes may be constructed from these elementary codes using the following recursive definition

{% raw %}

$$
\mathrm {RM} (r,m)=\{(\mathbf {u} ,\mathbf {u} +\mathbf {v} )\mid \mathbf {u} \in \mathrm {RM} (r,m-1),\mathbf {v} \in \mathrm {RM} (r-1,m-1)\}.
$$

{% endraw %}

The dimension of $$RM(r, m)$$ is given by

{% raw %}
$$k=\sum _{s=0}^{r}{m \choose s}$$
{% endraw %} 

and the minimum distance is given by

{% raw %}
$$ d=2^{m-r}.$$
{% endraw %}

### First Order RM Codes

The first order RM code is the code $$RM(1, m)$$, which is a $$(2^m, m+1, 2^{m-1})$$ code. By definition, every codeword $$c\in RM(1, m)$$ is evaluation of multivariate polynomial in $$\mathbb{F}_2[X_1, X_2, \cdots, X_m]$$ 
