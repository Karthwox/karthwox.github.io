---
layout: page
title: Medical Summary Generator
description: An LLM app that explains a patient's own medical records in plain language, including what their test numbers mean
importance: 1
category: llm apps
github: https://github.com/Karthwox/medical-summary-generator
---

Medical records are written for doctors. A patient leaves with a stack of prescriptions and lab printouts full of numbers and abbreviations, and no clear idea what any of it means. This app turns those records into a summary the patient can actually understand.

**[View on GitHub →](https://github.com/Karthwox/medical-summary-generator)**

## What it does

You enter your symptoms, medicines and lab results, or upload report PDFs. The app reads the PDFs in the browser, sends everything to an LLM, and builds a plain-language report:

- **one sentence** on what happened
- **your medicines:** what each one does and why it was likely prescribed
- **your test results:** each number marked high, normal or low, with what it means for you
- **questions to ask your doctor**
- **warning signs** that mean you should get help promptly

The report can be printed or saved as a PDF. It explains records; it doesn't replace a doctor.

## How it's built

- **Front end:** React, Vite and Tailwind, with light and dark themes. Everything runs in the browser; there's no server.
- **Models:** LLMs served through Groq, with automatic fallback to a second model if the first fails.
- **Files:** pdf.js extracts report text in the browser.
- **Structured output:** the model returns JSON in a fixed schema, which the app validates and renders into the report.
- **Demo mode:** type `mock` as the API key to see a sample report without one.

## Hardening it with my own skill

When I revisited this project, I ran it through my [security skill for coding agents]({{ '/projects/2_security_skill/' | relative_url }}). It found real problems:

- **The model's HTML was inserted straight into the page,** so a malicious response could run code. In testing, a crafted response did exactly that. Output is now sanitized and checked against the expected shape, and the same attack is neutralised.
- **A file removed from the upload list was still sent to the AI,** a privacy bug for medical documents. Fixed.
- **The API key setup would have baked a key into the public JavaScript,** and `.env` wasn't git-ignored. Users now bring their own key, which is never saved.
- **Smaller fixes:**
  - uploads checked by their actual content
  - caps on file size, pages and request length
  - a Content Security Policy
  - no patient data in the browser console
  - updated dependencies and models

_Built in 2025 as a personal project._
