---
id: "202606200002"
title: javascript URI as an XSS vector when href is fully controlled
date: 2026-06-20
type: concept
tags: [xss, javascript-uri, href]
publish: false
---

Decision point for any `href`/`src`-style attribute sink: can the injected value break **out** of the attribute (via unescaped `"` or `>`), or does it only control the URI **value** itself?

```
Can you break out of href="..."?  (test with " or >)
│
├─ YES → full HTML injection is available
│         best: " onmouseover="alert(1)"  or  "><svg onload=alert(1)>
│
└─ NO  → stuck controlling only the URI value
          best: javascript:alert(document.cookie)
```

If you can't break out but you *do* fully control the value (not just a parameter appended to an existing legitimate URL), a `javascript:` scheme is a fully spec-compliant XSS vector requiring only a click:

```html
<a href="javascript:alert(document.cookie)">Click me</a>
```

`data:` URIs are a fallback for downloadable-HTML execution, but modern Chrome/Firefox increasingly treat top-level `data:` navigation from a click as a download rather than executing it — treat as unreliable, not a first choice.

Real-world severity framing: `javascript:` requiring a click is medium-high; if you can *also* achieve attribute breakout (`onmouseover`/`onfocus`, near-zero interaction) that's high; on an admin/payment page, critical.

## Links
- Applied in: [[202606200001 DOM XSS via a jQuery href sink using a javascript URI]], [[202606240001 Stored XSS via an anchor href fully replaced with a javascript URI]]
- Filter evasion for the literal string `javascript:`: [[202606200003 Bypassing javascript colon string filters]]
- If the value is instead appended to a legitimate redirect URL rather than fully replacing it, see open-redirect-to-XSS chaining (same source note).
