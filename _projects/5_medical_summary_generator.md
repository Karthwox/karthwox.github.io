---
layout: page
title: Medical Summary Generator
description: An LLM app that turns a patient's notes, prescriptions, lab results and scanned reports into one structured medical summary
importance: 1
category: llm apps
---

A patient's medical history is usually scattered across handwritten notes, prescriptions, lab printouts and scanned reports. This app pulls it all into one structured summary that a reader can take in at a glance.

## What it does

You enter the basics (symptoms and timeline, medications, lab results, history) and drop in PDFs or images of reports. The app extracts the text from the PDFs in the browser, sends everything to an LLM, and turns the response into a structured report:

- **a one-line summary** of the case
- **presenting illness and history**, as a short narrative
- **treatment analysis:** each medication with its dose, its role and why it was likely prescribed
- **lab interpretation:** each result marked high, normal or low, with what it means
- **red flags:** symptoms to watch for

The finished report can be exported as a PDF.

## How it's built

- **Front end:** React and Vite, styled with Tailwind, with light and dark themes.
- **Models:** Llama models served through Groq. Text-only requests use a large text model; when images are attached, the app switches to a vision model. If one model fails or has been retired, it falls back to the next.
- **Files:** pdf.js extracts PDF text in the browser, and react-dropzone handles uploads.
- **Structured output:** the model is asked for JSON in a fixed schema, which the app parses and renders into the report layout.
- **Demo mode:** a built-in mock summary lets anyone try the full flow without an API key.

_A personal project. The summaries organise and explain records; they are not medical advice._
