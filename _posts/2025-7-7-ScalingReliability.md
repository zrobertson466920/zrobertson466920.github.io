---
layout: post
title: Can Humans Reliably Detect Scaling Laws - A Reality Check
published: true
---

*Taking a measurement theory approach to examine one human rule and three GPT-o3 prompts*

[Scaling Laws Are Unreliable for Downstream Tasks: A Reality Check](https://arxiv.org/abs/2507.00885)

A recent paper by Lourie, Hu, and Cho made waves by claiming that downstream scaling laws only work reliably 39% of the time. Their meta-analysis of 46 tasks found that most exhibit "degenerate" scaling behaviors - inverse scaling, nonmonotonic patterns, or just noise rather than the clean linear relationships we'd hope for. But as I dug into their methodology, a fundamental question emerged: how reliable is the classification of "predictable" versus "unpredictable" scaling itself?

This question matters because if different reasonable observers disagree substantially on which plots show predictable scaling, then the headline "39%" figure becomes meaningless. It's like claiming "39% of paintings are beautiful" without establishing any agreement on what constitutes beauty.

#### The Problem with Eyeballing

The paper's approach to classifying scaling behaviors is surprisingly informal. They define predictable scaling as fitting "closely to a linear functional form after, for example, exponentiating the cross-entropy loss" and state that "y = a exp{c·x} + b" represents the best case scenario where "y is the task performance metric and x is the validation perplexity."

But how closely is "closely"? The paper provides no quantitative thresholds, no R² cutoffs, no criteria for acceptable deviation. Instead, they simply assert that Figure 5 shows the 18 tasks with "smooth, predictable improvement" while Figures 6-10 show the remaining 28 tasks grouped into various "degenerate scaling behaviors." These categories include: predictable, non-monotonic, noisy scaling, inverse scaling, trendless, and breakthrough. 

This subjective visual inspection has well-known problems in measurement theory. Different observers bring different priors, different tolerance for noise, and different implicit definitions of what constitutes a "trend." Without explicit criteria, we're building scientific conclusions on quicksand.

#### The Experiment Setup

I analyzed the same 46 scatter plots from the paper by looking at the raw file and extracting the 6 figures they spread their classifications across. I used multiple AI annotation approaches:

* **The human "ground rule"**: Following the paper's classification - everything in their Figure 5 counts as "Predictable" (18 out of 46 plots), while everything else is "Not Predictable."

* **Attention-check**: since an AI could possibly hallucinate labels, we verify they were able to read and parse all the figures by having them specify the count and total number of plots labeled predictable. 

* **Four GPT-o3 prompts** with varying interpretations of "predictable scaling". The actual chats are available at the respective links:
  * **[c1](https://chatgpt.com/share/686c39ea-d0c0-800c-8dcc-f11e913586e4)** – Ultra-strict criteria requiring monotonic improvement, R² > 0.85, and less than 5% outliers
  * **[b1 and b2](https://chatgpt.com/share/686c39cf-898c-800c-bd9c-d412a700a277)** – Following the paper's definition of "roughly linear after transformation" with explicit count targets
  * **[s3](https://chatgpt.com/share/6867f0e7-aba4-800c-ae2e-7b58811028e0)** – Same definition as b1/b2 but without specifying the attention check in the prompt

#### Measuring Reliability

Information theory gives us an elegant way to quantify annotator reliability. We can treat each annotator's labels of the items as a random variable and calculate an f-mutual-information score between them. Using total-variation f-divergence (TVD) gives us:

$$I_{\mathrm{TVD}}(X;Y)=\tfrac12\!\sum_{i,j}P_{X}(i) P_{Y}(j)\,\bigl|\tfrac{P_{XY}(i,j)}{P_X(i)P_Y(j)}-1\bigr|$$

This has a maximum value of 0.5, so I multiply by 2 to put it on a 0-1 scale where 1 means perfect agreement. Unlike Cohen's kappa, this approach avoids quirky "expected agreement" assumptions and stays firmly grounded in information theory. In fact, it has a mathematically concise interpretation. The TVD-MI score is the accuracy a perfect classifier would be able to achieve when tasked with classifying whether a given pair of labels came from the paired annotations for a given item (to be classified) or randomly paired annotations from independently sampled items at random.

#### What the Results Reveal

We show the classifications by trend-type (as used by the authors) and include full annotations in the appendix.

| Trend type (total)             |  human      |   c1        |   b1        |   b2        |   s3        |
|--------------------------------|-------------|-------------|-------------|-------------|-------------|
| 💥 Breakthrough&nbsp;(2)        | &nbsp;0 / 2&nbsp; | &nbsp;0 / 2&nbsp; | &nbsp;2 / 2&nbsp; | &nbsp;2 / 2&nbsp; | &nbsp;2 / 2&nbsp; |
| 🔄 Inverse&nbsp;(4)             | &nbsp;0 / 4&nbsp; | &nbsp;0 / 4&nbsp; | &nbsp;0 / 4&nbsp; | &nbsp;0 / 4&nbsp; | &nbsp;0 / 4&nbsp; |
| 📊 Noisy&nbsp;(7)               | &nbsp;0 / 7&nbsp; | &nbsp;2 / 7&nbsp; | &nbsp;5 / 7&nbsp; | &nbsp;7 / 7&nbsp; | &nbsp;4 / 7&nbsp; |
| 🔀 Non-monotonic&nbsp;(6)       | &nbsp;0 / 6&nbsp; | &nbsp;0 / 6&nbsp; | &nbsp;2 / 6&nbsp; | &nbsp;3 / 6&nbsp; | &nbsp;2 / 6&nbsp; |
| 📈 Predictable&nbsp;(18)        | &nbsp;18 / 18&nbsp; | &nbsp;1 / 18&nbsp; | &nbsp;15 / 18&nbsp; | &nbsp;18 / 18&nbsp; | &nbsp;15 / 18&nbsp; |
| ➖ Trendless&nbsp;(9)           | &nbsp;0 / 9&nbsp; | &nbsp;0 / 9&nbsp; | &nbsp;0 / 9&nbsp; | &nbsp;0 / 9&nbsp; | &nbsp;0 / 9&nbsp; |

We then calculate the reliability matrix which forms the basis for making valid interpretations about the results:

|           |  human  |   c1    |   b1    |   b2    |   s3    |
|-----------|---------|---------|---------|---------|---------|
| **human** | &nbsp;0.95&nbsp; | &nbsp;0.02&nbsp; | &nbsp;0.49&nbsp; | &nbsp;0.54&nbsp; | &nbsp;0.52&nbsp; |
| **c1**    | &nbsp;0.02&nbsp; | &nbsp;0.24&nbsp; | &nbsp;0.12&nbsp; | &nbsp;0.09&nbsp; | &nbsp;0.13&nbsp; |
| **b1**    | &nbsp;0.49&nbsp; | &nbsp;0.12&nbsp; | &nbsp;1.00&nbsp; | &nbsp;0.73&nbsp; | &nbsp;0.87&nbsp; |
| **b2**    | &nbsp;0.54&nbsp; | &nbsp;0.09&nbsp; | &nbsp;0.73&nbsp; | &nbsp;0.91&nbsp; | &nbsp;0.70&nbsp; |
| **s3**    | &nbsp;0.52&nbsp; | &nbsp;0.13&nbsp; | &nbsp;0.87&nbsp; | &nbsp;0.70&nbsp; | &nbsp;1.00&nbsp; |

Finally, we can look at the mean reliability of each annotator:

| annotator | Σ row | avg. reliability |
|-----------|-------|----------------|
| human     | &nbsp;1.57&nbsp; | &nbsp;0.39&nbsp; |
| c1        | &nbsp;0.36&nbsp; | &nbsp;0.09&nbsp; |
| b1        | &nbsp;2.21&nbsp; | &nbsp;0.55&nbsp; |
| b2        | &nbsp;2.06&nbsp; | &nbsp;0.51&nbsp; |
| s3        | &nbsp;2.22&nbsp; | &nbsp;0.55&nbsp; |

#### What This Means

Several striking patterns emerge:

**Different reasonable interpretations yield wildly different results.** The ultra-strict c1 prompt identifies only 3 plots as predictable - a far cry from the paper's 18. Even the prompts (b1/b2/s3) that explicitly try to follow the paper's definition of "linear after transformation" identify around 25-27 plots as predictable, not 18.

First, the AI annotators aren't just monotonically expanding the set of predictable plots. Since `s3` scores highest in our framework we will walk through some examples. The authors label both plots below as "predictable" while AI labels only CoQA as "predictable".

![AI-Human disagreement from the "predictable" set](https://i.ibb.co/b5gNjXtj/Screenshot-2025-07-10-at-5-58-15-PM.png)

Second, the labels elsewhere are reasonable. The `s3` prompt labels all plots the authors categorize as "trendless" as "not predictable". The "breakthrough" (sigmoid) plots are labeled "predictable". Some of the "noisy scaling" plots are labeled "predictable", but not the  most noisy ones. 

**The human rule isn't reproducible.** The paper's classification achieves only moderate agreement (0.36-0.52) with systematic attempts to apply their own stated definition. Another (variational) interpretation of the TVD-MI score is that a perfect classifier would be able to tell if a given pair of labels came from a human vs. AI with probability (0.68-0.76). This suggests their visual grouping into figures may have been influenced by factors beyond the stated "linear after transformation" criterion.

**Standardized prompts achieve high mutual agreement.** The b1 and b2 prompts agree 91% of the time despite being run independently, showing that explicit criteria can produce reproducible classifications. They form a tight cluster with s3 (around 70% mutual agreement), suggesting they're capturing a coherent notion of "predictable scaling."

#### Why This Matters for the Field

The Lourie et al. paper makes important points about the unreliability of scaling laws, but my analysis suggests we need to be equally concerned about the reliability of our measurements of scaling behavior itself. When "predictable" counts can swing from 3 to 27 depending on how one interprets "roughly linear after transformation," the precise "39%" figure loses much of its meaning.

This isn't to dismiss the paper's core insights. Their examples of how validation data choice can reverse scaling conclusions and how experimental setup affects scaling behavior remain valuable contributions. But without reliable, reproducible classification of scaling behaviors, we can't meaningfully track whether the field is making progress on understanding when scaling laws work.

#### Lessons for Better Measurement

This analysis points to several ways we could improve how we study scaling laws:

1. **Make decision rules explicit and numerical.** Instead of eyeballing, use criteria like "R² ≥ 0.9 when fitting y = a·e^(cx) + b". This would eliminate the prompt lottery effect we see here.

2. **Always use multiple raters and report agreement.** Whether using humans or AI, having at least two independent annotations with published agreement statistics (using TVD-MI, Cohen's kappa, or standard mutual information) should be standard practice.

3. **Don't hide uncertainty in headline numbers.** When "predictable" counts can swing from 3 to 30 depending on reasonable interpretation differences, stating "39%" without confidence intervals is misleading at best.

Until scaling law studies adopt these basic reliability practices from measurement theory, our debates about "emergence" versus "predictability" will be shaped more by labeling conventions than by actual empirical patterns. The field deserves better than arguments built on sand.

If this was useful to you, please consider citing. 

```
@misc{robertson2025humans,
  title={Can Humans Reliably Detect Scaling Laws? A Reality Check},
  author={Robertson, Zachary},
  year={2025},
  month={July},
  institution={Stanford University},
  url={https://zrobertson466920.github.io/ScalingReliability/},
  note={Blog post analyzing inter-annotator agreement on scaling law classification using TVD mutual information}
}
```

### Appendix

Here is the full table with annotations:

| Benchmark (image)                                    |  human |   c1  |   b1  |   b2  |   s3  |
|------------------------------------------------------|--------|-------|-------|-------|-------|
| BIG-bench: CS Algorithms (breakthrough)              | ❌ | ❌ | ✅ | ✅ | ✅ |
| PubMed QA Labeled (breakthrough)                     | ❌ | ❌ | ✅ | ✅ | ✅ |
| AGIEval LSAT LR (inverse)                            | ❌ | ❌ | ❌ | ❌ | ❌ |
| AGIEval LSAT RC (inverse)                            | ❌ | ❌ | ❌ | ❌ | ❌ |
| AGIEval SAT English (inverse)                        | ❌ | ❌ | ❌ | ❌ | ❌ |
| BIG-bench: Elementary Math QA (inverse)              | ❌ | ❌ | ❌ | ❌ | ❌ |
| BIG-bench: Novel Concepts (noisy)                    | ❌ | ✅ | ✅ | ✅ | ✅ |
| BIG-bench: Strategy QA (noisy)                       | ❌ | ❌ | ✅ | ✅ | ✅ |
| BoolQ (noisy)                                        | ❌ | ❌ | ✅ | ✅ | ✅ |
| LogiQA (noisy)                                       | ❌ | ✅ | ✅ | ✅ | ✅ |
| Simple Arithmetic (NoSpaces) (noisy)                 | ❌ | ❌ | ❌ | ✅ | ❌ |
| Simple Arithmetic (WithSpaces) (noisy)               | ❌ | ❌ | ❌ | ✅ | ❌ |
| SIQA (noisy)                                         | ❌ | ❌ | ✅ | ✅ | ❌ |
| ARC-Challenge (non-monotonic)                        | ❌ | ❌ | ✅ | ✅ | ✅ |
| BBQ (non-monotonic)                                  | ❌ | ❌ | ❌ | ❌ | ❌ |
| BIG-bench: Logical Deduction (non-monotonic)         | ❌ | ❌ | ❌ | ❌ | ❌ |
| BIG-bench: Strange Stories (non-monotonic)           | ❌ | ❌ | ❌ | ❌ | ❌ |
| Commonsense QA (non-monotonic)                       | ❌ | ❌ | ❌ | ✅ | ❌ |
| COPA (non-monotonic)                                 | ❌ | ❌ | ✅ | ✅ | ✅ |
| ARC-Easy (predictable)                               | ✅ | ❌ | ✅ | ✅ | ✅ |
| BIG-bench: Conlang Translation (predictable)         | ✅ | ❌ | ❌ | ✅ | ❌ |
| BIG-bench: Dyck Languages (predictable)              | ✅ | ❌ | ✅ | ✅ | ❌ |
| BIG-bench: Operators (predictable)                   | ✅ | ❌ | ✅ | ✅ | ✅ |
| BIG-bench: QA Wikidata (predictable)                 | ✅ | ✅ | ✅ | ✅ | ✅ |
| BIG-bench: Repeat Copy Logic (predictable)           | ✅ | ❌ | ❌ | ✅ | ❌ |
| CoQA (predictable)                                   | ✅ | ❌ | ✅ | ✅ | ✅ |
| HellaSwag (predictable)                              | ✅ | ❌ | ✅ | ✅ | ✅ |
| HellaSwag (zero-shot) (predictable)                  | ✅ | ❌ | ✅ | ✅ | ✅ |
| Jeopardy (predictable)                               | ✅ | ❌ | ❌ | ✅ | ✅ |
| LAMBADA (predictable)                                | ✅ | ❌ | ✅ | ✅ | ✅ |
| MMLU (5-shot) (predictable)                          | ✅ | ❌ | ✅ | ✅ | ✅ |
| MMLU (zero-shot) (predictable)                       | ✅ | ❌ | ✅ | ✅ | ✅ |
| OpenBook QA (predictable)                            | ✅ | ❌ | ✅ | ✅ | ✅ |
| PIQA (predictable)                                   | ✅ | ❌ | ✅ | ✅ | ✅ |
| SQuAD (predictable)                                  | ✅ | ❌ | ✅ | ✅ | ✅ |
| Winograd (predictable)                               | ✅ | ❌ | ✅ | ✅ | ✅ |
| WinoGrande (predictable)                             | ✅ | ❌ | ✅ | ✅ | ✅ |
| AGIEval LSAT AR (trendless)                          | ❌ | ❌ | ❌ | ❌ | ❌ |
| BIG-bench: Conceptual Combinations (trendless)       | ❌ | ❌ | ❌ | ❌ | ❌ |
| BIG-bench: Language Identification (trendless)       | ❌ | ❌ | ❌ | ❌ | ❌ |
| BIG-bench: Misconceptions (trendless)                | ❌ | ❌ | ❌ | ❌ | ❌ |
| BIG-bench: Understanding Fables (trendless)          | ❌ | ❌ | ❌ | ❌ | ❌ |
| Enterprise PII Classification (trendless)            | ❌ | ❌ | ❌ | ❌ | ❌ |
| MathQA (trendless)                                   | ❌ | ❌ | ❌ | ❌ | ❌ |
| WinoGender MC: Female (trendless)                    | ❌ | ❌ | ❌ | ❌ | ❌ |
| WinoGender MC: Male (trendless)                      | ❌ | ❌ | ❌ | ❌ | ❌ |


