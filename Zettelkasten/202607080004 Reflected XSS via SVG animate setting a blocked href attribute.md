---
id: "202607080004"
title: Reflected XSS via SVG animate setting a blocked href attribute
date: 2026-07-08
type: case
tags: [xss, reflected-xss, svg, filter-bypass]
source: "[[27_Lab27-Reflected XSS with event handlers and href attributes blocked]]"
publish: false
---

Filter blocks all event-handler attributes and any `href` containing `javascript:`. Allowed tags were mapped with [[202607080006 Brute-forcing an unknown tag or attribute allowlist with ffuf]] and turned out to include `<a>`, `<animate>`, `<image>`, `<title>`, `<svg>` — but a static `<a href="javascript:...">` is still blocked outright.

The bypass: [[202607080005 SVG animate can set attributes a static filter blocks directly]]. An `<a>` tag with **no** `href` attribute at all passes the filter (nothing to block); an SVG `<animate>` element then sets that `href` dynamically after the page renders — the filter only ever inspected the static attribute in the submitted input, never the DOM's post-render state.

```html
<svg width="200" height="50">
  <a>
    <animate attributeName="href" values="javascript:alert(1)" begin="0s"></animate>
    <text x="10" y="30" fill="blue" style="cursor:pointer;text-decoration:underline">Click me</text>
  </a>
</svg>
```

`width`/`height` and an explicit `<text>` element are both required because `<svg>` defaults to zero size, and raw text nodes inside SVG aren't rendered — SVG has its own rendering rules distinct from HTML's.

## Links
- Core bypass principle: [[202607080005 SVG animate can set attributes a static filter blocks directly]]
- Discovery method for the allowed tag set: [[202607080006 Brute-forcing an unknown tag or attribute allowlist with ffuf]]
- Same "filter checks the input, not the rendered DOM" theme as: [[202607040005 Allowlists that permit unknown tags still allow arbitrary attributes]], [[202606200003 Bypassing javascript colon string filters]]
