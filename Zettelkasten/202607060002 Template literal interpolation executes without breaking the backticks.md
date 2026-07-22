---
id: "202607060002"
title: Template literal interpolation executes without breaking the backticks
date: 2026-07-06
type: concept
tags: [xss, template-literal, javascript]
publish: false
---

Inside a JS template literal (backtick-delimited string), `${expression}` is not text — it's a live expression slot evaluated at runtime, regardless of what other characters surround it or how heavily they're escaped:

```js
`Hello ${user}, you have ${items} items.`   // ${} always evaluates
```

If user input lands anywhere *inside* the backticks — even nested within a quoted sub-string that's part of the literal's static text — and the input itself isn't stripped of `${...}` syntax specifically, injecting `${alert(1)}` executes without needing to escape the backticks, break the string, or defeat any angle-bracket/quote encoding at all. Sanitizers that escape `< > ' " \` \\` but don't specifically strip `${` miss this entirely, because none of those escaped characters are what makes the payload work.

## Links
- Applied in: [[202607060001 XSS inside a JS template literal via placeholder interpolation]]
- Contrast with the general string-breakout recipe it bypasses the need for: [[202606250002 Breaking out of a JS string literal via quote, semicolon, and comment]]
