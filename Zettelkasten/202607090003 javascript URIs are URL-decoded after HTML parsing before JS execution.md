---
id: "202607090003"
title: javascript URIs are URL-decoded after HTML parsing before JS execution
date: 2026-07-09
type: concept
tags: [xss, javascript-uri, url-encoding]
publish: false
---

A `javascript:` URI goes through two independent parsing stages, and characters that are dangerous at one stage can be safely smuggled through it URL-encoded and still take effect at the other:

```
Stage 1 — HTML parser reads the href attribute value:
  sees %27, %3d, %26 etc. as inert literal text, doesn't decode them

Stage 2 — the javascript: URI handler, just before executing:
  URL-decodes the value: %27→'  %3d→=  %3f→?  %26→&

Stage 3 — JS engine executes:
  receives the fully decoded string, sees real ' = ? & characters
```

Practical effect: if a WAF/filter is inspecting the *raw HTML* for literal quote/equals/ampersand characters, sending their `%XX` URL-encoded forms passes that inspection cleanly — and the browser still restores them to real characters right before handing the string to the JavaScript engine. URL encoding here isn't obfuscation for its own sake, it's transport-layer safety through the HTML parser that gets undone exactly when it matters.

## Links
- Applied in: [[202607090001 Parenthesis and quote-free JS execution inside a javascript URI]]
- Same "two pipeline stages don't share context" family as: [[202607010003 Backslash escape-neutralization bypass]], [[202607050006 HTML entities decode before use in an inline event handler's JS context]]
