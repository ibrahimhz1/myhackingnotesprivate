---
id: "202607080002"
title: XSS defeats CSRF defenses because the victim's browser supplies both the token and the session
date: 2026-07-08
type: concept
tags: [xss, csrf, sameorigin]
publish: false
---

CSRF tokens protect against a request forged from a **different origin** — one that can't read the token because of same-origin policy. That protection model has an implicit assumption baked in: the attacker's code runs on a different origin than the target.

XSS breaks that assumption entirely. Once attacker-controlled JavaScript executes *inside* the vulnerable site's own origin (via a stored payload), it is — from the browser's perspective — first-party code:

- It can `fetch()` any same-origin page and read the CSRF token straight out of the response body.
- Any request it makes automatically carries the victim's session cookie (`credentials: 'include'`), because it's a same-origin request from the victim's own browser.

There is no separate "forge a token" step; there's nothing to forge. The token and session are both just *sitting there*, accessible to same-origin code, and CSRF protections were never designed to stop same-origin code from reading them. This is why an XSS finding on a page an authenticated user visits is treated as strictly more severe than the CSRF findings it can bypass — **XSS anywhere on an origin defeats CSRF everywhere on that origin.**

## Links
- Applied in: [[202607080001 Using stored XSS to bypass CSRF protection]]
- Same principle under a CSP that blocks the straightforward fetch-and-post version: [[202607120001 Dangling-markup CSRF-token theft under a strict CSP]]
