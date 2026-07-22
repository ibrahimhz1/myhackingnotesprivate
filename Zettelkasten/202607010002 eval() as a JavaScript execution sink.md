---
id: "202607010002"
title: eval() as a JavaScript execution sink
date: 2026-07-01
type: concept
tags: [xss, eval, sink-analysis]
publish: false
---

`eval()` (and equivalents like `new Function()`, `setTimeout("string")`) is the most permissive XSS sink possible: whatever string reaches it is parsed and run as native JavaScript, with no HTML parsing and no DOM tricks required.

```
innerHTML sink  → needs HTML tag injection (<img onerror>) — see [[202606130002 innerHTML never executes script tags]]
jQuery $() sink → needs a leading '<' to switch modes — see [[202606200005 jQuery's dual personality in the dollar function]]
hashchange sink → needs a delivery mechanism — see [[202606200007 iframe onload self-trigger delivery for hash-based DOM XSS]]
eval() sink     → direct JS string injection, no tricks needed beyond breaking the string
```

Where it gets defended is upstream, in whatever escaping is applied *before* the string reaches `eval()` — e.g. JSON-encoding the value before concatenation. Defeating that escaping is a separate problem from the sink itself; see [[202607010003 Backslash escape-neutralization bypass]] for one concrete escaping bug.

## Links
- Applied in: [[202607010001 DOM XSS via eval on a JSON search response]]
- Contrasted against: [[202606200005 jQuery's dual personality in the dollar function]], [[202606130002 innerHTML never executes script tags]]
