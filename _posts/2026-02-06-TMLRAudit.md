---
layout: post
title: "How Long Do TMLR Decisions Actually Take? An OpenReview Audit"
published: true
---

*What 4,865 submissions reveal about TMLR timelines in practice*

### Background

[TMLR](https://jmlr.org/tmlr/) (Transactions on Machine Learning Research) launched in 2022 as an alternative to the conference review cycle. No fixed deadlines, no accept/reject quotas, public reviews on OpenReview. One of its explicit selling points is speed: rolling submissions mean authors don't wait months for a synchronous review round. The timeline commitments below are central to that pitch.

### The Stated Timeline

TMLR tells authors when to expect decisions. Their <a href="https://jmlr.org/tmlr/ae-guide.html" target="_blank">Action Editor guide</a> states:

> Based on their discussion with the authors, reviewers will submit their own recommendation to you on a decision for a submission no sooner than 2 weeks and no later than 4 weeks after the submission of the third review.
>
> Once all 3 reviewer decision recommendations have been submitted, you should aim to submit your decision proposal within 1 week. Your decision proposal should therefore be submitted within 5 weeks of the beginning of the discussion between authors and reviewers.

Authors receive this timeline directly via email once three reviews are posted:

> You will have 2 weeks to respond to the reviewers. To maximise the period of interaction and discussion, please respond as soon as possible. The reviewers will be using this time period to hear from you and gather all the information they need. In about 2 weeks, and no later than 4 weeks, reviewers will submit their formal decision recommendation to the Action Editor in charge of your submission.

So: **4 weeks** from the third review to reviewer recommendations, **5 weeks** to the AE decision. There is a caveat ("If, and only if, authors and reviewers are still discussing the submission, you may decide to take more time") framed as an exception.

In practice, authors experience the full process as "time from the third review to a final decision," since that is when planning uncertainty resolves. OpenReview does not expose when reviewers submit their internal recommendations to the Action Editor, so the final decision timestamp is the closest consistently observable endpoint of the process from an author's perspective. Even allowing roughly an additional week for Action Editor processing beyond the 4-week reviewer window, most papers still exceed the implied 5-week total.

### The Audit

I scraped all 5,844 TMLR submissions from the OpenReview API (as of February 6, 2025) and pulled timestamps for the third review and the final decision. After filtering to papers with at least three reviews and a posted decision, I had **4,865 submissions** with complete timeline data. The 3-review requirement excludes ~400 desk rejects and withdrawn papers (which never entered the review process). No paper in the dataset has more than one decision, so revision rounds do not affect the measurement.

For each paper, compute `days_from_third_review_to_decision = (t_decision - t_third_review) / 86400` and compare against the 35-day (5-week) stated window.

### Results

| Metric&nbsp; | &nbsp;Value&nbsp; |
|--------------|------------------|
| Median | &nbsp;**45.2 days**&nbsp; |
| 75th percentile | &nbsp;57.0 days&nbsp; |
| 90th percentile | &nbsp;72.5 days&nbsp; |
| 95th percentile | &nbsp;83.3 days&nbsp; |
| 99th percentile | &nbsp;116.2 days&nbsp; |
| &nbsp; | &nbsp; |
| > 28 days&nbsp;(4-week window) | &nbsp;95.7%&nbsp; |
| > 35 days&nbsp;(≈5-week total) | &nbsp;**82.5%**&nbsp; |
| > 42 days&nbsp;(6 weeks) | &nbsp;60.4%&nbsp; |


The median paper receives its decision 17 days after the 4-week reviewer-recommendation window. Only 4.3% of submissions receive decisions within 28 days of the third review.

![Distribution of TMLR decision times by week]({{ site.baseurl }}/images/tmlr_histogram.png)

These figures reflect every decided TMLR paper as of the scrape date rather than a sample, and they have remained stable for multiple years.

**Is 4 Weeks the Right Benchmark?**

One interpretation is that the 4-week language applies only to reviewer recommendations, with roughly an additional week for the Action Editor, making ~35 days the relevant target. Even under that more generous benchmark, **82.5% of papers exceed 5 weeks**, and the median is still about 10 days longer.

Another interpretation is that the "ongoing discussion" caveat frequently applies. If most papers involve continued author–reviewer interaction, then the stated window functions more as an aspiration than a reliable planning horizon. In that case, publishing typical observed timelines may better align expectations with observed practice.

**Has this improved over time?** No. The median has been stable at roughly 45–46 days since 2023, with 2022 slightly faster (41.5 days) when TMLR was newer and had fewer submissions. The gap between the documented window and observed timelines has been persistent rather than transient.

![TMLR median decision time by year]({{ site.baseurl }}/images/tmlr_yearly.png)

### Reliability of the Measurement

**Is the measurement valid?** The timestamps come from OpenReview's API: machine-generated creation dates for reviews and decisions. No human judgment, no subjective classification. The only analytical choice is which review timestamp to use (I use the third, since that's when the clock starts per TMLR's own email).

**Is the sample representative?** The 4,865 submissions cover every decided TMLR paper as of the scrape date. This is a census of decided papers rather than a sample. Of the 5,440 submissions with three or more reviews, 571 (10.5%) have no decision yet. These right-censored papers are disproportionately slow, so the true population median is likely somewhat higher than 45 days.

### What This Means

I like TMLR. Rolling submissions and public reviews on OpenReview are good ideas. The people running it are volunteers, and wrangling reviewers is thankless work. None of this is about bad faith. The goal is expectation-setting using publicly observable data.

When most papers exceed a documented timeline, that timeline functions less as a predictable bound and more as an aspiration. Authors often plan concurrent submissions, job applications, and grant deadlines around the expected post-review window, so clarity about typical timelines is useful for planning.

Two possible responses follow. One is procedural: tighter escalation or backup-reviewer mechanisms to bring outcomes closer to stated targets. The other is communicative: publish typical observed timelines (for example, median ~6.5 weeks from third review to decision, ~8 weeks at the 75th percentile) so authors can plan using realistic expectations.

TMLR already publishes reviews publicly. Publishing rolling statistics on time-to-decision would be a comparatively small step that could further align expectations between authors and editors.

### Reproducibility Statement

The analysis uses a single Python script (~60 lines) that calls the [OpenReview API](https://docs.openreview.net/) with no credentials required. For each of TMLR's 5,784 submissions, it extracts the third review timestamp and decision timestamp from public reply metadata, computes the gap in days, and filters to the 4,865 papers with at least three reviews and a posted decision. The only analytical choice is using the third review as the clock start, matching the process described in TMLR author communications. The script and raw data are available [here](https://github.com/zrobertson466920/TMLRAudit).

If this analysis is useful, please consider citing or linking to it.

```bibtex
@misc{robertson2025tmlraudit,
  title={How Long Do TMLR Decisions Actually Take? An OpenReview Audit},
  author={Robertson, Zachary},
  year={2025},
  institution={Stanford University},
  url={https://zrobertson466920.github.io/TMLRAudit/},
  note={Blog post auditing TMLR decision timelines using OpenReview API data}
}
```
