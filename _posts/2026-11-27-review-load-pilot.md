---
layout: post
title: Does reviewer load make code review shallower?
date: 2026-11-27 16:00:00+0530
description: A pilot on 45,000 code reviews across 36 repos with AI-agent pull requests. Heavier load comes with slightly lighter reviews, but I found no sign it lets more bugs through.
tags: scrapped code-review ai-agents
related_posts: false
toc:
  sidebar: left
---

<blockquote class="block-tip" markdown="1">
**Verdict: scrapped.** When reviewers have more on their plate, their reviews get slightly lighter: about 2 percentage points more bare approvals per unit of log load. But I found no sign that this lets more bugs or reverts through. The effect is real but too small to build a study on.
</blockquote>

## The idea

Coding agents can open pull requests much faster than people can review them. Two old ideas suggest this should hurt review quality:

- **Queueing.** When work arrives faster than it's served, the queue grows. A reviewer facing a long queue has a reason to clear it quickly.
- **Vigilance.** In long monitoring tasks, people miss more as time goes on. A reviewer's tenth PR in a row may get less attention than their first.

That gave two hypotheses:

- **H1:** higher reviewer load leads to shallower reviews.
- **H2:** shallower reviews let more defects through.

The story only matters if both hold. Shallower reviews that don't change outcomes are just efficiency.

## Pilot setup

**Data.** I started from [AIDev](https://huggingface.co/datasets/hao-li/AIDev), a dataset of pull requests opened by coding agents on GitHub. Its curated subset has 361,296 agent PRs across 25,403 repos with more than 100 stars. I kept repos with at least 20 agent PRs, at least two human reviewers, at least half their PRs reviewed by a human, and at least 180 days of activity. That left **494 repos and 50,428 agent PRs**. The agent mix was mostly Claude Code (44%) and Copilot (39%), followed by Devin and Codex (6% each) and Cursor (5%).

AIDev only covers agent PRs, but a reviewer's load includes every PR they review. So for a slice of **36 repos** I pulled the full PR and review history from the GitHub API for June 2025 to March 2026: 44,962 PRs and 141,848 reviews. After keeping only each reviewer's first review on a PR, the analysis sample was **45,399 human reviews by 1,946 reviewers**. 10.5% of these were on agent PRs.

**Measuring load.** I used three measures:

- **position in a sitting:** a new sitting starts after a 30-minute gap between reviews
- **open review requests** waiting for the reviewer at the moment they submit a review
- **reviews submitted in the previous 24 hours**

**Measuring depth.** A review counts as a **bare approval** if it approves with no inline comments and an empty body. I also counted **inline comments** per review.

**Comparisons.** Each reviewer is compared only with themselves (reviewer fixed effects), controlling for PR size, whether an agent wrote the PR, and whether the PR is trivial (docs, chore, version bumps, typos, or 10 lines or fewer). I dropped reviews where the person who assigned the task to the agent then reviewed its PR, since that's closer to self-review. The ranges in brackets are 95% intervals. To get them, I resampled whole reviewers (whole repos, for the revert analysis) rather than individual reviews, since the same person's reviews tend to look alike.

**Measuring outcomes.** I used two outcomes:

- **Reverts:** 28,383 merged, human-reviewed PRs, of which 176 (0.62%) were later reverted.
- **Later bug fixes (SZZ):** in 10 of the repos, I traced which merged PRs had lines changed by a later bug-fix commit within 180 days. That covered 9,973 PRs, 13.9% of them.

## What I saw

### 1. The first, dramatic result was an artifact

In the AIDev-only data, reviews late in a long sitting looked much shallower: **87.5% bare approvals at position 9 or later, against 51.9% earlier**. That looked like vigilance decline.

It wasn't. 229 of the 287 late reviews came from a single repo, and only 20 reviewers ever reached position 9. Compared within reviewer and with controls, the difference was +11.6 pp, with an interval of −5.8 to +16.7 that crosses zero.

With the full histories for the 36 repos, the effect **flipped sign**: late reviews were _less_ likely to be bare approvals (−6.8 pp, [−12.2, −0.9]). Even that disappeared once I dropped one reviewer who accounted for a third of all late reviews (−6.7 pp, [−13.6, +2.1]). Long sittings are rare: only 176 reviews in the slice came at position 9 or later. In GitHub data, position in a sitting isn't a usable load measure.

### 2. Queue and daily load show a small, consistent effect

The other two load measures behaved much better:

- Per unit of log(1 + open requests), bare approvals rose **2.1 pp** [1.0, 3.2] and inline comments fell by **0.07** per review [0.03, 0.12].
- Per unit of log(1 + reviews in the past 24 h), bare approvals rose **2.0 pp** [0.7, 3.1] and inline comments fell by a similar amount.
- Both held on non-trivial PRs alone (+2.2 pp bare approvals, −0.09 inline comments).

For scale, about 58% of first reviews in the slice were bare approvals. Going from an empty queue to 8 or more open requests would add roughly 4–5 points. H1 holds, but weakly.

**A side finding on agent PRs:** with the same controls, reviewers approved agent PRs bare about as often as human PRs (+0.9 pp, [−0.9, +2.8]), but left fewer inline comments on them (−0.13, [−0.19, −0.06]).

### 3. No sign of harm downstream

This is where the idea stopped. Neither load nor review depth predicted later problems:

<figure class="viz-root" style="margin:1.5rem 0">
<style>
.viz-root { --surface-1:#ffffff; --text-primary:#191919; --text-secondary:#666666; --series-1:#257de0; --muted:#8c8c8c; --rule:#e6e6e6; }
:root[data-theme="dark"] .viz-root { --surface-1:#191919; --text-primary:#efefef; --text-secondary:#9c9c9c; --series-1:#338ced; --muted:#7a7a7a; --rule:#333333; }
.viz-root svg { width:100%; height:auto; font-family:inherit; }
.viz-root .hdr { fill:var(--text-primary); font-size:13px; font-weight:600; }
.viz-root .lbl, .viz-root .val { fill:var(--text-secondary); font-size:12px; }
.viz-root .tick, .viz-root .axt { fill:var(--text-secondary); font-size:11px; text-anchor:middle; }
.viz-root .grid { stroke:var(--rule); stroke-width:1; }
.viz-root .zero { stroke:var(--text-secondary); stroke-width:1; stroke-dasharray:3 3; }
.viz-root .hit { fill:transparent; }
.viz-root .row:hover .hit { fill:var(--rule); fill-opacity:.45; }
.viz-root .ci { stroke-width:2; stroke-linecap:round; }
.viz-root .sig .ci { stroke:var(--series-1); } .viz-root .sig .pt { fill:var(--series-1); stroke:var(--surface-1); stroke-width:2; }
.viz-root .ns .ci { stroke:var(--muted); } .viz-root .ns .pt { fill:var(--surface-1); stroke:var(--muted); stroke-width:2; }
.viz-root figcaption { color:var(--text-secondary); font-size:.85rem; margin-top:.5rem; }
</style>
<svg viewBox="0 0 760 302" role="img" aria-label="Effect of reviewer load on review depth and on later reverts and bug fixes, with 95% intervals">
<line x1="200.0" x2="200.0" y1="22" y2="262" class="grid"/><text x="200.0" y="278" class="tick">-3</text><line x1="251.4" x2="251.4" y1="22" y2="262" class="grid"/><text x="251.4" y="278" class="tick">-2</text><line x1="302.9" x2="302.9" y1="22" y2="262" class="grid"/><text x="302.9" y="278" class="tick">-1</text><line x1="354.3" x2="354.3" y1="22" y2="262" class="zero"/><text x="354.3" y="278" class="tick">0</text><line x1="405.7" x2="405.7" y1="22" y2="262" class="grid"/><text x="405.7" y="278" class="tick">+1</text><line x1="457.1" x2="457.1" y1="22" y2="262" class="grid"/><text x="457.1" y="278" class="tick">+2</text><line x1="508.6" x2="508.6" y1="22" y2="262" class="grid"/><text x="508.6" y="278" class="tick">+3</text><line x1="560.0" x2="560.0" y1="22" y2="262" class="grid"/><text x="560.0" y="278" class="tick">+4</text>
<text x="8" y="30" class="hdr">Review depth: share of bare approvals</text><g class="row sig"><title>Open review requests: +2.1 pp [+1.0, +3.2]</title><rect x="0" y="46" width="760" height="24" class="hit"/><text x="20" y="62" class="lbl">Open review requests</text><line x1="407.8" x2="520.9" y1="58" y2="58" class="ci"/><circle cx="463.3" cy="58" r="5" class="pt"/><text x="572" y="62" class="val">+2.1 pp [+1.0, +3.2]</text></g><g class="row sig"><title>Reviews in the past 24 h: +2.0 pp [+0.7, +3.1]</title><rect x="0" y="72" width="760" height="24" class="hit"/><text x="20" y="88" class="lbl">Reviews in the past 24 h</text><line x1="392.3" x2="511.7" y1="84" y2="84" class="ci"/><circle cx="456.6" cy="84" r="5" class="pt"/><text x="572" y="88" class="val">+2.0 pp [+0.7, +3.1]</text></g><text x="8" y="110" class="hdr">Later revert of the PR</text><g class="row ns"><title>Open review requests: +0.00 pp [-0.14, +0.13]</title><rect x="0" y="126" width="760" height="24" class="hit"/><text x="20" y="142" class="lbl">Open review requests</text><line x1="347.2" x2="360.7" y1="138" y2="138" class="ci"/><circle cx="354.4" cy="138" r="5" class="pt"/><text x="572" y="142" class="val">+0.00 pp [-0.14, +0.13]</text></g><g class="row ns"><title>Reviews in the past 24 h: -0.06 pp [-0.25, +0.14]</title><rect x="0" y="152" width="760" height="24" class="hit"/><text x="20" y="168" class="lbl">Reviews in the past 24 h</text><line x1="341.4" x2="361.2" y1="164" y2="164" class="ci"/><circle cx="351.4" cy="164" r="5" class="pt"/><text x="572" y="168" class="val">-0.06 pp [-0.25, +0.14]</text></g><text x="8" y="190" class="hdr">Later bug fix touched the PR's lines (SZZ, 180 days)</text><g class="row ns"><title>Open review requests: -0.9 pp [-2.1, +0.4]</title><rect x="0" y="206" width="760" height="24" class="hit"/><text x="20" y="222" class="lbl">Open review requests</text><line x1="246.3" x2="376.4" y1="218" y2="218" class="ci"/><circle cx="310.6" cy="218" r="5" class="pt"/><text x="572" y="222" class="val">-0.9 pp [-2.1, +0.4]</text></g><g class="row ns"><title>Reviews in the past 24 h: -0.5 pp [-1.6, +0.7]</title><rect x="0" y="232" width="760" height="24" class="hit"/><text x="20" y="248" class="lbl">Reviews in the past 24 h</text><line x1="272.0" x2="388.7" y1="244" y2="244" class="ci"/><circle cx="326.5" cy="244" r="5" class="pt"/><text x="572" y="248" class="val">-0.5 pp [-1.6, +0.7]</text></g>
<text x="380" y="296" class="axt">Change in percentage points per unit of log(1 + load)</text>
</svg>
<figcaption>Estimates with 95% bootstrap intervals, each reviewer compared only with themselves (depth) or within repository (reverts, bug fixes), controlling for PR size, agent authorship and trivial PRs. Filled blue dots: the interval excludes zero. Hollow grey dots: it doesn't. A unit of log(1 + load) is roughly going from 0 to 2 open requests. Hover a row for its numbers.</figcaption>
</figure>

- **Reverts:** load had no detectable effect (open requests: +0.00 pp, [−0.14, +0.13]), and neither did a bare approval (−0.20 pp, [−0.52, +0.17]). Reverts are rare, so this test has little power.
- **Later bug fixes:** this outcome has more events and gave the same answer. Open requests: −0.9 pp, [−2.1, +0.4]. Bare approval: +0.3 pp, [−3.0, +3.0].
- **One result that looks backwards:** PRs with _more_ inline comments were more likely to need a later bug fix (+7.6 pp per unit of log comments, [4.5, 10.4]). That's almost certainly reverse causation: reviewers comment more on PRs that already look risky. It's a reminder not to read depth measures as causes of quality.

## Why I stopped

The idea needed both links in the chain. The first link (load leads to lighter reviews) is there, but small. The second link (lighter reviews let more problems through) didn't show up with either outcome measure. Turning this into a convincing study would need a causal design, such as natural experiments in how reviewers get assigned, plus better quality measures. That's weeks of work chasing an effect that, on this evidence, may not matter.

A practical limit also showed up early: only **19.5%** of reviews had a nearby earlier review that allowed even a rough estimate of time spent, and only 3.4% of sittings had five or more reviews. The intense, back-to-back reviewing the vigilance story is about barely shows up in public GitHub data.

## What would make it worth reopening

- **Repos under real review pressure,** where one or two maintainers handle most agent PRs, or the same repos again once agent PR volume has grown.
- **Exogenous changes in load,** such as release crunches, holidays or a reviewer's absence, which would allow a causal estimate instead of a correlation.
- **A sharper quality outcome** than reverts or SZZ, for example security-relevant defects or review findings that reach production.
- **A controlled experiment** where reviewers face a set queue of seeded PRs. I set this aside for feasibility, but it would answer H1 and H2 cleanly.

## Lessons for anyone trying something similar

- **Check whether a single repo or reviewer drives the result** before believing it. My most striking number came from one repo.
- **Use queue-based load measures rather than position in a sitting.** Long sittings are too rare in GitHub data to support conclusions.
- **Reverts are too rare to detect small effects.** SZZ gives you more events, but it's noisy.
- **Don't treat review depth as a cause of quality.** Reviewers adapt their depth to how risky a PR looks.

<!-- TODO(Karthik): link the pilot code/data repo here once it's public. -->
