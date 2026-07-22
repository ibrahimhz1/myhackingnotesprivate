---
id: "202606120003"
title: DOM XSS via document.write into an image src attribute
date: 2026-06-12
type: case
tags: [xss, dom-xss, document-write, source-location-search]
source: "[[3_Lab3-DOM XSS in document.write sink using source location.search]]"
publish: false
---

Source: `location.search` (the `search` URL parameter). Sink: `document.write()` building an `<img>` tag whose `src` attribute embeds the parameter unescaped.

```js
function trackSearch(query) {
  document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');
}
```

The injected value only ever reaches the `src` attribute's *value* — it can't set `src` itself (that's already `/resources/images/tracker.gif?searchTerms=`), so the attack has to break out of the attribute rather than control the whole tag.

```
search: " onload=alert('XSS')>
```

The `"` closes the `src` value, `onload=alert('XSS')` adds a new attribute, `>` closes the tag early (the browser tolerates the dangling real `">` that follows).

## Links
- Event choice reasoning for this exact scenario: [[202606120004 Choosing onload vs onerror for an injected image tag]]
- Compare with the `<select>`/`<option>` variant of the same source/sink pattern: [[202606250003 DOM XSS by breaking out of a select and option element]]
