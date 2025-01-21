---
layout: post
title: "Reed Muller Codes"
date: 2024-01-07 03:44:00 +0530
categories: jekyll update
---
{% include analytics.html %}
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

### Introduction 


$$\mathrm {RM}(r, m)$$ is a binary linear block code $$(n, k, d)$$ of length $$2^m$$. $$\mathrm {RM}(m, m)$$ is defined as the universe $$(2^m, 2^m, 1)$$ code and $$\mathrm {RM}(0, m)$$ is defined as repetition code of length $$2^m$$. The remaining RM codes may be constructed from these elementary codes using the following recursive definition

{% raw %}

$$
\mathrm {RM} (r,m)=\{(\mathbf {u} ,\mathbf {u} +\mathbf {v} )\mid \mathbf {u} \in \mathrm {RM} (r,m-1),\mathbf {v} \in \mathrm {RM} (r-1,m-1)\}.
$$

{% endraw %}

The dimension of $$\mathrm {RM}(r, m)$$ is given by

{% raw %}
$$k=\sum _{s=0}^{r}{m \choose s}$$
{% endraw %} 

and the minimum distance is given by

{% raw %}
$$ d=2^{m-r}.$$
{% endraw %}

The generator matrix of $$r^{\text{th}}$$ order RM code of length $$N=2^m$$ can be obtained by choosing rows with Hamming weight at least $$2^{m-r}$$ from $$P$$.

{% raw %}
$$            P = \begin{bmatrix} 1 & 0 \\ 1 & 1 \end{bmatrix}^{\otimes m}$$
{% endraw %}


<!-- ![Reed-Muller Code Construction](/assets/img/2024-01-07-Reed-Muller-Codes/kron.svg){: width="250"} -->

The matrix $$P$$ can be seen as below for $$m=4$$

<div style="text-align: center;">
    <img src="/assets/img/2024-01-07-Reed-Muller-Codes/kron.svg" width="250">
</div>




### First Order RM Codes

The first order RM code is the code $$RM(1, m)$$, which is a $$(2^m, m+1, 2^{m-1})$$ code. By definition, every codeword $$c\in RM(1, m)$$ is evaluation of multivariate polynomial in $$\mathbb{F}_2[X_1, X_2, \cdots, X_m]$$ of at most degree 1.

#### Fast Decoding of First Order RM Codes



## References

1. [Emmanuel Abbe, Amir Shpilka, Min Ye, "Reed-Muller Codes: Theory and Algorithms"](https://arxiv.org/abs/2002.03317)