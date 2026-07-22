---
id: "202607040003"
title: Reflected XSS via body onresize and an attribute-triggered resize
date: 2026-07-04
type: case
tags: [xss, reflected-xss, tag-allowlist-bypass]
source: "[[14_Lab14-Reflected XSS into HTML context with most tags and attributes blocked]]"
publish: false
---

Most tags/attributes blocked, but `<body onresize>` and an injected `style="width:...` attribute on a preceding element survive the filter:

```
"><body onresize=print()>" onload=this.style.width='100px'>
```

The `onload` on the first (already-present) element resizes the page shortly after render by changing its `width`, which triggers the injected `<body onresize=print()>` handler without any user interaction — no click, no wait for a real window resize. This is the same "find a tag/attribute pair the allowlist didn't anticipate, then trigger it via a native DOM side-effect rather than an event the filter is watching for" pattern as [[202607080005 SVG animate can set attributes a static filter blocks directly]].

## Links
- Same allowlist-evasion mindset, more fully developed: [[202607080004 Reflected XSS via SVG animate setting a blocked href attribute]]
