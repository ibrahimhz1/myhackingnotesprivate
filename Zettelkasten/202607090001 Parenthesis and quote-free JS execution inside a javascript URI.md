---
id: "202607090001"
title: Parenthesis and quote-free JS execution inside a javascript URI
date: 2026-07-09
type: case
tags: [xss, reflected-xss, javascript-uri, filter-bypass]
source: "[[28_Lab28-Reflected XSS in a JavaScript URL with some characters blocked]]"
publish: false
---

Input reflects into a `javascript:` URI inside an existing `fetch()` call, with `" ` ( )` all blocked — ruling out both new string literals and any direct function call syntax.

```
&'},x=x=>{throw/**/onerror=alert,'XSS'},toString=x,window+'',{x:'
```

Breakdown: `&'}` closes the existing `fetch()` body/options; the comma operator (see [[202607090002 Comma operator plus onerror and throw calls a function without its name or parens]]) chains a sequence of plain assignment/coercion expressions — none of which is a function *call* in the literal sense — that ultimately triggers `alert('XSS')` via `window.toString()` coercion. `/**/` substitutes for a space in case whitespace is stripped. The trailing `{x:'` re-opens an object whose string is closed by a `'` that already exists later in the original code, keeping the surrounding syntax valid.

Getting the raw `'` characters through the URL and HTML layers relies on [[202607090003 javascript URIs are URL-decoded after HTML parsing before JS execution]] — the payload is URL-encoded in the address bar/HTML, but arrives at the JS engine already decoded.

## Links
- Core trick: [[202607090002 Comma operator plus onerror and throw calls a function without its name or parens]]
- Why encoded characters still work: [[202607090003 javascript URIs are URL-decoded after HTML parsing before JS execution]]
