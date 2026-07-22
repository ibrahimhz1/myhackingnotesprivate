---
id: "202607170003"
title: A CSP directive is only as strong as its own value's input sanitization
date: 2026-07-17
type: concept
tags: [xss, csp, methodology]
publish: false
---

CSP is delivered as an HTTP response header, and like any header, if any part of its value is built from unsanitized user input, that part is injectable — the security mechanism becomes attacker-writable. The specific irony in this lab family: `report-uri` exists to *report* CSP violations back to the site, but if the reporting endpoint's URL itself contains an unsanitized parameter, an attacker can append `;` plus a brand-new directive, weakening or fully defeating the policy that was supposed to contain them.

Takeaways to check on every CSP-protected target:

```
1. Does ANY reflected/URL-controlled value appear anywhere in the CSP header itself?
   (report-uri, report-to, and any custom nonce-adjacent parameter are the classic spots)
2. If so — can a ';' be injected to append a new directive?
3. Is there a narrower directive (script-src-elem, style-src-attr, etc.) that overrides
   the broad one for a subset of resources — see [[202607170002 script-src-elem overrides script-src for script elements]]
4. Any single injectable directive value can undo the entire policy's protection.
```

## Links
- Concrete instance: [[202607170001 CSP bypass via report-uri header injection]]
- Directive-precedence detail used in that instance: [[202607170002 script-src-elem overrides script-src for script elements]]
