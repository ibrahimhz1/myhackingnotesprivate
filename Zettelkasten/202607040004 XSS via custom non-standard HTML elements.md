---
id: "202607040004"
title: XSS via custom non-standard HTML elements
date: 2026-07-04
type: case
tags: [xss, reflected-xss, tag-allowlist-bypass, custom-elements]
source: "[[15_Lab15-Reflected XSS into HTML context with all tags blocked except custom ones]]"
publish: false
---

Filter blocks every *known* HTML tag but doesn't recognize (and so doesn't block) an arbitrary made-up tag name — see [[202607040005 Allowlists that permit unknown tags still allow arbitrary attributes]]. Browsers happily render unrecognized elements as generic inline elements and still process any standard attribute/event handler placed on them:

```html
<abc-def autofocus onfocus="alert(document.cookie)" tabindex=1>
```

`tabindex=1` plus `autofocus` is needed because a non-standard element isn't focusable by default — adding `tabindex` makes it a focus target, and `autofocus` then fires `onfocus` immediately on page load with no interaction. A variant targets a fragment identifier instead of `autofocus`:

```html
<abc-def id="abc" onfocus="alert(document.cookie)" tabindex=1>#abc
```

Delivery: since the victim needs to actually load a URL containing the payload, a simple redirect page works — `window.location='...?search=<payload>'`.

## Links
- Underlying gap in the filter's logic: [[202607040005 Allowlists that permit unknown tags still allow arbitrary attributes]]
