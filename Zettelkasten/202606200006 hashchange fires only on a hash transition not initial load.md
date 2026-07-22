---
id: "202606200006"
title: hashchange fires only on a hash transition not initial load
date: 2026-06-20
type: concept
tags: [xss, hashchange, delivery]
publish: false
---

A page listening for `window.addEventListener('hashchange', ...)` will **not** run that handler just because the page was loaded with a hash already in the URL. The event only fires on a *transition* from one hash value to another while the page is already open.

```
New tab opens https://site.com/#<payload>
├── page loads, DOM ready, listener registers
└── hashchange? NEVER FIRES — hash didn't change, it was there from the start

Same tab: https://site.com/#a  →  https://site.com/#<payload>
└── hashchange fires ✅ — this is a real transition
```

Consequence for exploitation: you can't just hand a victim a crafted URL and expect a hash-based DOM XSS to fire on load — the delivery mechanism has to force a hash change *after* the page has already loaded once. See [[202606200007 iframe onload self-trigger delivery for hash-based DOM XSS]] for the standard way to do that against a victim who never touches the URL bar.

## Links
- Motivating case: [[202606200004 DOM XSS via a jQuery selector sink using hashchange]]
- Delivery technique this necessitates: [[202606200007 iframe onload self-trigger delivery for hash-based DOM XSS]]
