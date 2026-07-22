---
id: "202607040001"
title: Stored DOM XSS bypassing a single-pass angle-bracket filter
date: 2026-07-04
type: case
tags: [xss, stored-xss, filter-bypass, replace]
source: "[[13_Lab13-Stored DOM XSS]]"
publish: false
---

```js
function escapeHTML(html) {
  return html.replace('<', '&lt;').replace('>', '&gt;');
}
```

Comment bodies are rendered via this "sanitizer" before being rendered. The bug — [[202607040002 String.replace with a string argument only replaces the first match]] — is that `.replace()` called with a string (not a regex with `/g`) only ever touches the *first* occurrence. A comment containing two sets of angle brackets has its first pair encoded and every subsequent pair passed through raw:

```
<><img src=1 onerror=alert(1)>
```

The leading `<>` absorbs the single-occurrence replacement; the real payload after it goes untouched. While investigating why an injected `</u>` didn't visibly appear in rendered output, established that browsers silently ignore closing tags with no matching open tag — see [[Browser behaviour on closed tags]] for the underlying browser parsing behaviour that caused the confusion during testing (a pre-existing note in this vault, not part of this pilot conversion).

## Links
- Root cause: [[202607040002 String.replace with a string argument only replaces the first match]]
- Adjacent browser-parsing detail encountered while testing this: [[Browser behaviour on closed tags]]
