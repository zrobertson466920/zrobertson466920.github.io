---
layout: page
title: About
permalink: /about/
description:  Stanford CS PhD studying AI evaluation without ground truth; tree-of-thought prompting, reasoning data work

---

I study how to evaluate AI systems when ground truth is unavailable. My current focus is [mutual evaluation](https://github.com/zrobertson466920/zrobertson466920.github.io/blob/master/Short%20Talk%20-%20Measuring%20Information%20with%20LLMs.pdf): using interactions between agents to measure reliability without labeled answers. More broadly, I'm interested in how human preferences, stated and revealed, get encoded into agents and ranking systems, and where those processes break down.

I work across theory and practice. I contributed early work on chain-of-thought prompting, a technique for eliciting step-by-step reasoning in language models. In later reporting on the technique’s early independent origins, [The Atlantic described me as a “co-inventor.”](https://www.theatlantic.com/technology/2026/04/4chan-ai-dungeon-thinking-reasoning/686794/?gift=0eg_N5wf_UNSiF4sMBbZ4StD-I2TBN1LxAzadN65nOE&utm_source=copy-link&utm_medium=social&utm_campaign=share) I continue maintaining one of the [larger human-generated tree-of-thought](https://zrobertson466920.github.io/BranchingDirective/) systems for studying reasoning and evaluation. I've also used the system to contribute to early [reasoning data work](https://hai.stanford.edu/news/exploring-complex-ethical-challenges-data-annotation) for frontier models. The through-line is my interest in how illegible human signals become legible data for training machine behavior.

### Themes

**Scalable oversight:** Information-theoretic evaluation of AI systems without ground truth. Current focus is **mutual evaluation**, using model interactions to measure reliability without labeled answers, along with theoretical foundations for robustness to adversarial manipulation ([arXiv:2508.05469](https://arxiv.org/abs/2508.05469), Aug 2025 accepted with minor revisions at TMLR, 2026). Supported by an OpenAI Superalignment Fast Grant ($500k, 2024).

**Human signal construction:** Methods and systems for eliciting, structuring, and validating human signals used to train and evaluate models. This includes early work on chain-of-thought prompting and reasoning decomposition, as well as research on annotation pipelines, red teaming, and aligning ranking systems (e.g., social media feeds) with users’ articulated values.

### News & Engagement

- **Apr 2026:** Featured in *The Atlantic*: [*The Strange Origin of AI’s ‘Reasoning’ Abilities*](https://www.theatlantic.com/technology/2026/04/4chan-ai-dungeon-thinking-reasoning/686794/?gift=0eg_N5wf_UNSiF4sMBbZ4StD-I2TBN1LxAzadN65nOE&utm_source=copy-link&utm_medium=social&utm_campaign=share).
- **Apr 2026:** Coauthor on [Value Alignment of Social Media Ranking Algorithms](https://dl.acm.org/doi/full/10.1145/3772318.3791281) (**CHI 2026**), a generalizable method for aligning social media feeds to users’ values, showing with 400+ users that value-driven rankings produce meaningfully aligned feeds using their own X/Twitter data.
- **Sept 2025:** Invited talk at *Building an Aligned AI Future* (Fifty Years @ Stanford): [event](https://luma.com/d1wxjy6s?tk=KM9dGC).
- **Aug 2025 / Apr 2026:** First-author on *Let's Measure Information Step-by-Step: LLM-Based Evaluation Beyond Vibes*, a paper on robust evaluation without ground truth: [arXiv:2508.05469](https://www.arxiv.org/abs/2508.05469). **Accepted with minor revisions at TMLR**.
- **July 2024:** Featured by Stanford HAI for *WellLabeled*, a cross-disciplinary project on ethical challenges in AI data annotation and worker protections: [article + video](https://hai.stanford.edu/news/exploring-complex-ethical-challenges-data-annotation).
- **July 2024:** Invited talks on scalable oversight including FloodGate and Max Planck Institute for Intelligent Systems.
- **July 2024:** Early production deployments related to scalable evaluation infrastructure: [LinkedIn post](https://www.linkedin.com/posts/florian-h%C3%B6nicke-b902b6aa_icml-icml24-icml2024-activity-7224064255677849601-wdgS).
- **May 2024:** Lead on OpenAI Superalignment Fast Grant ($500k).

### Experience

- **Stanford University 2022-present:** PhD student in Computer Science working on scalable oversight, AI evaluation, and alignment.
  - [EDGE fellow](https://vpge.stanford.edu/fellowships-funding/vpge-fellowships/edge-doctoral-fellowship-program) and recipient of a three-year [SGF fellowship](https://vpge.stanford.edu/fellowships-funding/sgf).
  - Instructor for [CS221 2025](https://stanford-cs221.github.io/spring2025/) and [2026](https://stanford-cs221.github.io/spring2026/): teaching principles and techniques of artificial intelligence.
- **University of Illinois Urbana-Champaign 2020-2022:** MS in Computer Science, with work on metric elicitation and evaluation systems. [GEM fellowship](https://www.gemfellowship.org/gem-fellowship-program/) recipient. 
- **University of Chicago 2016-2020:** BS in Computational & Applied Mathematics. [Jackie Robinson scholarship](https://jackierobinson.org/scholarship/) recipient. 
- **Google 2022:** Industry experience in ads welfare optimization.
- **Lam Research 2020-2021:** Applied reinforcement learning to semiconductor process control.
- **Toyota Technological Institute 2019-2020:** Research experience in perception and language grounding.
- **Stackexchange:** Over 500k+ readers on math and stackexchange have been helped by my questions and answers. [Profile.](https://stackexchange.com/users/5852690/zach466920?tab=top)

### Contact

[zroberts@stanford.edu](mailto:zroberts@stanford.edu)

[CV](https://github.com/zrobertson466920/zrobertson466920.github.io/blob/master/Awesome_CV-3.pdf) · [Google Scholar](https://scholar.google.com/citations?user=769PIisAAAAJ&hl=en) · [GitHub](https://github.com/zrobertson466920) · [X/Twitter](https://x.com/zwrobertson) · [LinkedIn](https://www.linkedin.com/in/zrobertson466920/)

_Last updated: April 2026._

### Selected Publications

1. Farnaz Jahanbakhsh, Dora Zhao, Tiziano Piccardi, **Zachary Robertson**, Ziv Epstein, Sanmi Koyejo, and Michael S. Bernstein. [Value Alignment of Social Media Ranking Algorithms](https://dl.acm.org/doi/full/10.1145/3772318.3791281). ACM CHI, 2026. 
2. **Zachary Robertson** and Sanmi Koyejo. [Let's Measure Information Step-by-Step: LLM-Based Evaluation Beyond Vibes](https://arxiv.org/abs/2508.05469). arXiv:2508.05469, 2025. Accepted with minor revisions at TMLR, 2026.
3. Olawale Salaudeen, Anka Reuel, Ahmed Ahmed, Suhana Bedi, **Zachary Robertson**, Sudharsan Sundar, Ben Domingue, Angelina Wang, and Sanmi Koyejo. [Measurement to Meaning: A Validity-Centered Framework for AI Evaluation](https://arxiv.org/abs/2505.10573). arXiv:2505.10573, 2025
4. **Zachary Robertson** and Sanmi Koyejo. [Implicit Regularization in Feedback Alignment Learning Mechanisms for Neural Networks](https://proceedings.mlr.press/v235/robertson24b.html). ICML, 2024
5. **Zachary Robertson**, Hannah Cha, Andrew Sheha, and Sanmi Koyejo. [Implementability of Information Elicitation Mechanisms with Pre-Trained Language Models](https://openreview.net/pdf?id=QqMnRGlRJk). ICML 2024 Workshop on Theoretical Foundations of Foundation Models
6. **Zachary Robertson** and Sanmi Koyejo. [No Bidding, No Regret: Pairwise-Feedback Mechanisms for Digital Goods and Data Auctions](https://arxiv.org/abs/2306.01860). ISIT Workshop on Information-Theoretic Methods for Trustworthy Machine Learning, 2024
7. Boxiang Lyu, Zhe Feng, **Zachary Robertson**, and Sanmi Koyejo. [Pairwise Ranking Losses of Click-Through Rates Prediction for Welfare Maximization in Ad Auctions](https://arxiv.org/abs/2306.01799). ICML, 2023
8. **Zachary Robertson**, Hantao Zhang, and Sanmi Koyejo. [Cooperative Inverse Decision Theory for Uncertain Preferences](https://proceedings.mlr.press/v206/robertson23a.html). AISTATS, 2023
9. **Zachary Robertson**. [GPT4 is Slightly Helpful for Peer-Review Assistance: A Pilot Study](https://arxiv.org/abs/2307.05492). arXiv preprint arXiv:2307.05492, 2023
10. **Zachary Robertson**, Hantao Zhang, and Sanmi Koyejo. [Probabilistic Performance Metric Elicitation](https://www.ideals.illinois.edu/items/124609). 1st Workshop on Human and Machine Decisions (WHMD 2021) at NeurIPS 2021, 2022
11. **Zachary Robertson** and Matthew Walter. [Concurrent Training Improves the Performance of Behavioral Cloning from Observation](https://arxiv.org/abs/2008.01205). arXiv preprint arXiv:2008.01205, 2020
