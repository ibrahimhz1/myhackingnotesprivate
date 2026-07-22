---
id: "202607010001"
title: DOM XSS via eval on a JSON search response
date: 2026-07-01
type: case
tags: [xss, dom-xss, eval, json]
source: "[[12_Lab12-Reflected DOM XSS]]"
publish: false
---

```js
xhr.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
    eval('var searchResultsObj = ' + this.responseText);
    displaySearchResults(searchResultsObj);
  }
};
```

The server JSON-encodes the search term into `responseText`, and that raw text is concatenated straight into [[202607010002 eval() as a JavaScript execution sink]]. First attempt breaks out of the JSON string naively:

```
?search=hello"-alert(1)}//
```

but the server escapes the injected `"` to `\"`, trapping it inside the string — the `-` subtraction trick (`"hello"-alert(1)` forces both operands to evaluate, calling `alert(1)` as a side effect) never gets a chance to run because the breakout quote itself never lands unescaped. The fix needed [[202607010003 Backslash escape-neutralization bypass]]: prefixing the quote with a backslash makes the server escape *that* backslash instead, leaving the original quote free.

```
?search=\"-alert(1)}//
```

Server response: `{"searchTerm":"\\"-alert(1)}//"}` — two backslashes before the quote means the escape was neutralized and the string is genuinely closed at that point from `eval()`'s perspective.

## Links
- Sink category: [[202607010002 eval() as a JavaScript execution sink]]
- Bypass technique used: [[202607010003 Backslash escape-neutralization bypass]]
- Same "close string, subtract/comma, comment out the rest" shape as: [[202606250002 Breaking out of a JS string literal via quote, semicolon, and comment]]
