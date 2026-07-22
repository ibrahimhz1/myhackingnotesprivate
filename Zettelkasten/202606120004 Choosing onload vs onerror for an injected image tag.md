---
id: "202606120004"
title: Choosing onload vs onerror for an injected image tag
date: 2026-06-12
type: concept
tags: [xss, payload-design, img-tag]
publish: false
---

When injecting an `<img>` tag as an XSS vector, the choice between `onerror` and `onload` depends on whether the attacker controls the `src` attribute:

- **`onerror` needs the image to actually fail to load.** If the attacker can set `src` (e.g. to `x`, a non-existent path), the load fails and `onerror` fires reliably. This is the default choice in most sinks (`innerHTML`, direct tag injection).
- **`onload` is the only option when the attacker can't control `src`.** If the payload only lands *inside* an existing, valid `src` value — e.g. appended as a query string after a real image path like `tracker.gif?searchTerms=PAYLOAD` — the image still loads successfully. `onerror` would never fire, so `onload` is the correct (and only working) event.

The general rule: figure out first whether the injection point lets you break the resource load. If yes, `onerror` is more reliable elsewhere too (see [[202606130003 Reliable non-script event payloads for DOM and innerHTML sinks]]). If the resource will always load fine, `onload` is what fires.

## Links
- Applied in: [[202606120003 DOM XSS via document.write into an image src attribute]]
- General payload menu once you have an injection point: [[202606130003 Reliable non-script event payloads for DOM and innerHTML sinks]]
