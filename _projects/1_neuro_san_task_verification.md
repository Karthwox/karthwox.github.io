---
layout: page
title: Task verification for neuro-san
description: An end-of-turn checker that verifies an agent network actually did what was asked
importance: 1
category: open source
github: https://github.com/cognizant-ai-lab/neuro-san-studio
---

**Open-source contribution to [neuro-san-studio](https://github.com/cognizant-ai-lab/neuro-san-studio)**, Cognizant AI Lab's framework for building multi-agent networks.

Multi-agent systems often finish a turn and report success without checking that the user's request was actually carried out. For [issue #1574](https://github.com/cognizant-ai-lab/neuro-san-studio/issues/1574) I proposed and built an end-of-turn **checker**: after the agents respond, it checks the result against the original task and flags turns where the work wasn't done.

The checker's verdict could also be useful beyond a single turn. One idea I had is to feed it back into adaptive routing, so the system learns which agents reliably complete which kinds of tasks. That part is only an idea for now: it isn't implemented in the PR.

- Tested live with Gemini models.
- Opened as [PR #1575](https://github.com/cognizant-ai-lab/neuro-san-studio/pull/1575) against the `feature/task-completion-verification` branch.
