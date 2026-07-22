---
id: "202607170001"
title: CSP bypass via report-uri header injection
date: 2026-07-17
type: case
tags: [xss, csp-bypass, header-injection]
source: "[[30_Lab-Reflected XSS protected by CSP, with CSP bypass]]"
publish: false
---

```
Content-Security-Policy: default-src 'self'; script-src 'self' 'nonce-RANDOM'; report-uri /csp-report?token=
```

The search term is reflected both into the HTML body (confirming a `<script>` injection lands but is blocked by CSP) **and** into the `report-uri` directive's `token` parameter of the CSP header itself — a header-injection bug inside the security control that's supposed to be preventing XSS. Because the token value isn't sanitized before being echoed into the header, a `;` injects an entirely new CSP directive:

```
Search input: test;script-src-elem 'unsafe-inline'
→ header becomes: ...report-uri /csp-report?token=test;script-src-elem 'unsafe-inline'
```

[[202607170002 script-src-elem overrides script-src for script elements]] means this new directive re-enables inline `<script>` execution specifically, even though the original `script-src` never allowed it. Combined single payload (both injections in one request):

```
<script>alert(1)</script>&token=;script-src-elem 'unsafe-inline'
```

The HTML-body half lands the script tag; the `token=` half rewrites the CSP that would otherwise have blocked it — both land from the same response, so the weakened policy is already in effect by the time the browser evaluates the injected script.

## Links
- Directive-precedence mechanism exploited: [[202607170002 script-src-elem overrides script-src for script elements]]
- General principle this is an instance of: [[202607170003 A CSP directive is only as strong as its own value's input sanitization]]
