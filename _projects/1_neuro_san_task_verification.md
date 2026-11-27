---
layout: page
title: task verification for neuro-san
description: an end-of-turn checker that verifies an agent network actually did what was asked
importance: 1
category: open source
github: https://github.com/cognizant-ai-lab/neuro-san-studio
---

**Open-source contribution to [neuro-san-studio](https://github.com/cognizant-ai-lab/neuro-san-studio)**, Cognizant AI Lab's framework for building multi-agent networks.

Multi-agent systems often finish a turn and report success without checking that the user's request was actually carried out. For [issue #1574](https://github.com/cognizant-ai-lab/neuro-san-studio/issues/1574) I proposed and built an end-of-turn **checker**: after the agents respond, it checks the result against the original task and flags turns where the work wasn't done.

The checker's verdict is also a useful signal beyond a single turn. It can feed back into adaptive routing, so the system learns which agents reliably complete which kinds of tasks.

- Tested live with Gemini models.
- Opened as [PR #1575](https://github.com/cognizant-ai-lab/neuro-san-studio/pull/1575) against the `feature/task-completion-verification` branch.
