---
id: "202606200001"
title: DOM XSS via a jQuery href sink using a javascript URI
date: 2026-06-20
type: case
tags: [xss, dom-xss, jquery, javascript-uri]
source: "[[5_Lab5-DOM XSS in jQuery anchor href attribute sink using location.search source]]"
publish: false
---

Source: `returnPath` URL parameter. Sink: jQuery sets `$('#backLink').attr('href', returnPath)` — the **entire** `href` value is attacker-controlled (not appended to an existing value).

First move: test whether the attribute can be broken out of at all.

```
test"'><xss-marker
```

Both `<` and `>` came back HTML-encoded — no breakout possible. But because the whole `href` value (not just a fragment of it) is under attacker control, the attribute-breakout question is moot: the *scheme* of the URL itself can be weaponized instead.

```
javascript:alert('XSS')
```

Set as `returnPath`, this becomes `<a href="javascript:alert('XSS')">Back</a>` — clicking it runs the payload. No HTML injection needed at all, just control of the URI value.

## Links
- Why this scheme trick works and how to defeat filters on it: [[202606200002 javascript URI as an XSS vector when href is fully controlled]], [[202606200003 Bypassing javascript colon string filters]]
- Same fully-controlled-href pattern, different context (stored, not DOM): [[202606240001 Stored XSS via an anchor href fully replaced with a javascript URI]]
- Decision tree for attribute sinks in general: [[202606200008 Reflected XSS by breaking out of an HTML attribute value]]
