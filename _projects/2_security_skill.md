---
layout: page
title: A security skill for AI coding agents
description: A drop-in skill that makes coding agents apply security rules while they build web apps
importance: 1
category: agent tooling
---

AI coding agents can build a working web app from a few sentences, but they routinely leave out protections nobody asked for: rate limits on paid API calls, server-side checks on who can read what, safe file uploads. This skill gives the agent a short set of security rules to follow _while_ it writes the code, so those protections are built in from the start instead of patched on later.

## How it works

The skill is a small router. `SKILL.md` holds the rules that apply everywhere and points the agent to one reference file for whatever it's currently building, so it only reads what the task needs:

- **Always-on rules:** secrets never go in source or client code, with no fallback secrets even for development; authorize every request on the server; validate and bound all input; never disable a protection to clear an error.
- **Four "before you write the handler" rules** for the places generated code most often goes wrong: limits on anything that costs money or contacts people, uploads, destructive account-level actions, and server requests to user-supplied URLs.
- **Seven reference files:** access control, secrets, abuse controls, authentication, uploads, outbound requests, and cross-origin requests.

It also keeps a `SECURITY.md` decision record in the project, with an access matrix, a secrets inventory, an abuse-surface table, assumptions and deferrals. The skill is strict by default. Some hardening, like rate limits or MFA, can be deferred only while the app is local-only, and each deferral is written down. Secrets handling, server-side authorization, input validation and spend caps are never deferred.

Two more modes sit on top:

- **Exposure gate:** before anything makes the app reachable by others (deploying, tunnels, sharing a link, real data), the agent stops, says what will be exposed, audits the project and resolves every open deferral.
- **Audit mode:** checks an existing project against the same rules, item by item, fixes what it safely can and lists the rest with exact steps.

## What testing showed

I tested the skill in a pre-registered study with blinded code review, comparing it with no guidance, a one-line "follow security best practices" reminder, and a public security skill. The honest result: the security content is what helps, not the skill's length or structure. The reminder, a checklist and the skill all cut critical failures substantially, a length-matched placebo didn't, and the skill showed no detectable advantage over the one-line reminder while using more tokens. The full write-up is on the [research page]({{ '/publications/' | relative_url }}).
