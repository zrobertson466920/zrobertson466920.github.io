I saw an [interesting website](https://www.thekingoftheinternet.com/?fbclid=IwAR0lXnCx6GEcSu6ICf4FXp3qoXLwV0z7nNmC_QMftbVhea3LTwg9XMKCOMY) the other day where the winner always had to pay one dollar more than the previous visitor. This got me thinking about what would happen if the sequence of visitors to the website had identically distributed buy-out tendencies $$\lbrace w_i \rbrace$$. We'll make a technical assumption throughout that the distribution of wealth was [sub-Gaussian](https://en.wikipedia.org/wiki/Sub-Gaussian_distribution).

We'll assume the initial asking price $$a_0$$ is free and then we have the following recurrence relation,

$$
a_{n+1} = \max (a_n , a_n \cdot \mathbb{I}(w_n > a_n)+1), \quad a_0 = 0
$$

The asymptotic behavior of $$\lbrace a_n \rbrace$$ determines the profitibility of the website. 

The ultimate thing to try and calculate would be something like an upper-bound on the long-term profit and compare it with some educated guesses such as the linear rate attainable with a constant pricing model or the naive quadratic expectation one might expect if you think of adding the ask prices together. It turns out that we do worse than both approaches we get that the profit $$o(\nu^2 \log(n))$$.

## First Thoughts

As an aside, it's interesting that if the increments were not thresholded we might be able to work out the asymptotic form of the ask price without an independence assumption.

**Lemma 1:** Suppose $$(X_i)$$ are zero mean subgaussian with scale parameter $$\nu$$. Then,

$$
\mathbb{E} \left[\max_{i \in [n]} X_i \right] \le \nu \sqrt{2 \log n}
$$

Naively, we could use our lemma and claim that $$\mathbb{E}[a_n] \le \nu \sqrt{2 \log n}$$. However, the increments in this model can be arbitrary so we'll want to be more careful.

## Asymptotic Profit

If we're more serious we can solve the problem with a direct calculation while maintaining rigour.

**Proposition 2:** Suppose the $$w_i$$ are i.i.d and sub-Gaussian then we have,

$$
\mathbb{E}[a_n] \sim o \left(\nu \sqrt{\log(n)} \right)
$$

**Remark:** I'm thinking the proof can be upgraded to big-$$O$$ using a Chernoff argument. I wonder if the independence assumption can be dropped as in the lemma.

The final step is find the long-term profit which would be given by, 

$$
\sum_{k = 0}^n \mathbb{E}[a_k] \sim o(\nu^2 \log(n))
$$

This indicates that the model only appears to be better than the naive strategy 

**Proof 1:** We need a bound on $$\max_i X_i$$ so we work with,

$$
\exp(s \mathbb{E}[\max_i X_i]) \le \mathbb{E}(\exp(s \max_i X_i)) = \mathbb{E}[\max_i e^{s X_i}]
$$

$$
\le \mathbb{E}\left[\sum_i e^{s X_i} \right] = \sum_i \mathbb{E}[e^{s X_i}] \le n e^{s^2 \nu^2 / 2}
$$

$$
\mathbb{E}[\max_i X_i] \le \frac{\log(n)}{s} + \frac{1}{2} s^2 \nu^2
$$

The result follows after optimizing the free parameter $$s$$. $$\square$$

**Proof 2:** We need to calculate the waiting time for the $$k$$-th increment,

$$
\mathbb{E} \left[ \Delta t_k |  w_{t_k} > k \right]
$$

$$
= \sum_{j = 1}^{\infty} j \cdot P(w > k)\cdot P(w \le k)^{j-1} = P(w > k) \cdot\partial_P \sum_{j = 0}^{\infty} P(w \le k)^{j}
$$

$$
= (1-P) \partial_P \left[ \cfrac{1}{1-P} \right] = \cfrac{1}{1-P(w \le k)}
$$

$$
\ge Ce^{k^2 / 2\nu^2}
$$

The last inequality is follows from the sub-Gaussian assumption. This implies an asymptotic form,
$$
\tau(k) = \sum_k \mathbb{E} \left[ \Delta t_k |  w_{t_k} > k \right] \ge \sum_k C e^{k^2 /2 \nu^2} \sim \omega \left( \frac{1}{\nu^2} e^{k^2 / \nu^2} \right) 
$$

$$
\Rightarrow \mathbb{E}[a_n] = \tau^{-1}(n) \sim o\left(\nu \sqrt{\log \left( n\right)} \right)
$$

$$\square$$

