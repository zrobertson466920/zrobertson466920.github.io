---
layout: post
title: Planning Problem
published: true
---

I’ve been thinking about the law of recursive planning for a while. Basically, it’s the idea that the time spent on solving a problem is equal to time spent planning plus time spent executing. Thus, when we want to decide how much time to spend planning we need to use this metric as the objective. In practice, this is complicated by the fact that the optimization problem is necessarily online. The boundary case is when the amount planning shortens execution time in a one-to-one fashion. In that case, all the time should be spent planning.

One particularly deep aspect of the problem is that in order to carry out a task to completion we must have a plan that we execute. The plan could be written in an ‘online’ manner, such as improvisation. The line between planning and doing becomes very blurry and leads to a chicken/egg problem. Which came first, knowledge of a plan or how to execute? I’ll explore this using a few concrete examples to get us going and then we’ll jump off into abstraction to discuss some of the deeper aspects of this problem.

# A First Example

I need a first example. For me, I was writing the article you’re reading so it seems natural to start there. I don’t want to spend too much time planning out the essay. On the other hand, I don’t want to plan so little that the organization and flow is poor. Usually I spend a bit too much time in the planning phase so this article is an experiment in trying to do what is optimal.

The general idea is that you are carrying out some task, like writing an article for your website. You can spend some amount of time thinking and planning out a strategy to achieve you’re goal and then you can spend some amount of time carrying out your plan and achieving the goal. Imagine that the task would usually take several hours. Most people would agree that having a clear list of sub-goals or directions at hand would cut the required time down to just a couple hours. 

The intuition seems to be that there is a strategy that is able to balance between the objectives of planning and executing in an optimal way. However, over the years I’ve caught myself making the error of measuring the effectiveness of having a plan or strategy in terms of how much time it saves me in carrying out the task and will completely ignore the time spent on creating the strategy in the first place. For example, when I was writing this article I spent a significant amount of time crafting a uniqueness proof for the optimal strategy I’ll propose in next section. Was that really optimal? I had a picture of the proof done in under an hour. If you’d asked me right after I’d proved the theorem I’d tell you that it was a good use of time, but it wasn’t necessary for the article. So it’s possible that deviations from this supposed optimal strategy might be part some other strategy for the optimization of a different quantity. However, the case remains that I can readily admit that making the proof was not an optimal use of my time if I claim the purpose was for the completion of the article. 

People are notoriously bad at estimating how much time things will take. Moreover, we’re also poor at estimating the amount of time we spend on tasks with variable time requirements. Thus, when we decide to create a plan we often neglect to estimate the cost of taking such an action and we often fail to measure whether or not the plan was successful. It’s a sort of recursive issue since usually we are trying to use the plan to estimate the cost of taking another action. In some sense what I’m trying to do is contain the infinite-regress to a second-order meta-plan. 

# A formal statement

Given that there are some vague notions surrounding the ideas of planning, executing, and optimal lets put things on a formal footing. Assume that there is an oracle that could tell you how long a task would take without a dedicated plan. Say that for writing this article it returned seven hours. Now, I previously argued that planning should be able to cut this down to just two hours. This is the step where I take a bet with respect to an outside oracle. I am placing a bet that the time it takes to make a plan plus the time it takes to execute that plan is less than the time returned by oracle. If not, I lose the bet. For example, I might spend 6 hours creating a detailed plan and then spend two hours executing for a total of eight hours. In this case I lose. Even though I was able to significantly reduce execution time, I spent way too much time planning to make the strategy effective and ultimately spent more time on the task than if I had just started executing from the start.

Suppose you have to complete some task \\(T\\). Suppose also that you have
some sort of initial policy \\(\pi_0\\) that delineates all the steps you
need to take to complete \\(T\\). Moreover, you also have an oracle
\\(\Phi\\) that will tell you how long it will take to execute \\(\pi\\).
Naturally, we’ll place a restriction on \\(\Phi(\pi)\\) so that it’s
bounded and always equal to or greater than zero. Now, say you can
invest \\(t\\)-time and plan \\(\pi_0 \rightarrow \pi_t\\) in the sense that
it’s always the case that \\(\Phi(\pi_0) \ge \Phi(\pi_t)\\). Thus, with a
slight abuse of notation we can write the result of planning \\(\pi\\) for
\\(t\\)-time as \\(\Phi(t)\\). Naturally, our goal is to minimize the total
amount of time \\(\Gamma(t) = t+\Phi(t)\\) it takes to execute the
planning and the resulting policy.

Suppose everything was nice and that \\(\Phi \in C^1\\) and is convex. We
know that it must be the case that \\(\Phi(t) \ge 0\\). Thus, it should
follow that \\(\lim_{t \to \infty} \Phi(t)\\) exists and that
\\(\lim_{t \to \infty} \Phi^{'}(t) = 0\\). The point is that condition for
the minima is easy to state and is equivalent to when
\\(\Phi^{'}(t) = -1\\). Thus, as long as the rate of compression is faster
than the rate at which time progresses for some \\(t'\\) there will exist
an optimal planning time.

# Examples

## a)

Let’s consider a semi-realistic example of a planning function. The most
obvious requirement to try and model is that we should see diminishing
returns on planning as \\(\Phi(t) \to 0\\). Thus, a crude model would be
to model our planning function so that the reduction in execution time
is proportional to the size of the current execution time.

\\[\cfrac{\partial \Phi}{\partial t} = \lambda \cdot \Phi\\]
\\[\Rightarrow \Phi(t) = T_0 \cdot e^{-\lambda t}\\]

We’re using \\(T_0\\) to model the amount of time a task would take with
zero planning and \\(\lambda\\) to model the rate of reduction. We can
solve for the optimal \\(t'\\) using our discussion above since \\(\Phi\\)
is convex and bounded. We end up with,

\\[t' = \cfrac{1}{\lambda} \log(\lambda T_0) \ , \quad \Gamma = \cfrac{1}{\lambda}\left(1+ \log(\lambda T_0)\right)\\]

Thus, the planning to executing ratio ranges from zero to one and
depends solely on \\(\lambda T_0\\). For \\(\lambda T_0 < 1\\) we’ll have
\\(t' < 0\\) which means that planning is an ineffective option.

## b)

This observation can be turned into a practical algorithm. From the
first example we showed that the task reduction quantity \\(\lambda T_0\\)
governs the effectiveness of planning. The requirement that
\\(\lambda T_0 > 1\\) is the same as demanding that the rate of reduction
from planning be greater than one unit of time per unit of time.

For many common tasks it’s immediately obvious whether or not a plan
should be constructed. Make the assumption that this decision rule is
based off of \\(\lambda T_0\\). It follows that we can estimate the
reduction in total time as a function of time by looking at the current
plan for execution at each time step. Once this estimate of the total
time reaches a minimum we should stop planning and start executing.

The pathological aspect of this problem seems to be that there might be
a critical threshold that needs to be reached before planning is
effective. For instance perhaps we estimate \\(\lambda T_0\\), but the
true model for \\(\Phi\\) has a varying rate equation. Thus, in the worst
case we’d only double the amount of time taken to complete the task. In
other words, we spend no more than ½ of our time planning execution. The
reasoning is that in order for planning to be an effective strategy the
minimum must be reached sometime before the estimated time for
execution. If this assumption is violated our original hypothesis is as
well which means that are no guarantees about when or if a minimum might
be reached.

It would seem as though we could layer more complexity onto these simple
models to make them more effective. For instance, we can formalize what
it means to determine whether or not our guess is correct by using
statistical estimators. It also seems as though the convexity
requirement on \\(\Phi\\) is a bit strong for our purposes since planning
is a somewhat complicated and somewhat arbitrary/random affair.

# General Strategy

As noted in the previous section, we’ve restricted \\(\Phi\\) so severely
this tells us nothing about whether or not it’s a good idea in general
to try and optimize \\(\pi\\). This is because we have no guarantee that
our optima at \\(t'\\) is better that \\(t+\Phi(t)\\) for \\(t \in [0,t']\\).
Now, lucky for us most of this has already been studied in the context
of optimal persistent policy theory. These deal with optimal selection
problems where the \\(n\\)-th observation costs \\(c(n)\\). Thus, we’ll
adopt this strategy and assume we can construct a sequence of plans
\\({\pi_t}_{t \ge 0}\\) and incur unit cost for each plan we create.

Let \\(V(\pi)\\) denote the expected time of planning plus execution for
continuing to compress \\(\pi\\) after the observation of \\(\pi\\). It
follows that \\(V\\) defines a stopping criteria for if
\\(V(\pi) < \Phi(\pi)\\) we should expect to do worse unless we stop
planning and start executing. In practice, for each \\(t\\) we
observe/estimate \\(\Phi(t)\\) and pay a cost \\(t\\) to do so. To
accommodate error in estimation we introduce a transition function to
map \\(\pi_t \rightarrow \pi_{t+1}\\). It’s given as the probability of
going to policy \\(y\\) starting from \\(x\\) and will be written as
\\(P_x(y)\\). All of our policies live in the decision space \\(D\\). The
decision to continue planning incurs unit cost. If our \\(V\\) was optimal
for each \\(\pi\\) we’d have,

\\[(1) \quad V(\pi) = \min \left( \Phi(\pi), \int_{D} V(y) \ dP_x(y) + 1 \right)\\]

**Theorem 3.1.** Suppose that \\(\Phi(x)\\) is bounded on \\(D\\). Then equation \\((1)\\) has
at least one solution \\(V\\) that satisfies
\\(\inf_{d \in D} V(d) \ge \inf_{d \in D} \Phi(d)\\).

*Proof.* The trick is to define a sequence of partial solutions using recursion.
Define truncations of \\(V\\) as,

\\[V^0(\pi) = \Phi(\pi)\\]
\\[V^1(\pi) = \min \left(\Phi(\pi), \int_\pi V^0(\pi) \ dP_x(y) + 1 \right)\\]
\\[V^{n}(\pi) = \min \left(\Phi(\pi), \int_D V^{n-1}(\pi) \ dP_x(y) + 1 \right)\\]

\\[\Rightarrow V^n(\pi) \ge V^{n+1}(\pi)\\]

This is saying that the expected execution will go down as we continue
to refine our \\(V\\). It also follows that there is a limit to how far we
can push this refinement process as we must have,

\\[\Rightarrow V^n(\pi) \ge \inf_{\pi \in D}(\Phi(\pi)) \quad \forall n\\]

The intuition here is that we can only do as good as the best plan
available. It follows from the Lebesgue Convergence Theorem that,

\\[ V^{*}(\pi) = \lim_{n \to \infty} V^{n}(\pi) \\]

\\[= \min \left(\Phi(\pi), \lim_{n \to \infty} \int_D V^{n-1}(\pi) \ dP_x(y) + 1 \right)\\]

\\[ = \min \left(\Phi(\pi), \int_{D} V^{*}(\pi) \ dP_x(y) + 1 \right) \\]

\\[\Rightarrow \inf_{\pi \in D} V^*(\pi) \ge \inf_{\pi \in D} \Phi(\pi)\\]

Okay, now we need to show that our solution is unique. The idea of the
proof is that if you had more than a single solution, say \\(V\\) and
\\(U\\), there will have to be regions where one policy will go for
continuing and the other won’t. If we consider a region where \\(U < V\\)
this means that the advantage of continuing will be greater if you use
\\(U\\). On the other hand, over the region where the value of \\(V\\) is
greater than \\(U\\) there will also have to be a maximum difference
between the value functions. However, this would mean that the advantage
function of \\(U\\) will need to be greater than \\(V\\) which is a
contradiction.

Given the sketch, it sounds like we’ll have to demand that \\(\Phi(D)\\)
be compact. We’ll show after the proof that it’s possible to relax this
condition to an assertion that it’s always possible to transition to a
certain set of policies.

**Theorem 3.2.** Let \\(\Phi(D)\\) be a compact topological space and for \\(\pi \in D\\) let
\\(P_x(A) > 0\\) for all open sets in \\(A \in D\\). Then there’s at most a
single solution such that \\((i)\\) \\(V\\) is continuous and \\((ii)\\)
\\(\inf_{\pi \in D} V(\pi) = \inf_{\pi \in D} \Phi(x)\\).

*Proof.* For a function \\(R(x)\\) that measures execution time we define the
advantage as \\(\gamma_r(\pi) = R(x) - \int_D R(y) \ dP_x(y)\\). Think of
this as a measure of the change in expected execution time. If it’s
positive the current state has an advantage over the next expected
state. We define things this way so that if \\(R\\) is a solution of
\\((i)\\) then it follows that \\(\gamma_r(x) \le 1\\). To see this note
that subtracting \\(V\\) from the left-hand side of \\((1)\\) needs to give
zero. Now, imagine we have two solutions \\(U\\) and \\(V\\) for \\((1)\\).
Let,

\\[ S_1 = \{ x | V(x) = U(x) \}, \quad S_2 = \{ x | V(x) > U(x) \}, \quad S_3 = \{ x | V(x) < U(x) \} \\]

We’ll show that \\(S_2 = S_3 = \emptyset\\). Define
\\(W(x) = \max(U(x),V(x))\\) then for \\(x \in S_1 \cup S_2\\) we have
\\(W(x) = V(x)\\) and for all \\(x \in D\\),
\\(\int_D V(y) \ dP_x(y) \le \int_D W(y) \ dP_x(y)\\). From this we
conclude that \\(\gamma_w(x) \le \gamma_v(x) \le 1\\). Since
\\(U(x) < V(x)\\) and \\(\inf(V(x)) = \inf(\Phi(x))\\) we conclude that we
have to have \\(\gamma_u(x) = 1\\) or else \\((ii)\\) won’t be satisfied.
We’re about to run into a problem. We know from the compactness of
\\(\Phi(D)\\) that that there exists \\(x_0\\) such that,

\\[W(x_0) - V(x_0) = \max(W(x) - V(x)) > 0\\]

\\[\Rightarrow W(x_0) - V(x_0) > \int_D W(y) - V(y) \ dP_x(y)\\]

\\[\Rightarrow \gamma_w(x_0) > \gamma_v(x)\\]

This is a contradiction so it must be the case that \\(S_2 = \emptyset\\).
By symmetry of \\(U\\) and \\(V\\) we can also eliminate \\(S_3\\).

Let \\(\Phi(D)\\) be a measurable space where there exists a non-empty
\\(\Phi(D')\\) such that \\(\Phi(D') \subset \Phi(D)\\) and \\(P_x(D') > 0\\)
for all \\(x \not \in D'\\). Then we have a single solution \\(V\\) that is
measurable and bounded.

We can use the same argument as above. However, because we lost
compactness we must look at the infimum which will have a value of say
\\(-M\\). We fix \\(\epsilon < M \delta (1+\delta)^{-1}\\) and then we can
get a point \\(x_{\epsilon}\\) inside of the set we took an infimum over.
Now when we integrate over \\(S\\) we can extract \\(S_{\epsilon}\\) and
everything will work out.
