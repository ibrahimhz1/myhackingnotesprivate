---
id: "202606200005"
title: jQuery's dual personality in the dollar function
date: 2026-06-20
type: concept
tags: [xss, jquery, sink-analysis]
publish: false
---

jQuery's `$()` does two entirely different things depending on the first character of its argument string:

```javascript
$('section.blog-list h2')        // no leading '<' → CSS selector mode, queries the DOM
$('<img src=x onerror=alert()>') // leading '<'    → HTML creation mode, builds real DOM nodes
```

The actual check, simplified from jQuery's source:

```javascript
var rquickExpr = /^(?:\s*(<[\w\W]+>)[^>]*)/;
if (rquickExpr.test(selector)) {
  jQuery.parseHTML(selector); // real DOM nodes, real event handlers
} else {
  document.querySelectorAll(selector);
}
```

Vanilla `document.querySelector()` would just throw a `SyntaxError` on an invalid selector string — it never silently reinterprets its argument as HTML. jQuery's convenience feature (`$('<div>')` to create elements) is exactly what turns `$()` into a dangerous sink whenever unfiltered user input reaches it: a string containing `<` is parsed into a **live** DOM element with working event handlers, not just displayed as text.

Sink-type cheat sheet this generalizes to:

```
eval("user_input")          → JS execution sink  → string injection works
setTimeout("user_input")    → JS execution sink  → string injection works
$("user_input")             → selector sink      → string injection does NOT work, HTML injection does
innerHTML = "user_input"    → HTML sink          → tag/event injection works
```

## Links
- Discovered while solving: [[202606200004 DOM XSS via a jQuery selector sink using hashchange]]
- The execution-sink category this is contrasted against: [[202607010002 eval() as a JavaScript execution sink]]
