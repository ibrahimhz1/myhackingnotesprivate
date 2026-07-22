---
id: "202606120001"
title: Reflected XSS into an unsanitized HTML context
date: 2026-06-12
type: case
tags: [xss, reflected-xss, html-sink]
source: "[[1_Lab1-Reflected XSS into HTML context with nothing encoded]]"
publish: false
---

A search term reflected verbatim into the page (`<h1>` sink) with no HTML-encoding at all. The baseline case: if a sink does zero encoding, a raw `<script>` tag executes.

```html
<script>alert('XSS exec')</script>
```

No filter, no breakout needed — the value lands directly in an HTML body context. This is the control case against which every other lab in this set adds one more layer of defense (encoding, escaping, CSP, allowlisting) that has to be defeated in turn.

## Links
- Contrast: [[202606120002 Stored XSS into an unsanitized HTML context]] — same lack of encoding, but persisted rather than reflected.
- Contrast: [[202606200008 Reflected XSS by breaking out of an HTML attribute value]] — the sink here is element *content*; that one is an attribute.
