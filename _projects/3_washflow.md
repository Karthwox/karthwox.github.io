---
layout: page
title: WashFlow
description: A campus laundry-management platform for students and laundry staff, hardened with my own security skill
importance: 1
category: web apps
github: https://github.com/Karthwox/washflow
---

Laundry at a college hostel runs on paper tokens and guesswork: students don't know how many free washes they have left or when their clothes will be ready, and staff track it all by hand. WashFlow is a working prototype built to fix that at IIIT Bangalore, where it's planned for a pilot.

**[View on GitHub →](https://github.com/Karthwox/washflow)**

## What it does

- **Student portal:** free-wash quota for the semester, bills, live position in the laundry queue, job history, and feedback on the service.
- **Operator portal:** log walk-in drop-offs, mark jobs ready and collected, look up students, reset quotas and read feedback.
- **Server-side billing:** free washes, extra-wash charges, drying and ironing costs are all computed on the server, never trusted from the browser.

It's a React and Vite front end on an Express-style Node API, built with Google's Antigravity agent.

## Hardening it with my own skill

The first version worked but wasn't safe to put in front of students. A roll number alone logged anyone in as that student, the staff passkey was `admin`, and malformed requests could reach the server logic unchecked. I hardened it with my [security skill for coding agents]({{ '/projects/2_security_skill/' | relative_url }}), which meant following its rules, writing the decision record it asks for (`SECURITY.md`) and proving the fixes with tests that try to break in:

- real passwords, hashed with scrypt, and a staff key that lives only in the server's environment
- sessions that expire, plus rate limits against password guessing
- strict validation of every input
- a write queue, which fixed a real bug: twenty simultaneous washes for one student used to leave 11 free washes on the account and charge nothing
- job statuses that can only move forward, and a database reset that's off by default and needs the key re-entered

All 28 attack tests now pass, against 10 before. The skill's decision record also lists what's still needed before real students use it: college sign-in to verify who registers, individual staff accounts, and a real database.
