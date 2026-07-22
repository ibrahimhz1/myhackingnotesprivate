---
id: "202606120002"
title: Stored XSS into an unsanitized HTML context
date: 2026-06-12
type: case
tags: [xss, stored-xss, html-sink]
source: "[[2_Lab2-Stored XSS into HTML context with nothing encoded]]"
publish: false
---

A blog comment form has three reflected-back fields — comment, name, website — but they aren't sanitized uniformly. Testing the same `<script>alert('xss')</script>` payload against every field one at a time found that the **name** field was inserted into the HTML sink raw, while the sibling **website/username** field was HTML-encoded.

The lesson isn't the payload — it's the method: when a form has multiple fields that all get echoed back somewhere, don't assume they share one encoding path. Test each field independently, because inconsistent sanitization across fields of the same form is common (different code paths, different developers, different eras of the codebase).

Stored beats reflected in impact even with an identical payload: every visitor who views the page re-triggers it, no need to get a victim to click a crafted link.

## Links
- Same root cause as: [[202606120001 Reflected XSS into an unsanitized HTML context]] — no encoding at all on the vulnerable sink.
- Contrast: [[202607040001 Stored DOM XSS bypassing a single-pass angle-bracket filter]] — here there's no filter at all; that lab has a filter with a specific logic bug.
