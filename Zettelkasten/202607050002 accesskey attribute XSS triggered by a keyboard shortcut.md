---
id: "202607050002"
title: accesskey attribute XSS triggered by a keyboard shortcut
date: 2026-07-05
type: case
tags: [xss, reflected-xss, accesskey, canonical-link]
source: "[[17_Lab17-Reflected XSS in canonical link tag]]"
publish: false
---

Injection point is a `<link rel="canonical">` tag in the page `<head>`, which normally isn't interactive at all — but the `accesskey` attribute plus an `onclick` handler turns it into something a simulated user's keypress can fire, even on an element that's otherwise invisible/non-clickable:

```
?'accesskey='x'onclick='alert(1)
```

`accesskey='x'` binds the element to a browser-defined key combination (Chrome-specific: `Alt+X` / `Ctrl+Alt+X` / `Alt+Shift+X`, depending on OS), and pressing it fires the bound element's `onclick` exactly as if it had been clicked — despite the element never being visible or normally focusable. Worth remembering as a general technique: **`accesskey` turns any element into a keyboard-triggerable one**, useful whenever a lab or app scenario specifies the victim will press a particular key combination.

## Links
- Sibling non-interaction-trigger techniques: [[202607040004 XSS via custom non-standard HTML elements]] (autofocus/tabindex), [[202607080004 Reflected XSS via SVG animate setting a blocked href attribute]] (native animation, no click needed at all)
