---
id: "202606200008"
title: Reflected XSS by breaking out of an HTML attribute value
date: 2026-06-20
type: case
tags: [xss, reflected-xss, attribute-breakout]
source: "[[7_Lab7-Reflected XSS into attribute with angle brackets HTML-encoded]]"
publish: false
---

Search term reflected into `value="..."` of an `<input>` tag. `<` and `>` are HTML-encoded, but `'` and `"` are not — so the attribute itself can be closed even though new *tags* can't be opened.

```html
<!-- original -->
<input type=text name=search value="some">

<!-- payload: xyz" autofocus onfocus="alert('XSS')  -->
<input type=text name=search value="some" autofocus onfocus="alert('XSS')">
```

The technique: close the existing attribute with `"`, add new attribute(s) in the open space before the tag's own closing `>`, and use `autofocus` + `onfocus` specifically because it fires with **zero user interaction** — the moment the page renders. This is the general fallback whenever `<`/`>` are encoded but quotes are not: you can't inject a new element, but you can inject new *attributes* onto the existing one.

## Links
- General decision tree for attribute-context sinks (break out vs. control the value): [[202606200002 javascript URI as an XSS vector when href is fully controlled]]
- Same "add an attribute, not a tag" idea reused against a JSON-escaped href in: [[202607050005 Stored XSS defeating escaping via HTML-entity-encoded quote smuggling]]
