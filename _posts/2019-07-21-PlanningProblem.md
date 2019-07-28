---
layout: post
title: Planning Problem
published: true
---

It's fair to say that we spend a lot of our lives floating from task to task. Some are work and some are not. Nevertheless, they are tasks. Things that we want to do. Some are complicated are require planning. However, since planning is such an integral step in completing more complicated tasks it seems as though we might want to "meta-plan" or plan our planning. I became fascinated in the concept after I realized it should be possible to frame the whole thing as a sort of optimization problem. In practice, the problem remains complicated by the fact that the optimization occurs in a necessarily online fashion. However, any reasonable method of planning is especially well behaved in the formalism I'll present. Moreover, even when the method is faulty due to human bias, as long as it is consistent we still end up being to prove that meta-planning, in a certain sense, is rational.

One particularly recrusive aspect of the problem is that in order to carry out a task to completion we must already have a plan that we execute. The plan could be written in an ‘online’ manner, which is done in improvisation. Still, the line between planning and doing at this point becomes very blurry and quickly leads to a chicken/egg problem. Which came first, knowledge of a plan or how to execute it? I’ll bound the scope of this using a few concrete examples and then we’ll jump off into abstraction to step around the recursion.

# A First Example

For me, the most natural first example would be this article. I don’t want to spend too much time planning out the essay. On the other hand, I don’t want to plan so little that the organization and flow is poor. Usually I spend a bit too much time in the planning phase so this article is an experiment in trying to do something that is a bit more optimal. What do I mean by more optimal? The general idea is that you are carrying out some task, such as writing an article and you can spend some amount of time thinking and planning out a strategy to achieve you’re goal. After that you carry out the task or write the article. Imagine a task like the traveling salesman. If there are enough cities and enough distance between them most people would agree that having a clear itenerary is going to be more effective than just visiting cities at random. In this example, it's clear that there must be a trade-off given that the general algorithm for planning would be \\(NP\\) complete!

The intuition seems to be that there is a strategy that is able to balance between the objectives of planning and executing in an optimal way. However, over the years I’ve caught myself making the error of measuring the effectiveness of having a plan or strategy in terms of how much time it saves me in carrying out the task. Yet, I will commonly ignore the time spent on creating the strategy in the first place. For example, while I was writing this article I spent a significant amount of time crafting a uniqueness proof for the optimal strategy I’ll propose in next section. Was that really optimal? I had a picture of the proof done in under an hour. If you’d asked me right after I’d proved the theorem whether or not it was a good use of time I’d say "Yes, but it wasn’t necessary for the article." So it’s possible that deviations from this supposed optimal strategy might be part some other strategy for the optimization of a different quantity. However, the case remains that making the proof was not an optimal use of my time if I claim the purpose was for the completion of the article. 

It's worth pointing out that people are notoriously bad at estimating how much time things will take (see [planning fallacy](https://en.wikipedia.org/wiki/Planning_fallacy)). Moreover, we’re poor at estimating the amount of time we spend on tasks with variable time requirements. Thus, when we decide to create a plan we often neglect to estimate the cost of taking such an action and we often fail to measure whether or not the plan was successful. It’s a sort of recursive issue since usually we are trying to use the plan to estimate the cost of taking another action. In some sense what I’m trying to do to facilitate analysis is contain the infinite-regress to a second-order meta-plan. 

# A formal statement

Given that there are some vague notions surrounding the ideas of planning, executing, and optimality let's put things on a more formal footing before going into detail. Assume that there is an oracle that could tell you how long a task would take without a dedicated plan. Say that for the task of writing this article it returned seven hours. If I think that I planning could help reduce this time I can take a bet with respect to this oracle. I would place a bet that the time it takes to make a plan plus the time it takes to execute the resulting plan is less than the time returned by oracle. If not, I lose the bet. For example, I might spend 6 hours creating a detailed plan and then spend two hours executing for a total of eight hours. In this case I lose. Even though I was able to significantly reduce the execution time, I spent way too much time planning to make the strategy effective and ultimately spent more time on the task than if I had just started executing from the start.

Same as the above, but with the level of formality neccessary to make a rigourous argument. Let's say you have to complete some task \\(T\\). Suppose also that you have
some sort of initial policy \\(\pi_0\\) that delineates all the steps you
need to take to complete \\(T\\). Moreover, you also have an oracle
\\(\Phi\\) that will produce an estiamte of you how long it will take to execute \\(\pi\\).
Naturally, we’ll place a restriction on \\(\Phi(\pi)\\) so that it’s
bounded and always equal to or greater than zero. Now, say you can
invest \\(t\\)-time and plan \\(\pi_0 \rightarrow \pi_t\\) in the sense that
it’s always the case that \\(\Phi(\pi_0) \ge \Phi(\pi_t)\\). Thus, with a
slight abuse of notation we can write the result of planning \\(\pi\\) for
\\(t\\)-time as \\(\Phi(t)\\). Naturally, our goal is to minimize the total
amount of time \\(\Gamma(t) = t+\Phi(t)\\) it takes to execute the
planning and the resulting policy.

In the case where everything is nice \\(\Phi \in C^1\\) and is convex. We
know that it must be the case that \\(\Phi(t) \ge 0\\). Thus, it should
follow that \\(\lim_{t \to \infty} \Phi(t)\\) exists and that
\\(\lim_{t \to \infty} \Phi^{'}(t) = 0\\). The point is that condition for
the minima is easy to state and is equivalent to when
\\(\Phi^{'}(t) = -1\\). Thus, as long as the benefit rate of planning is faster
than the rate at which time progresses there will be an optimal planning time \\(t'\\).

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

It's worth noting a connection between this simplisitc model and the
simplistic model we usually use to make plans. Usually, we make plans by
creating lists of sub-tasks or steps that need to be completed. Sometimes,
we iterate this process if the sub-tasks are difficult to complete. In an idealized model
we'd model the creation of a task list using a branching process and then claim
that the execution time of a task is reduced proportional to the number of leaves
in the resulting tree. 

## b)

There is an important quanity here that can be used to make a practical algorithm.
From the first example we showed that the task reduction quantity \\(\lambda T_0\\)
governs the effectiveness of planning. The requirement that
\\(\lambda T_0 > 1\\) is the same as demanding that the rate of reduction
from planning be greater than one.

For many common tasks it’s immediately obvious whether or not a plan
should be constructed. Make the assumption that this decision rule is
a statistic of \\(\lambda T_0\\). It follows that we can estimate the
reduction in total time by looking at the current
plan for execution at each time step. Once this estimate of the total
time reaches a minimum we should stop planning and start executing.

The pathological aspect of this algorithm is that there might be
a critical threshold that needs to be reached before planning is
effective. Consider that we estimate \\(\lambda T_0\\), but the
true model for \\(\Phi\\) has a varying rate equation. However, even in the worst
case we’d only double the amount of time taken to complete the task. The
reasoning is that in order for planning to be an effective strategy the
minimum must be reached sometime before the estimated time for
execution of the intial plan. If this assumption is violated our original hypothesis would be as
well which means that are no guarantees about when or if a minimum might
be reached.

It would seem as though we could layer more complexity onto these simple
models to make them more effective. For instance, we can formalize what
it means to determine whether or not our guess is correct by using
statistical estimators. It also seems as though the convexity
requirement on \\(\Phi\\) is a bit strong for our purposes since planning
is a somewhat complicated and somewhat arbitrary/random affair. Finally,
the fact that we can create plans that have sub-plans means that we can
recursively call our meta-plan for making plans. We'll come back to this last
point in a bit.

# General Strategy

As noted in the previous section, we’ve restricted \\(\Phi\\) so severely
that we might be concerned it would tell us nothing about whether or not it’s a good idea in general
to try and optimize \\(\pi\\). This is because we have no guarantee that
our optima at \\(t'\\) is better that \\(t+\Phi(t)\\) for \\(t \in [0,t']\\).
Now, lucky for us most of this has already been studied in the context
of optimal persistent policy theory. The theory deals with optimal selection
problems where the \\(n\\)-th observation costs \\(c(n)\\). Thus, we can
use this tool and assume we can construct a sequence of plans
\\(\lbrace \pi_t \rbrace_{t \ge 0}\\) and incur unit cost for each plan we create.

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
\\((1)\\) then it follows that \\(\gamma_r(x) \le 1\\). To see this note
that subtracting \\(V\\) from the left-hand side of \\((1)\\) needs to give
zero. Now, imagine we have two solutions \\(U\\) and \\(V\\) for \\((1)\\).
Let,

\\[ S_1 = \lbrace x \mid V(x) = U(x) \rbrace, \quad S_2 = \lbrace x \mid V(x) > U(x) \rbrace, \quad S_3 = \lbrace x \mid V(x) < U(x) \rbrace \\]

We’ll show that \\(S_2 = S_3 = \emptyset\\). Define
\\(W(x) = \max(U(x),V(x))\\) then for \\(x \in S_1 \cup S_2\\) we have
\\(W(x) = V(x)\\) and for all \\(x \in D\\),

\\[\int_D V(y) \ dP_x(y) \le \int_D W(y) \ dP_x(y)\\]

From this we conclude that \\(\gamma_w(x) \le \gamma_v(x) \le 1\\). Since
\\(U(x) < V(x)\\) and \\(\inf(V(x)) = \inf(\Phi(x))\\) we conclude that we
have to have \\(\gamma_u(x) = 1\\) or else \\((ii)\\) won’t be satisfied.
We’re about to run into a problem. We know from the compactness of
\\(\Phi(D)\\) that that there exists \\(x_0\\) such that,

\\[W(x_0) - V(x_0) = \max(W(x) - V(x)) > 0\\]

\\[\Rightarrow W(x_0) - V(x_0) > \int_D W(y) - V(y) \ dP_x(y)\\]

\\[\Rightarrow \gamma_w(x_0) > \gamma_v(x)\\]

This is a contradiction so it must be the case that \\(S_2 = \emptyset\\).
By symmetry of \\(U\\) and \\(V\\) we can also eliminate \\(S_3\\).

**Corollary 3.3.** Let \\(\Phi(D)\\) be a measurable space where there exists a non-empty
\\(\Phi(D')\\) such that \\(\Phi(D') \subset \Phi(D)\\) and \\(P_x(D') > 0\\)
for all \\(x \not \in D'\\). Then we have a single solution \\(V\\) that is
measurable and bounded.

We can use the same argument as above. However, because we lost
compactness we must look at the infimum which will have a value of say
\\(-M\\). We fix \\(\epsilon < M \delta (1+\delta)^{-1}\\) and then we can
get a point \\(x_{\epsilon}\\) inside of the set we took an infimum over.
Now when we integrate over \\(S\\) we can extract \\(S_{\epsilon}\\) and
everything will work out.

# Redux

Does this information help illuminate anything? Returning to the article example given at the beginning, it would seem as though I spent too much time planning the mathematical portion of the writing. I'm about done with the article and I see that the mathematical portion grew in scope way beyond what was intended in the original plan. This deviation was significant due to me not realizing that the article would need to be formatted. I think that if I had realized how much time it would take to format the article, more importantly latex, it would've become readily apparent that I should've beeen more sparing in the amount of math I used. Overall, I spent about 14 hours on this article while my original estimate was about 7. I spent several hours on making mathematical proofs when I really should've been reading up on how to format an article with latex correctly. I also think that I could've spent a bit more time proof-reading everything before submitting.

What ultimately happened was that while I created an initial plan for the article, the unexpected addition of mathematical arguments broke all my estimates. I should've realized that formatting latex would take a long time since it was my first attempt. It would seem as though the ideal of clean separation between planning and execution is an abstraction. The reality is that we all switch between planning and executing whenever we engage with suffiently complex tasks. I only planned the structure of the article for about an hour because I was overly optimisitc about how long it would take to finish the whole thing. I fell prey to the planning fallacy.

Does this mean this idea failed? Not exactly. Recall the idealized model of planning. When we make plans we oftentimes will create tasks that themselves require further planning. It seems perfectly reasonable that we could simply "call" our meta-plan on these sub-tasks in order to reduce the effect of human bias. I was overly optimistic in thinking about how long it would take to format the math. This problem could've been heavily mitigated if I had a plan that made the task of making the mathematical section a sub-plan. If I had done this, I would've had a chance to look at the bigger picture and most likely would've proceeded differently.

# Outlook

Ultimately, I think that the idea was a success because it was developed to enough generality to be useful in getting a concept handle on some pretty big quesetions surrounding the idea of meta-planning. First, what I am currently engaging in is the construction of a plan that when executed can manage the planning of plans. While I showed that for a broad class of tasks meta-planning is rational, for a much broader class of problems it isn't. Not all problems are [compressable](https://en.wikipedia.org/wiki/Algorithmic_complexity). This means that oftentimes the most effective plan will be to have no real plan at all. You can't plan for this either. Pursuing this line of reasoning is difficult as you quickly reach the limit of rationality due to the undecidability of the various questions involved. Perhaps this is a reason for less than perfect rationality in humans. Second, we're overly optimistic about how much time our plans will take to execute. However, when we realize that our plans were too optimistic we are more than capable of updating them because we naturally make hierachical or tree like plans. Using these three observations I conclude that the current model I've developed is sufficient to fine-tune itself for the plans that any systematic method could hope to be capable of tackling. There is no need to continue worrying about abstract details at the meta-level. I need to practice using what I came up with.
