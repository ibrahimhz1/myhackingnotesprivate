---
id: "202606240001"
title: Stored XSS via an anchor href fully replaced with a javascript URI
date: 2026-06-24
type: case
tags: [xss, stored-xss, javascript-uri]
source: "[[8_Lab8-Stored XSS into anchor href attribute with double quotes HTML-encoded]]"
publish: false
---

Comment author's `website` field is inserted as the `href` of the linked author name: `<a href="{website}">name</a>`. Double quotes are HTML-encoded, so attribute breakout is blocked — but the `website` field itself has no URL-format validation, so the entire `href` value is attacker-controlled.

```
javascript:alert(document.cookie)
```

Set as the website value, this fully replaces the href, and clicking the linked author name executes it. Identical underlying vector to [[202606200001 DOM XSS via a jQuery href sink using a javascript URI]] — the recurring pattern is: *when you can't break out of an attribute but you fully control its value, check whether that value's own scheme can be weaponized.*

## Links
- Same technique, DOM sink instead of stored: [[202606200001 DOM XSS via a jQuery href sink using a javascript URI]]
- Underlying concept: [[202606200002 javascript URI as an XSS vector when href is fully controlled]]
