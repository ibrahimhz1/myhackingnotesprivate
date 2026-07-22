---
id: "202606250002"
title: Breaking out of a JS string literal via quote, semicolon, and comment
date: 2026-06-25
type: concept
tags: [xss, js-string-breakout, payload-design]
publish: false
---

General recipe for injecting into a raw (unescaped) JavaScript string literal, whether the sink is a `<script>` block, an inline event handler, or a template being `eval`'d:

```
1. Close the string    — match the delimiter in use: ' or "
2. Terminate the statement — ;
3. Inject the call     — alert(1)
4. Silence the rest     — // comments out anything the app appends after your input
```

```
'; alert('XSS');//
```

This only works when the delimiter quote itself reaches the JS engine unescaped. When quotes are backslash-escaped before storage, this recipe alone fails and you need [[202607010003 Backslash escape-neutralization bypass]] first to get an unescaped quote through. When the sink is specifically inside a template literal rather than a quoted string, `${}` interpolation is a separate, often simpler route — see [[202607060002 Template literal interpolation executes without breaking the backticks]].

## Links
- Applied in: [[202606250001 Reflected XSS by breaking out of a JavaScript string literal]]
- Next layer when quotes are escaped: [[202607010003 Backslash escape-neutralization bypass]]
- Alternative when the context is a template literal: [[202607060002 Template literal interpolation executes without breaking the backticks]]
