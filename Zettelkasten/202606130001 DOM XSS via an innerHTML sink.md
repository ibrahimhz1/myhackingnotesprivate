---
id: "202606130001"
title: DOM XSS via an innerHTML sink
date: 2026-06-13
type: case
tags: [xss, dom-xss, innerhtml, source-location-search]
source: "[[4_Lab4-DOM XSS in innerHTML sink using source location.search]]"
publish: false
---

Source: `location.search`. Sink: `span.innerHTML = query`. Nothing in the page source shows the injected value at all after a normal search — confirming it's inserted dynamically via `innerHTML`, not server-templated.

The first instinct — inject `<script>alert(1)</script>` — visibly lands in the DOM (DevTools shows it, unencoded) but never fires. See [[202606130002 innerHTML never executes script tags]] for why. The working payload had to use an event-handler tag instead:

```html
<img/src=asdf onerror="alert('XSS')"/>
```

An `<svg onload=alert(1)>` variant was tried too and *didn't* fire — `onload` on `<svg>` only reliably triggers on initial document parse, not on later dynamic insertion via `innerHTML`. The `<svg><animate ... onbegin=alert(1)>` form does fire on insertion because it isn't tied to a load lifecycle.

## Links
- Why `<script>` is dead on arrival here: [[202606130002 innerHTML never executes script tags]]
- Full payload menu for this sink family: [[202606130003 Reliable non-script event payloads for DOM and innerHTML sinks]]
- Reused via a different route (jQuery `$()`) in: [[202606200004 DOM XSS via a jQuery selector sink using hashchange]]
