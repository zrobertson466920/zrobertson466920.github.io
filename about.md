---
layout: page
title: About
permalink: /about/
description: Stanford CS PhD building info-theoretic foundations for evaluation infrastructure
---

I'm a CS PhD at Stanford working on **scalable oversight**; information-theoretic mechanisms to evaluate AI systems **without ground truth**. I work end-to-end: **proofs → benchmarks → pilots**.

### Highlights
- **Funding:** Lead on OpenAI **Superalignment Fast Grant** ($500k, 2024).
- **Result:** Info-theoretic framework for **black-box LLM evaluation without ground truth** with **10–100×** robustness to adversarial manipulation vs. prior methods ([arXiv:2508.05469](https://www.arxiv.org/abs/2508.05469), Aug 2025).
- **Adoption/recognition:** Initial production deployments ([LinkedIn](https://www.linkedin.com/posts/florian-h%C3%B6nicke-b902b6aa_icml-icml24-icml2024-activity-7224064255677849601-wdgS)); invited lightning talk at *Building an Aligned AI Future* (Fifty Years @ Stanford, Sept 2025) ([event](https://luma.com/d1wxjy6s?tk=KM9dGC)).

### Research Program
**Theory.** We link incentive-compatible scoring rules to **f-mutual information**, showing **bounded f-divergences** (e.g., TVD) maintain polynomial robustness under strategic attacks while **unbounded** ones (e.g., KL) can degrade exponentially.  

**Experiments/Implementation.** We instantiate these mechanisms for **LLM evals without ground truth**, where querying for **information relationships** (not quality judgments) yields robust item-level scores. In practice, **TVD-MI** achieves strong AUC under attack; code + preregistration released.

**Field validation.** Technical framework connects peer prediction (game theory) with item response theory (psychometrics) via maximum entropy principles. Concurrent emergence of multiple evaluation platforms and institutional pivots (AISI Safety→Measurement rebrand, Dec 2025) validate urgency of gaming-resistant evaluation infrastructure. Feedback from mechanism-design experts (e.g., [Yuqing Kong](https://cfcs.pku.edu.cn/yuqkong/)).

### Adoption & Recognition
- ICML 2024 highlight; early production deployments ([LinkedIn](https://www.linkedin.com/posts/florian-h%C3%B6nicke-b902b6aa_icml-icml24-icml2024-activity-7224064255677849601-wdgS))
- Invited lightning talk: *Paying for Information, Not Vibes* at **Building an Aligned AI Future** (Fifty Years @ Stanford, Sept 2025) ([event](https://luma.com/d1wxjy6s?tk=KM9dGC))
- **Open science:** preregistration ([OSF](https://osf.io/c7pum)), code, and complete proofs

### Research Leadership & Funding
- **Lead & primary researcher**, OpenAI Superalignment Fast Grant ($500k, 2024): conceived agenda; executed from proposal to validation
- **Advocacy:** Led *WellLabeled* (Stanford HAI) on data-annotation worker rights; interviews with Turkopticon, Scale AI, and OpenAI; featured by Stanford HAI ([article + video](https://hai.stanford.edu/news/exploring-complex-ethical-challenges-data-annotation), Jul 2024); invited talk at FloodGate
- **First-author** theoretical and empirical work connecting mechanism design to ML evaluation

### Talks & Public Engagement
- *Paying for Information, Not Vibes.* Lightning talk, **Building an Aligned AI Future** (Fifty Years @ Stanford), Sept 2025
- Invited talks (selected): Max Planck Institute for Intelligent Systems; FloodGate (2024)

### Background (select)
- **Theory:** BS, Computational & Applied Math (UChicago)
- **Systems:** MS, CS (UIUC): metric elicitation & evaluation systems
- **Industry:** Google (ads welfare optimization); Lam Research (RL for semiconductor process control)
- **Research:** Toyota Technological Institute (perception & language grounding)

### News
- **Sept 2025:** Invited lightning talk at *Building an Aligned AI Future* (Fifty Years @ Stanford) — [event](https://luma.com/d1wxjy6s?tk=KM9dGC)
- **Aug 2025:** First-author paper on evaluation mechanisms featured on arXiv front page
- **July–Dec 2024:** Invited talks: Max Planck Institute for Intelligent Systems; FloodGate
- **April 2024:** OpenAI Superalignment Fast Grant awarded ($500k)
- **2023–2024:** Led WellLabeled advocacy group on data-annotation worker rights at Stanford HAI

### Contact
[zroberts@stanford.edu](mailto:zroberts@stanford.edu)

[CV](https://github.com/zrobertson466920/zrobertson466920.github.io/blob/master/Awesome_CV-3.pdf) · [Google Scholar](https://scholar.google.com/citations?user=769PIisAAAAJ&hl=en) · [GitHub](https://github.com/zrobertson466920)

_Last updated: September 2025._

### Selected Publications

1. **Zachary Robertson** and Sanmi Koyejo. [Let's Measure Information Step-by-Step: LLM-Based Evaluation Beyond Vibes](https://arxiv.org/abs/2508.05469). arXiv:2508.05469, 2025
2. Olawale Salaudeen, Anka Reuel, Ahmed Ahmed, Suhana Bedi, **Zachary Robertson**, Sudharsan Sundar, Ben Domingue, Angelina Wang, and Sanmi Koyejo. [Measurement to Meaning: A Validity-Centered Framework for AI Evaluation](https://arxiv.org/abs/2505.10573). arXiv:2505.10573, 2025
3. **Zachary Robertson** and Sanmi Koyejo. [Implicit Regularization in Feedback Alignment Learning Mechanisms for Neural Networks](https://proceedings.mlr.press/v235/robertson24b.html). ICML, 2024
4. **Zachary Robertson**, Hannah Cha, Andrew Sheha, and Sanmi Koyejo. [Implementability of Information Elicitation Mechanisms with Pre-Trained Language Models](https://openreview.net/pdf?id=QqMnRGlRJk). ICML 2024 Workshop on Theoretical Foundations of Foundation Models
5. **Zachary Robertson** and Sanmi Koyejo. [No Bidding, No Regret: Pairwise-Feedback Mechanisms for Digital Goods and Data Auctions](https://arxiv.org/abs/2306.01860). ISIT Workshop on Information-Theoretic Methods for Trustworthy Machine Learning, 2024
6. Boxiang Lyu, Zhe Feng, **Zachary Robertson**, and Sanmi Koyejo. [Pairwise Ranking Losses of Click-Through Rates Prediction for Welfare Maximization in Ad Auctions](https://arxiv.org/abs/2306.01799). ICML, 2023
7. **Zachary Robertson**, Hantao Zhang, and Sanmi Koyejo. [Cooperative Inverse Decision Theory for Uncertain Preferences](https://proceedings.mlr.press/v206/robertson23a.html). AISTATS, 2023
8. **Zachary Robertson**. [GPT4 is Slightly Helpful for Peer-Review Assistance: A Pilot Study](https://arxiv.org/abs/2307.05492). arXiv preprint arXiv:2307.05492, 2023
9. **Zachary Robertson**, Hantao Zhang, and Sanmi Koyejo. [Probabilistic Performance Metric Elicitation](https://www.ideals.illinois.edu/items/124609). 1st Workshop on Human and Machine Decisions (WHMD 2021) at NeurIPS 2021, 2022
10. **Zachary Robertson** and Matthew Walter. [Concurrent Training Improves the Performance of Behavioral Cloning from Observation](https://arxiv.org/abs/2008.01205). arXiv preprint arXiv:2008.01205, 2020
