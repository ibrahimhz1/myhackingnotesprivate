---
id: "202607050006"
title: HTML entities decode before use in an inline event handler's JS context
date: 2026-07-05
type: concept
tags: [xss, html-entities, browser-behaviour]
publish: false
---

Inline event handler attributes (`onclick="..."`, `onload="..."`, etc.) are HTML attribute values first and JavaScript source second. The browser decodes any HTML entities in the attribute value **before** handing the resulting text to the JS engine as a script to run:

```
Attribute text:   onclick="tracker.track('site&apos;abc')"
Browser decodes:  tracker.track('site'abc')     ← ' apostrophe now real, before JS parses it
```

If an app's string-escaping logic only pattern-matches for a literal `'` character in the *input* and backslash-escapes it, it will never notice an already-HTML-entity-encoded apostrophe (`&apos;`) in that same input — the app sees `&apos;`, not `'`, and has nothing to escape. But the browser decodes `&apos;` back to a real `'` when parsing the attribute, and by then the app's escaping pass is long done. The quote arrives at the JS engine unescaped despite the app's filter believing it handled every quote.

The general lesson: **HTML-attribute decoding and JavaScript-string escaping are two separate pipeline stages that don't automatically know about each other** — a value can be "safe" from one stage's point of view and dangerous once the next stage processes it.

## Links
- Applied in: [[202607050005 Stored XSS defeating escaping via HTML-entity-encoded quote smuggling]]
- Same layering-mismatch family: [[202607010003 Backslash escape-neutralization bypass]], [[202607090003 javascript URIs are URL-decoded after HTML parsing before JS execution]]
