---
id: "202606130002"
title: innerHTML never executes script tags
date: 2026-06-13
type: concept
tags: [xss, innerhtml, browser-behaviour]
publish: false
---

The HTML living standard explicitly marks `<script>` elements created via `innerHTML` assignment as **parser-inserted = false**, and the browser refuses to execute any script node that wasn't inserted by the *original document parser*. This is a spec-mandated security decision, not a WAF or filter — there is nothing to bypass with encoding tricks.

```
span.innerHTML = "<script>alert(1)</script>"
→ DevTools shows: <span><script>alert(1)</script></span>   (visible, unencoded)
→ Actually happens: browser creates the script node, flags it non-executable, skips it entirely
```

The one place `<script>` *does* still execute is when the sink is `document.write()` or `eval()` instead of `innerHTML` — those go through the original parsing/execution path, not the DOM-mutation path.

```javascript
document.write('<script>alert(1)</script>');  // executes
eval('<script>alert(1)</script>');            // executes
element.innerHTML = '<script>alert(1)</script>'; // inert
```

Practical consequence: whenever the sink is `innerHTML` (or `.html()` in jQuery, `dangerouslySetInnerHTML` in React, etc.), stop looking for script-tag payloads and go straight to event-handler-bearing tags instead.

## Links
- Payload menu for this exact situation: [[202606130003 Reliable non-script event payloads for DOM and innerHTML sinks]]
- Applies to: [[202606130001 DOM XSS via an innerHTML sink]]
- Contrast — where script tags DO work: [[202607010002 eval() as a JavaScript execution sink]], `document.write` cases like [[202606120003 DOM XSS via document.write into an image src attribute]]
