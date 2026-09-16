---
layout: post
title: "The Replication Loop: Mutual Evaluation and Supervision without Peers"
published: true
---

*Can a worker be incentivized and evaluated without peer workers, ground-truth references, or future observed outcomes? Replication provides one route.*

Peer prediction and proper scoring rules are mechanisms that incentivize workers to truthfully report their beliefs for completing evaluation tasks. Peer prediction determines rewards from the reports of other workers rather than ground truth [1]. Proper scoring rules score reported beliefs against (future) observed outcomes [2]. Are there incentivizing mechanisms that need neither peer workers nor (future) observed outcomes?

This article provides a positive answer. I formalize (<a href="https://github.com/zrobertson466920/mutual-evaluation/tree/main" target="_blank">see Lean4 repo</a>) mutual evaluation of a replicable task worker and a critic that compares returns, both modeled as strategic agents seeking a common payoff (evaluation score). This model assumes access to a worker that can independently replicate their own work. For example, copies of a large language model (LLM) system can all be independently given the same evaluation tasks and specification. I then introduce a replication loop implementation, based on waiting for critic matches, that returns an unbiased estimate (correct in expectation) of the worker-critic payoff.

To be brief, replication $$Y_1'$$ of $$Y_1$$  on the same task $$X$$ replaces the peer $$Y_2$$ as a task proxy. However, by the Data-Processing Inequality (DPI) this only lower-bounds true task information [3]. By comparing repeated replication attempts on the same task and freshly sampled tasks, the mechanism is able to score how much the worker preserves true task information.

$$
\underbrace{I(Y_1;Y_2)}_{\text{peer proxy}} \ \le_{\text{DPI}} \ \underbrace{I(X;Y_{\bullet})}_{\text{true task information}} .
$$

This means producing a score does not require a second peer worker, a ground-truth reference, or an estimator for likelihood ratios. For Pearson and Shannon information, simple first-replication waiting times give unbiased estimates. In terms of incentives, consider a worker that truthfully reports and a critic that defines replication agreement without losing distinctions with the task information. This jointly maximizes the score and forms a Nash equilibrium. The trade-off is a random, potentially unbounded number of worker replications. Mutual evaluation also makes the incentives of the critic explicit; as it decides which worker returns count as the same.

## The mutual evaluation game

A mutual evaluation game $$G$$ has an evaluation type (task set) $$X$$ and a completion return type (return alphabet) $$R$$. Both are finite here. Write $$\Delta(R)$$ for beliefs over returns. A channel or kernel affects an input to a belief (probability distributions) over outputs. A task $$x\sim P$$, drawn from the task prior (distribution) $$P$$, generates raw returns through two fixed worker channels $$w_i:X\to\Delta(R)$$, $$i\in \lbrace 0,1 \rbrace$$. The critic chooses a finite-valued rule that induces an evaluation score on joint report laws [4]. The rule is $$c:R\times R\to S$$, with $$S\subset\mathbb R$$ finite and nonempty; $$u(c,\rho)$$ is the common payoff.

Each worker chooses a reporting kernel $$\sigma_i:R\to\Delta(R)$$:

$$
y_i\sim w_i(x),\qquad r_i\sim\sigma_i(y_i).
$$

A truthful strategy returns the raw return unchanged. A garbling post-processes the reported return through a further kernel. Conditional on $$x$$, independently sample twice from each reported channel. The strategy profile $$\sigma=(\sigma_0,\sigma_1)$$ thus generates an outcome law (distribution) $$\rho=\text{law}_G(\sigma)$$ over $$(R\times R)\times(R\times R)$$.

This model is intended to allow comparison between peer prediction setting and the replication setting introduced later. Peer prediction would use both worker channels. The conceptual move in this article is to ignore the second worker channel and design the evaluation score as a strategic channel.

## Critic annotations

Here $$X$$ also denotes the random task. For a finite-valued variable $$Z$$, write $$p_Z(z)=\Pr(Z=z)$$ and $$p_{X,Z}(x,z)=\Pr(X=x,Z=z)$$. Define Pearson and Shannon mutual information by

$$
\begin{aligned} I_{\chi^2}(X;Z)&:=\sum_{x,z}\frac{p_{X,Z}(x,z)^2}{P(x)p_Z(z)}-1,\\
 I(X;Z)&:=\sum_{x,z}p_{X,Z}(x,z)\log\frac{p_{X,Z}(x,z)}{P(x)p_Z(z)}. \end{aligned}
$$

Terms with zero denominator are omitted, and zero-mass logarithmic terms contribute zero. Logarithms are natural. The Shannon score is also called the Kullback–Leibler (KL) score.
 
Fix the critic score set to $$S= \lbrace 0,1 \rbrace$$. A critic rule $$c:R\times R\to \lbrace 0,1 \rbrace$$ can be seen as a relation. This relation is *valid* when

$$
y\sim_c y' \quad\Longleftrightarrow\quad c(y,y')=1
$$

defines an equivalence relation on the entire return alphabet. Notice validity is a global property, not defined by a single sample.

**Theorem (finite annotation representation).** Suppose $$R$$ is finite. A critic is valid if and only if there are a finite annotation alphabet $$B$$ and a deterministic map $$g:R\to B$$ such that

$$
c(y,y')=c_g(y,y') :=\mathbf 1 \lbrace g(y)=g(y')\rbrace \qquad\text{for every }y,y'\in R.
$$

A valid critic is way to partition the worker's return alphabet. It decides which returns count as the same type.

## Replication Loop Mechanisms

The procedure introduced here only requires values of $$c(y, y')$$ and a validity determination to run. This allows different annotation maps to induce the same procedure. For both evaluation scores below, an invalid rule receives the constant score zero and the procedure does not run. Intuitively, validity is a *property* not an assumption. If it is false the score is set to zero.

The critic here amounts to a finite annotation $$A=g(Y)$$ of the worker's report $$Y$$, where $$g:R\to B$$ maps returns to a finite label set $$B$$. The two scores are $$I_{\chi^2}(X;A)$$ and $$I(X;A)$$: Pearson and Shannon information retained about the task. Worker garbling changes the reported information; critic garbling coarsens the annotation by merging types.

Fix the single replicated worker's reported channel $$k=k_\sigma$$, where $$\sigma:R\to\Delta(R)$$ now denotes this worker's reporting kernel applied after $$w_0$$. We will define two replication sequences using random variables based on a fixed component. First sample the fixed task $$x \sim P$$. Given $$x$$, sample a return $$Y \sim k(x)$$ and use this as the fixed return, or anchor. The first sequence consists of alternative return replications from the fixed task. These are drawn according to

$$
\widetilde Y_n \sim k(x), \qquad n \ge 1
$$

Conditional on $$x$$, the law of the sequence $$(\widetilde Y_n)_{n \ge 1}$$ is independent and identically distributed (iid). The second sequence consists of null return replications from freshly drawn tasks sampled

$$
x_n\sim P,\quad Z_n\sim k(x_n), \qquad n\ge1.
$$

Conditional on the fixed task $$x$$, the fixed return and replication sequences are independent.

It is assumed the reporting channel and critic remain fixed throughout the experiment.

Given a valid critic define the alternative and null type-replication variables (first-match counts, or hitting times) from the comparisons:

$$
\tau_{\mathrm{alternative}} := \inf \lbrace n\ge1:c(Y,\widetilde Y_n)=1 \rbrace, \qquad \tau_{\mathrm{null}} := \inf \lbrace n\ge1:c(Y,Z_n)=1
\rbrace,
$$

with $$\inf\varnothing=\infty$$. The experiments below are based on inverse-binomial sampling, but do not use a peer worker or ground truth [5].

### The Pearson collision payment

Request one same-task return $$\widetilde Y_1$$. If

$$
c(Y,\widetilde Y_1)=0,
$$

stop and pay $$-1$$. Otherwise continue producing null replications and pay $$\tau_{\mathrm{null}}-1$$. Thus

$$
W_{\chi^2} = \begin{cases} -1, &c(Y,\widetilde Y_1)=0,\\
 \tau_{\mathrm{null}}-1, &c(Y,\widetilde Y_1)=1. \end{cases}
$$

The null search is not run after a mismatch.

### The KL two-clock payment

Run both replications (the two clocks). With harmonic numbers

$$
H_0:=0, \qquad H_m:=\sum_{j=1}^{m}\frac1j,
$$

pay

$$
W_{\mathrm{KL}} := H_{\tau_{\mathrm{null}}-1} - H_{\tau_{\mathrm{alternative}}-1}.
$$

The replication loop mechanisms both terminate and equal their corresponding mutual information functional in expectation. Specifically, the replication procedure evaluates the information *retained* by the equivalence classes of the critic. 

**Theorem (termination, integrability, and unbiasedness).** For finite $$X,R$$, every fixed channel $$k$$, and every valid critic $$c$$, each invoked replication sequence terminates almost surely (with probability one) and both payments are integrable ($$\mathbb {E}[\mid W_{\bullet} \mid] < \infty $$). If $$c=c_g$$ and $$A=g(Y)$$, set $$A'=g(\widetilde Y_1)$$ and $$p_A(a)=\Pr(A=a)$$. Then

$$
\begin{aligned} u_{\chi^2}(c;P,k) &= \sum_{a:p_A(a)>0} \frac{\Pr(A=a,A'=a)}{p_A(a)} -1 = I_{\chi^2}(X;A), \\
 u_{\mathrm{KL}}(c;P,k) &= I(X;A), \end{aligned}
$$

Here $$A$$ and $$A'$$ are annotations of alternative replications, not annotations of null replications. The proof is formalized, and there is a useful intuition. The replication times are geometrically distributed from which you can recover $$p_A$$ and then the KL estimation can be seen as an extension. The conceptual bridge

$$
\tau \sim \text{Geom}(p) \ \Rightarrow \ \mathbb{E}[\tau] = 1/p, \quad \mathbb{E}[H_{\tau-1}] = - \log(p)
$$

is at the heart of why it is possible to convert replication times into unbiased estimates of information [6].

## Critic Regret and Value envelopes

The replication loop mechanism evaluates the information *retained* by the equivalence classes of the critic. Sufficiency means preserving task beliefs when replacing a report by its annotation. The value envelope and regret associated with the mutual evaluation game will help clarify the incentives.

The value envelope (supremal game valuation) optimizes over critic rules. Define critic regret as the difference between the supremal game valuation and critic evaluation score. 

$$
V(\rho):=\sup_{c:R\times R\to S}u(c,\rho), \qquad r(c,\rho):=V(\rho)-u(c,\rho).
$$

The evaluation score is equal to the game valuation minus critic regret. This regret is nonnegative and equal to zero if and only if the critic evaluation score is the supremal game valuation. 

$$
u(c,\rho)=V(\rho)-r(c,\rho).
$$

**Channel-level evaluation scores.** The parameter of the replication loop score is the channel instance $$(P,k)$$. The KL experiment need not be determined by the four-return outcome law used by the core game.

Write $$\bullet$$ for either $$\chi^2$$ or $$\mathrm{KL}$$, and $$\mathbb E$$ for expectation. This separates the critic rule, random payment, and evaluation score as distinct constructs:

$$
c \quad\longmapsto\quad W_\bullet(c;P,k) \quad\longmapsto\quad u_\bullet(c;P,k) = \mathbb E[W_\bullet(c;P,k)].
$$

The envelope and critic regret are then the generic score quantities

$$
V_\bullet(P,k):=\sup_c u_\bullet(c;P,k), \qquad r_\bullet(c;P,k):= V_\bullet(P,k)-u_\bullet(c;P,k).
$$

**Theorem (envelopes, regret, and sufficiency).** Maximizing over the full binary critic space gives

$$
V_{\chi^2}(P,k) = I_{\chi^2}(X;Y), \qquad V_{\mathrm{KL}}(P,k) = I(X;Y).
$$

Both envelopes are attained by the literal-agreement critic

$$
c_{\mathrm{id}}(y,y') := \mathbf1 \lbrace y=y' \rbrace.
$$

For every valid critic $$c=c_g$$, with $$A=g(Y)$$,

$$
\begin{aligned} r_{\mathrm{KL}}(c;P,k) &= I(X;Y)-I(X;A) \\
 &= I(X;Y\mid A). \end{aligned}
$$

Critic regret measures what the annotation discards; in the KL case, $$r_{\mathrm{KL}}=I(X;Y\mid A)$$, the conditional mutual information $$I(X;Y)-I(X;A)$$ still in $$Y$$ given $$A$$. This leaves an optimal critic free to merge reports that leave beliefs unchanged over tasks. This separates type-annotation from information loss. Literal agreement always attains the envelope. However, this need not be the only optimal critic. This is useful since the runtime may depend on the complexity of the type-annotations.

**Examples.** Let $$X$$ and $$U$$ be independent fair bits and let the truthful return be

$$
Y_0=(X,U).
$$

Literal agreement retains the full report. The nuisance-removing annotation

$$
g(x,u)=x
$$

strictly compresses the return alphabet but retains all task information. Both critics therefore attain Pearson information $$1$$ and Shannon information $$\log 2$$, and both have zero regret.

A constant annotation retains no task information, giving regrets $$1$$ and $$\log2$$, respectively. Any bijective annotation merely relabels reports and again has zero regret.

These examples illustrate the following three notions are distinct: the size of the annotation alphabet, the amount of task information retained by the critic, and the sampling cost of implementing its evaluation score.

## Critic garbling and truthful equilibrium

Type-annotation gives a second garbling operation. If $$c=c_g$$ is valid and $$h:B\to C$$ maps annotations to another label set $$C$$, then

$$
c_{h\circ g}(y,y') = \mathbf1 \lbrace h(g(y))=h(g(y')) \rbrace
$$

is another valid critic obtained by garbling the annotation classes. For either score,

$$
u_\bullet(c_{h\circ g};P,k) \le u_\bullet(c_g;P,k).
$$

Here $$w_0$$ is the raw channel of the single replicated worker. For every reporting kernel and every critic $$d$$,

$$
u_\bullet(d;P,k_\sigma) \le V_\bullet(P,k_\sigma) \le V_\bullet(P,w_0).
$$

**Corollary (truthful equilibrium under replication).** Truth and any critic optimal at truth jointly maximize the common evaluation score and form a Nash equilibrium of the corresponding replication game: neither can gain by changing its own strategy alone. Worker deviations choose one reporting kernel before the replication experiment begins.

## Efficiency and unresolved design choices

The mechanisms require a fixed memoryless (history-independent) worker-critic strategy profile and use a random, potentially unbounded number of samples.

### A timing caveat

After the anchor, advancing each unfinished clock once per parallel round gives KL completion time $$T_{\mathrm{KL}}$$, with expectation

$$
\mathbb E[T_{\mathrm{KL}}] = \mathbb E[\max \lbrace \tau_{\mathrm{null}},\tau_{\mathrm{alternative}} \rbrace],
$$

not the expected total sample count. Pearson has an entry clause: an immediate same-task match. Excluding this one-sample test, its null-search wait on the same draws is

$$
T_{\chi^2}:=\mathbf1 \lbrace \tau_{\mathrm{alternative}}=1 \rbrace\tau_{\mathrm{null}} \le \tau_{\mathrm{null}}\le T_{\mathrm{KL}}.
$$

KL wait therefore dominates Pearson's null-search wait almost surely and in expectation. But KL pays a harmonic difference, not raw counts: with the same-task collision gate and the $$-1$$ offset, raw null waiting time instead gives the Pearson payment, $$W_{\chi^2}=T_{\chi^2}-1$$. A critic optimizing that proxy while reporting KL would be misaligned: it would optimize a different objective.

The conflict here illustrates the importance of analyzing the critic as a strategic agent. Garbling influenced by memory, expected waiting time, and so on can fundamentally *deviate* from the intended construct. A critic that optimizes this raw waiting-time proxy while the stated objective is KL is therefore implicitly optimizing the Pearson score instead. While the setting is stylized, such considerations appear rich for further analysis and follow-up work.

**Conclusion.** The mutual evaluation model provides a common formalization for the peer-prediction and replication settings. Peer prediction uses both worker channels. The conceptual move here is to replace the second worker by independent replications of the first, while treating the critic's notion of agreement as a strategic choice.

## Citation

```bibtex
@misc{robertson2026mutualeval,
  title={The Replication Loop: Mutual Evaluation and Supervision without Peers},
  author={Robertson, Zachary},
  year={2026},
  month={September},
  institution={Stanford University},
  url={https://zrobertson466920.github.io/MutualEvaluation}
}
```

## Reproducibility Statement

All mathematical declarations and statements are formalized in Lean4 available at <a href="https://github.com/zrobertson466920/mutual-evaluation/tree/main" target="_blank">this repository</a>. Every visible theorem claim derives from a set of manually reviewed declarations including annotations. The prose in this blog was human authored, but derived from these annotations. AI was used in significant capacity to formalize the *proofs* (not statements) and collate annotations for article preparation. 

## References

[1] Miller, Nolan, Paul Resnick, and Richard Zeckhauser. “Eliciting Informative Feedback: The Peer-Prediction Method.” *Management Science* 51.9 (2005): 1359–1373.

[2] Brier, Glenn W. “Verification of Forecasts Expressed in Terms of Probability.” *Monthly Weather Review* 78.1 (1950): 1–3.

[3] Schoenebeck, Grant, and Fang-Yi Yu. “Learning and Strongly Truthful Multi-Task Peer Prediction: A Variational Approach.” *12th Innovations in Theoretical Computer Science Conference (ITCS 2021)*, LIPIcs, vol. 185, 2021, pp. 78:1–78:20. doi:10.4230/LIPIcs.ITCS.2021.78.

[4] Robertson, Zachary, and Sanmi Koyejo. "Let's Measure Information Step-by-Step: LLM-Based Evaluation Beyond Vibes." *arXiv e-prints* (2025): arXiv-2508.

[5] Aznag, Abdellah, et al. “Sample Complexity of Peer Prediction.” *arXiv*, 2026, arXiv:2608.16838. 

[6] van Opheusden, Bas, Luigi Acerbi, and Wei Ji Ma. “Unbiased and Efficient Log-Likelihood Estimation with Inverse Binomial Sampling.” *PLOS Computational Biology*, vol. 16, no. 12, 2020, e1008483. doi:10.1371/journal.pcbi.1008483.
