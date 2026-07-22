---
id: "202607010003"
title: Backslash escape-neutralization bypass
date: 2026-07-01
type: concept
tags: [xss, filter-bypass, escaping]
publish: false
---

When a server escapes `"` to `\"` before embedding user input in a JSON/JS string, sending a raw `"` alone stays trapped inside the string. But if the injected value is a backslash *followed by* a quote (`\"`), the server escapes **both characters independently**, producing `\\\"` — and JavaScript reads escape sequences left to right:

```
You send:        \"
Server escapes:  \\\"
                 ││└── your original "  → server added \ before it
                 └──── your original \  → server added \ before it

JS reads:  \\  → one literal backslash (escape sequence consumed)
           "   → NOT escaped anymore → closes the string ✅
```

Full payload: `\"-alert(1)}//` becomes, after server escaping, `{"searchTerm":"\\"-alert(1)}//"}`. JavaScript parses `\\` as a literal backslash, then hits an unescaped `"` that legitimately terminates the string — everything after (`-alert(1)}`) runs as code, and `//` comments out the remainder.

The general principle: **the server escaped your quote — smuggle in a backslash to consume the server's escaping backslash, leaving your quote free.** This is a specific instance of "a filter that escapes characters individually, without considering how its own escape character composes with itself, can be tricked into self-cancelling."

## Links
- Applied in: [[202607010001 DOM XSS via eval on a JSON search response]]
- Same class of "filter checks the input, not what the runtime actually sees" bug as: [[202606200003 Bypassing javascript colon string filters]], [[202607090003 javascript URIs are URL-decoded after HTML parsing before JS execution]]
