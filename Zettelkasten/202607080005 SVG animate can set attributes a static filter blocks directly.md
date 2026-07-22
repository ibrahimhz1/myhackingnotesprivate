---
id: "202607080005"
title: SVG animate can set attributes a static filter blocks directly
date: 2026-07-08
type: concept
tags: [xss, svg, filter-bypass]
publish: false
---

Input filters almost always validate what was *submitted*, not what the DOM looks like after the browser finishes rendering and running animations. SVG's `<animate attributeName="X" values="...">` element can set the value of attribute `X` on a sibling/parent element **after** the page loads — including attributes the filter would have blocked if they'd been present in the submitted markup:

```
Event handler filter:  no onerror/onload/onclick anywhere in the input → passes
href="javascript:" filter: the <a> has no href in the submitted input → passes
Result: page loads → <animate> fires → href becomes javascript:alert(1) at runtime
        → filter never saw this because it only checked static input
```

The general pentesting heuristic this teaches: when a direct approach is blocked, don't try harder with the same shape of payload — ask what native browser feature can produce the blocked state *indirectly*, after the filter has already finished looking. SVG animation, CSS animation/transition end-events, and meta-refresh are all in this same "the filter checks the input, not the runtime DOM" category.

## Links
- Applied in: [[202607080004 Reflected XSS via SVG animate setting a blocked href attribute]]
- Same category of bug: [[202607040005 Allowlists that permit unknown tags still allow arbitrary attributes]], [[202607040003 Reflected XSS via body onresize and an attribute-triggered resize]]
