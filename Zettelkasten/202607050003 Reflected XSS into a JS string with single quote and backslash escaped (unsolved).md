---
id: "202607050003"
title: Reflected XSS into a JS string with single quote and backslash escaped (unsolved)
date: 2026-07-05
type: case
tags: [xss, reflected-xss, js-string-breakout, stub]
source: "[[18_Lab18-Reflected XSS into a JavaScript string with single quote and backslash escaped]]"
publish: false
---

Lab description only — no working payload recorded in the source note. The sink is a JS string with both `'` and `\` escaped, which by [[202606250002 Breaking out of a JS string literal via quote, semicolon, and comment]] rules out the plain quote-breakout recipe; [[202607010003 Backslash escape-neutralization bypass]] is the natural next thing to try (it's designed for exactly this — escaped quote *and* escaped backslash), but it isn't confirmed against this specific lab.

## Links
- Technique to try: [[202607010003 Backslash escape-neutralization bypass]]
- Baseline recipe this defeats: [[202606250002 Breaking out of a JS string literal via quote, semicolon, and comment]]
