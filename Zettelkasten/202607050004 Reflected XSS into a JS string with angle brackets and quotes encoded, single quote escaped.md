---
id: "202607050004"
title: Reflected XSS into a JS string with angle brackets and quotes encoded, single quote escaped
date: 2026-07-05
type: case
tags: [xss, reflected-xss, js-string-breakout]
source: "[[19_Lab19-Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped]]"
publish: false
---

Angle brackets and `"` are HTML-encoded (irrelevant inside a JS string anyway), and `'` is backslash-escaped. [[202607010003 Backslash escape-neutralization bypass]] applies directly: prefix the quote with a backslash so the escaper's own escaping neutralizes itself.

```
\'-alert(1)//
\';alert(1)//
```

Both variants close the string once the escape is neutralized; the first uses the subtraction-forces-evaluation trick, the second a plain statement terminator — either closes cleanly given `//` mops up the remainder.

## Links
- Technique: [[202607010003 Backslash escape-neutralization bypass]]
- Same bypass applied against a JSON sink instead of an inline script: [[202607010001 DOM XSS via eval on a JSON search response]]
