---
layout: post
title: Planning Problem
published: true
---

\[section\] \[theorem\]<span>Lemma</span>
\[theorem\]<span>Proposition</span> \[theorem\]<span>Corollary</span>
\[theorem\]<span>Fact</span>

\[theorem\]<span>Definition</span> \[theorem\]<span>Construction</span>
\[theorem\]<span>Convention</span> \[theorem\]<span>Example</span>
\[theorem\]<span>Exercise</span> \[theorem\]<span>Problem</span>
\[theorem\]<span>Counterexample</span>
\[theorem\]<span>Conjecture</span> \[theorem\]<span>Notation</span>
\[theorem\]<span>Warning</span> \[theorem\]<span>Remark</span>

# Introduction

Suppose you have to complete some task \(T\). Suppose also that you have
some sort of initial policy \(\pi_0\) that delineates all the steps you
need to take to complete \(T\). Moreover, you also have an oracle
\(\Phi\) that will tell you how long it will take to execute \(\pi\).
Naturally, we’ll place a restriction on \(\Phi(\pi)\) so that it’s
bounded and always equal to or greater than zero. Now, say you can
invest \(t\)-time and plan \(\pi_0 \rightarrow \pi_t\) in the sense that
it’s always the case that \(\Phi(\pi_0) \ge \Phi(\pi_t)\). Thus, with a
slight abuse of notation we can write the result of planning \(\pi\) for
\(t\)-time as \(\Phi(t)\). Naturally, our goal is to minimize the total
amount of time \(\Gamma(t) = t+\Phi(t)\) it takes to execute the
planning and the resulting policy.

Suppose everything was nice and that \(\Phi \in C^1\) and is convex. We
know that it must be the case that \(\Phi(t) \ge 0\). Thus, it should
follow that \(\lim_{t \to \infty} \Phi(t)\) exists and that
\(\lim_{t \to \infty} \Phi^{'}(t) = 0\). The point is that condition for
the minima is easy to state and is equivalent to when
\(\Phi^{'}(t) = -1\). Thus, as long as the rate of compression is faster
than the rate at which time progresses for some \(t'\) there will exist
an optimal planning time.
