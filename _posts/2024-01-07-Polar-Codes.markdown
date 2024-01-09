---
layout: post
title: "Polar Codes"
date: 2024-01-07 03:44:00 +0530
categories: jekyll update
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

### Encoding 

Reliability sequence

{% highlight julia %}

function encode(message, N)

    K = length(message)                    # message length and N-K is frozen bits length
    n = log2(N);                           # number of stages

    QN = Q[Q.<=N]                          # pick positions having value <= N from Reliability Sequence

    u = zeros(Int, N)                      # initialize codeword
    u[QN[N-K+1:end]] = message             # insert message bits at high reliability positions

    # iterative encoding
    bitsToCombine = 1
    for depth = n-1:-1:0
        for i = 1:2*bitsToCombine:N                    # combine bits in pairs
            u[i:i+2*bitsToCombine-1] = reshape([mod.(u[i:i+bitsToCombine-1] + u[i+bitsToCombine:i+2*bitsToCombine-1], 2) u[i+bitsToCombine:i+2*bitsToCombine-1]], 2 * bitsToCombine) # update codeword
        end
        bitsToCombine = bitsToCombine * 2 # double the number of bits to combine in next stage
    end

    return u

end
{% endhighlight %}