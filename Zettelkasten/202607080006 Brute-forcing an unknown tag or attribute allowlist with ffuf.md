---
id: "202607080006"
title: Brute-forcing an unknown tag or attribute allowlist with ffuf
date: 2026-07-08
type: concept
tags: [xss, methodology, ffuf, fuzzing]
publish: false
---

When a filter's exact allowed tag/attribute set isn't documented (or the lab intentionally withholds it), Burp's Community Intruder is far too slow for a full HTML-tag wordlist. `ffuf` against a saved raw request is a much faster way to map the allowlist:

```
$ touch requests/lab_request.txt   # paste the raw GET/POST request, with FUZZ marking the injection point implied by the wordlist substitution
$ ffuf --request requests/lab_request.txt \
       -w tags_wordlist_without_angle_bracket.txt \
       -p 0.5
```

Diffing response length/status across the wordlist run reveals which tag names the filter lets through (distinct response, usually longer/different length because the tag rendered) versus which get stripped (identical, shorter response). Once the allowed tag set is known (e.g. `<a>`, `<animate>`, `<image>`, `<title>`, `<svg>`), the next step is testing which *attributes* each of those tags accepts, the same way.

## Links
- Applied in: [[202607080004 Reflected XSS via SVG animate setting a blocked href attribute]]
