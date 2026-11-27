---
layout: page
title: A better-vibecoding skill for coding agents
description: One agent skill that tackles the common failure modes of vibe-coded apps, from bland UI to security holes
importance: 1
category: agent tooling
---

AI coding agents can build a working app from a few sentences, but "vibe-coded" apps keep failing in the same ways. This project is a single agent skill, for Claude Code, Codex and Antigravity, that targets six of them:

1. repetitive, bland UI
2. generic or stock imagery
3. missing UX states (loading, empty, error)
4. security flaws
5. secret handling
6. weak testing

**How it's built.** I tackle one problem at a time: find its root cause, write a focused reference file, then test it before moving on. The design group builds on the open-source [Impeccable](https://github.com/pbakaus/impeccable) design skill instead of reinventing it. The security group is organised by vulnerability cluster (access control, secrets, abuse controls, authentication, and so on), built from a 69-item list of the ways vibe-coded web apps go wrong. It's strict by default, with a register for deliberately deferred protections, and has two modes: a preventive mode used while building, and an audit mode that checks the finished prototype.

**Evaluation.** Each cluster is tested with scripted agent runs that compare no instruction, a one-line "follow security best practices" reminder, and the skill. That evaluation became my paper on skills versus reminders. See [research]({{ '/publications/' | relative_url }}).
