---
layout: page
title: Medical PDF Extraction
description: A local pipeline that turns lab reports and prescriptions into structured fields, with a person checking every value
importance: 2
category: llm apps
github: https://github.com/Karthwox/medical-pdf-extraction
---

Lab reports and prescriptions arrive as PDFs: tables, abbreviations and free text, laid out differently by every lab and clinic. This pipeline turns them into structured fields, such as test results with units and reference ranges, medicines with dosage and frequency, and patient details, so the data can be stored and compared.

**[View on GitHub →](https://github.com/Karthwox/medical-pdf-extraction)**

## How it works

1. **Read the PDF.** PyMuPDF and pdfplumber pull the text and tables from each page. Scanned pages go to OCR.
2. **Split it into sections.** Simple rules sort each block into patient details, test results, medications or past results. Blocks the rules can't place go to the model.
3. **Extract the fields.** A local model, run through Ollama, turns each section into JSON. A small vocabulary of test and drug names helps it match spellings ("Hb" and "Haemoglobin"), and Pydantic checks every result against a schema.
4. **Review.** A local page shows the PDF beside the extracted fields. Every value has to be confirmed, edited or rejected; nothing is accepted automatically, and each decision is logged.

Everything runs on one machine. No document or extracted value is sent anywhere.

## Hardening it with my own skill

Running locally doesn't make it safe, so I checked it with my [security skill for coding agents]({{ '/projects/2_security_skill/' | relative_url }}). It found real problems:

- **Any website open in the browser could read the extracted medical data** from the local server. The server now refuses requests from other sites and from unexpected hostnames.
- **A crafted PDF could run code in the review page,** because extracted text went into the page as raw HTML. In testing, it did. Every value is now escaped, and the same attack shows up as plain text.
- **Uploads weren't checked:** any file, any size, saved under the name the browser sent. Now only real PDFs up to 25 MB are accepted, under names the server chooses.
- **Smaller fixes:**
  - a review can only change its own document
  - error messages no longer expose internals
  - each OCR page gets its own temporary folder
  - the review page loads nothing from outside servers

The original code passed 1 of 6 of my attack checks; the hardened version passes all 6, plus 2 browser checks.

_Built in 2026 as a personal project._
