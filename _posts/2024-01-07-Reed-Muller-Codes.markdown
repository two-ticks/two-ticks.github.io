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

A codeword in $$\mathrm{RM}(1, m)$$ is the truth table of an affine function $$\ell(x) = a_0 + a_1 x_1 + \cdots + a_m x_m$$ on $$\mathbb{F}_2^m$$. Decoding is asking which affine function is closest to my received word.

The naive search through all $$2^{m+1}$$ codewords costs $$O(2^{2m})$$. We can do much better. The inner products of the received vector against every linear codeword can all be read off a single Walsh-Hadamard transform.

Map the received word $$r\in\{0,1\}^n$$ to $$\pm 1$$ via $$y(x) = (-1)^{r(x)}$$. For each $$a \in \mathbb{F}_2^m$$ define

{% raw %}
$$\hat{y}(a) \;=\; \sum_{x \in \mathbb{F}_2^m} y(x)\, (-1)^{a\cdot x}.$$
{% endraw %}

This is the correlation between $$y$$ and the linear codeword indexed by $$a$$. The whole transform $$\hat y$$ over all $$2^m$$ values of $$a$$ can be computed in $$O(m\,2^m)$$ time using the butterfly structure. That is the *Fast Hadamard Transform* (FHT). The decoder picks the $$a$$ that maximises $$\lvert\hat{y}(a)\rvert$$. The *sign* of that maximum gives the constant bit $$a_0$$.

Where does the speed-up come from? It is hiding inside the same Kronecker structure that defines the RM code. The Hadamard matrix is built recursively from the $$2 \times 2$$ matrix

{% raw %}
$$H_1 \;=\; \begin{pmatrix} 1 & 1 \\ 1 & -1 \end{pmatrix},$$
{% endraw %}

iterated as

{% raw %}
$$H_m \;=\; H_1 \otimes H_{m-1} \;=\; \begin{pmatrix} H_{m-1} & H_{m-1} \\ H_{m-1} & -H_{m-1} \end{pmatrix}.$$
{% endraw %}

So $$H_m$$ is a $$2^m \times 2^m$$ matrix. The transform we care about is just $$\hat{y} = H_m\, y$$. Computed naively that costs $$2^{2m}$$ multiplications. The Kronecker form lets us *factor* $$H_m$$ into $$m$$ extremely sparse matrices, one per dimension:

{% raw %}
$$H_m \;=\; M_1\, M_2 \cdots M_m, \qquad M_k \;=\; I_{2^{\,k-1}} \,\otimes\, H_1 \,\otimes\, I_{2^{\,m-k}}.$$
{% endraw %}

Each $$M_k$$ is block-diagonal. It has exactly two non-zero entries per row, both $$\pm 1$$. So multiplying by $$M_k$$ is $$2^m$$ butterfly operations (one add and one subtract per pair). Stack $$m$$ such layers and the whole transform costs

{% raw %}
$$m \cdot 2^m \quad \text{additions/subtractions, no multiplications at all.}$$
{% endraw %}

For $$m=2$$ the factorisation reads

{% raw %}
$$H_2
\;=\;
\underbrace{\begin{pmatrix} 1 & 1 & 0 & 0 \\ 1 & -1 & 0 & 0 \\ 0 & 0 & 1 & 1 \\ 0 & 0 & 1 & -1 \end{pmatrix}}_{M_1 \,=\, I_1 \otimes H_1 \otimes I_2}
\cdot
\underbrace{\begin{pmatrix} 1 & 0 & 1 & 0 \\ 0 & 1 & 0 & 1 \\ 1 & 0 & -1 & 0 \\ 0 & 1 & 0 & -1 \end{pmatrix}}_{M_2 \,=\, I_2 \otimes H_1 \otimes I_1}.$$
{% endraw %}

You can read the butterfly diagram off this directly. The first stage pairs adjacent samples. The second stage pairs samples at stride $$2$$. Each level doubles the stride. This is the Cooley-Tukey decomposition. The twiddle factors are all $$\pm 1$$ because we are over $$\mathbb{F}_2$$. The recursion on matrices and the recursion on the code $$\mathrm{RM}(r, m) = \{(\mathbf{u}, \mathbf{u} + \mathbf{v})\}$$ are the *same* recursion. The in-place butterfly walks the $$(\mathbf{u}, \mathbf{u}+\mathbf{v})$$ tree.

The whole story (hard or soft decision) collapses into one transform plus a single argmax. For length $$2^{10} = 1024$$ that is roughly $$10{,}000$$ butterflies instead of $$10^6$$ correlations.


### Weight Enumerators

How much do we know about a code if we know only the *weights* of its codewords? Surprisingly a lot. It is all packaged in a single bivariate polynomial.

For a binary code $$C \subseteq \mathbb{F}_2^n$$ the *weight enumerator* is

{% raw %}
$$W_C(x, y) \;=\; \sum_{c \in C} x^{\,n - w(c)} y^{\,w(c)} \;=\; \sum_{i=0}^{n} A_i\, x^{n-i} y^{i},$$
{% endraw %}

where $$A_i$$ counts the codewords of Hamming weight $$i$$. The tuple $$(A_0, A_1, \dots, A_n)$$ is the *weight distribution*.

For first order RM codes the picture is unusually clean. Every nonzero affine function on $$\mathbb{F}_2^m$$ is balanced. Half its inputs hit $$0$$ and half hit $$1$$. The one exception is the constant function $$1$$. So the only nonzero weights are $$2^{m-1}$$ and $$2^m$$, and

{% raw %}
$$W_{\mathrm{RM}(1,m)}(x, y) \;=\; x^{\,2^m} \;+\; (2^{m+1} - 2)\, x^{\,2^{m-1}} y^{\,2^{m-1}} \;+\; y^{\,2^m}.$$
{% endraw %}

For higher order RM codes the weight distribution is much less obliging. Closed forms are known only for a handful of cases. Kasami, Sloane and others pieced these together using deep number-theoretic identities.


### MacWilliams Identity

The dual of $$\mathrm{RM}(r, m)$$ is $$\mathrm{RM}(m-r-1, m)$$. If we know the weight enumerator of one, we should be able to read off the other for free. That is exactly what MacWilliams' identity gives us:

{% raw %}
$$W_{C^{\perp}}(x, y) \;=\; \frac{1}{\lvert C \rvert}\, W_{C}(x+y,\; x-y).$$
{% endraw %}

The proof is a one-line miracle if you trust your characters. Apply $$(x,y) \mapsto (x+y, x-y)$$ at every coordinate, sum over $$C$$, and use the orthogonality

{% raw %}
$$\sum_{c \in C} (-1)^{c \cdot u} \;=\; \lvert C \rvert \cdot \mathbb{1}[u \in C^{\perp}].$$
{% endraw %}

The factor $$1/\lvert C \rvert$$ is the price of inversion.

A small sanity check. The dual of $$\mathrm{RM}(1, m)$$ is $$\mathrm{RM}(m-2, m)$$, the extended Hamming code. Plug the first-order enumerator above into MacWilliams. Out drops the (much richer) weight distribution of the extended Hamming code, without ever enumerating its $$2^{\,2^m - m - 1}$$ codewords.


#### Krawtchouk Form of MacWilliams

The polynomial identity $$W_{C^\perp}(x,y) = \tfrac{1}{\lvert C \rvert} W_C(x+y, x-y)$$ is elegant. On the way to *computing* a dual weight distribution we usually want it coefficient by coefficient. Expand $$(x+y)^{n-i}(x-y)^i$$ and collect the coefficient of $$x^{n-k}y^{k}$$. What falls out is a *Krawtchouk polynomial*:

{% raw %}
$$K_{k}(i; n) \;=\; \sum_{j=0}^{k} (-1)^{j} \binom{i}{j} \binom{n-i}{k-j}.$$
{% endraw %}

It is a polynomial of degree $$k$$ in $$i$$. Think of it as a *discrete orthogonal polynomial* on $$\{0, 1, \dots, n\}$$ with the binomial weight $$\binom{n}{i}$$. The MacWilliams identity then reads, coordinate-wise on the weight distribution,

{% raw %}
$$A^{\perp}_{k} \;=\; \frac{1}{\lvert C \rvert} \sum_{i=0}^{n} A_{i}\, K_{k}(i; n).$$
{% endraw %}

So if you stack the dual weights into a vector and the primal weights into another, MacWilliams is just *one matrix multiplication* by the Krawtchouk matrix $$\bigl[K_k(i;n)\bigr]_{k,i}$$.

A few facts that make Krawtchouks pleasant to work with:

- $$K_k(0; n) = \binom{n}{k}$$. Useful for normalisation checks.
- A *three-term recurrence* in $$k$$. The whole table builds in $$O(n^2)$$ without ever touching a binomial.
- The generating function

  {% raw %}
  $$\sum_{k=0}^{n} K_{k}(i; n)\, z^{k} \;=\; (1+z)^{\,n-i}\,(1-z)^{\,i},$$
  {% endraw %}

  is exactly the per-coordinate version of the substitution $$(x,y) \mapsto (x+y, x-y)$$. Krawtchouks are that substitution, written one degree at a time.
- Orthogonality: $$\sum_{i=0}^{n} \binom{n}{i} K_{k}(i;n)\, K_{\ell}(i;n) = 2^{n}\binom{n}{k}\delta_{k\ell}.$$ This is what lets you *invert* MacWilliams cleanly. Run it twice and you return to where you started.

A concrete payoff. Apply the formula to $$\mathrm{RM}(1,m)$$, where weights live only at $$0$$, $$2^{m-1}$$ and $$2^m$$. Each $$A^{\perp}_k$$ is a sum of three Krawtchouk values. Out drops the weight distribution of the extended Hamming code in closed form. No codeword enumeration, no Plotkin tricks. Just three table lookups per dual weight.


### Second Order Decoding: Recursive Projection-Aggregation

Stepping from $$r=1$$ to $$r=2$$ the FHT trick falls apart. Codewords are no longer evaluations of *linear* functions, and the codeword count explodes. The *Recursive Projection-Aggregation* (RPA) decoder of Ye and Abbe sidesteps this. It reduces $$\mathrm{RM}(r, m)$$ to many independent decodings of $$\mathrm{RM}(r-1, m-1)$$.

The key move is *projection*. For each one-dimensional subspace $$\mathbb{B} = \{0, b\} \subset \mathbb{F}_2^m$$ we form a projected received vector $$y/\mathbb{B}$$. We combine each pair of values $$\bigl(y(x), y(x+b)\bigr)$$ by adding the LLRs in the soft setting, or XORing in the hard one. The structure of $$\mathrm{RM}(r,m)$$ guarantees that this projection is a noisy codeword of $$\mathrm{RM}(r-1, m-1)$$. So we recurse.

There are $$2^m - 1$$ such subspaces. Each one returns its own estimate of every bit. To stitch them together RPA *aggregates*. Typically this is a weighted average of LLRs across all projections. The result is used to refine the original word. Then it iterates. A few rounds usually suffice.

Two things make this work:
- The projection respects the recursive $$(\mathbf{u}, \mathbf{u} + \mathbf{v})$$ definition. The subproblem at every level is itself an RM decoding problem.
- The aggregation is an error-correcting *average*. Even when individual projections are noisy, their consensus is sharp.

The base case of the recursion is $$r = 1$$. There we plug in the FHT decoder above. So the whole second-order decoder is, at heart, *many Hadamard transforms glued together by majority votes*. For $$\mathrm{RM}(2, m)$$ at moderate $$m$$, RPA gets within a small fraction of a dB of maximum likelihood. Its complexity is roughly $$O(n^{\,r} \log n)$$.


#### RXA — Reduced-Complexity Variants

Vanilla RPA pays for every projection at every level. This becomes wasteful as $$r$$ grows. *Recursive subset-Aggregation* style variants, often abbreviated **RXA** in the follow-up literature, prune either the projection set or the iteration count. A few flavours show up in practice:

- Project only along subspaces aligned with the recursive code structure. This saves a $$\log_2$$ factor per level.
- Stop iterating early once the projections agree to within a confidence threshold.
- Replace majority aggregation with a soft min-sum. You take a small accuracy hit in exchange for hardware-friendly arithmetic.

The trade-off is what you would expect. A few tenths of a dB of performance for a constant or polynomial factor of speed. RXA is essentially what makes RPA usable on second-order codes of length $$1024$$ or $$2048$$ on a CPU.


### Ashikhmin's Fast Decoder for High-Rate RM Codes

For *low-rate* first-order codes the FHT is the right tool. For *high-rate* codes the picture flips. Take $$\mathrm{RM}(m-1, m)$$ (the single parity-check code) and $$\mathrm{RM}(m-2, m)$$ (the extended Hamming code). Ashikhmin and Litsyn turn the problem on its head.

Their trick is to *exploit the dual*. The dual of $$\mathrm{RM}(m-2, m)$$ is $$\mathrm{RM}(1, m)$$, which has only $$2^{m+1}$$ codewords. They show that maximum-a-posteriori decoding of the high-rate code can be expressed as a correlation against the *dual* codewords, modulated by the channel evidence. That correlation is, once again, a Walsh-Hadamard transform.

So the soft-decision MAP decoder for $$\mathrm{RM}(m-2, m)$$ runs in $$O(m\, 2^m)$$. That is the same complexity as decoding its tiny dual. For $$\mathrm{RM}(m-1, m)$$ the situation is even simpler. The dual is the repetition code, and one transform collapses to a single sign decision.

The general moral is this. When a code or its dual is small, decoding can hide inside an FHT. RPA generalises that intuition recursively for the codes in between. Ashikhmin's decoder closes the loop at the high-rate end.


## References

1. [Emmanuel Abbe, Amir Shpilka, Min Ye, "Reed-Muller Codes: Theory and Algorithms" (2020)](https://arxiv.org/abs/2002.03317)
2. [Min Ye, Emmanuel Abbe, "Recursive Projection-Aggregation Decoding of Reed-Muller Codes" (2020)](https://arxiv.org/abs/1909.05646)
3. [Alexei Ashikhmin, Simon Litsyn, "Simple MAP Decoding of First and Second Order Reed-Muller Codes" (2004)](https://ieeexplore.ieee.org/document/1337115)
4. F. J. MacWilliams, N. J. A. Sloane, *The Theory of Error-Correcting Codes*, North-Holland (1977).