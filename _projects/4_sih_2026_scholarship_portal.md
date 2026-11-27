---
layout: page
title: Scholarship Portal for Scheduled Tribes
description: A Smart India Hackathon 2026 team submission for managing tribal scholarships, from application to disbursement
importance: 2
category: web apps
github: https://github.com/MohitCRM/SIH-2026
---

A team submission to **Smart India Hackathon 2026**, built in person by a team of three. Results are not out yet.

**Problem statement 26239:** AI-Enabled Scholarship and Fellowship Management System for Scheduled Tribes, set by the Ministry of Tribal Affairs under the Smart Education theme.

**[View on GitHub →](https://github.com/MohitCRM/SIH-2026)**

## The problem

The Ministry runs scholarship and fellowship schemes for Scheduled Tribe students, such as the National Fellowship for Scheduled Tribes and the National Overseas Scholarship. Each application passes through many stages: registration, document submission, eligibility checks, scrutiny, selection and payment. The brief asked for one system that handles all of it, with AI where it helps.

## What we built

A portal with a separate view for each of the three people involved in an application.

- **Students** apply through a four-step form: identity and bank details, eligibility certificates, then course and scheme details, which change with the scheme. Last comes a review and declaration. Applications can be saved as drafts, and are locked once submitted.
- **Verifying officers** see each application's documents side by side with the extracted details. They either send it back to the student for corrections, or approve it and pass it on to the Ministry.
- **The Ministry** creates and manages schemes, sees a merit list of approved applications ranked by score, approves candidates in bulk, and generates sanction letters. Each letter has a QR code that anyone can scan to check it's genuine. Fund disbursement is simulated.

The whole portal works in **English and Hindi**, and remembers the chosen language.

The repo also has two standalone Python services for the AI side:

- a **document reader** that turns marksheets, income certificates and similar PDFs into structured fields, keeping the page and text each value came from
- an **eligibility checker** that tests an application against the scheme's rules and returns pass, fail or needs-review for each rule, with the reason

## Built with

React (Vite), Tailwind CSS and DaisyUI, with react-i18next for languages. Node.js, Express and MongoDB on the back end. Python and FastAPI for the document reader and eligibility checker, with Sarvam AI for document extraction.
