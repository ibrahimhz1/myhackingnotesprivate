---
id: "202607040005"
title: Allowlists that permit unknown tags still allow arbitrary attributes
date: 2026-07-04
type: concept
tags: [xss, filter-bypass, allowlist]
publish: false
---

A tag-name allowlist built to block *known* dangerous elements (`script`, `img`, `svg`, `iframe`, ...) has no opinion about a tag name it's never heard of. If the filter's logic is "strip tags not on this list" rather than "strip everything except this list," a fabricated element name like `<abc-def>` passes straight through — and critically, the filter usually doesn't separately re-validate the *attributes* on that surviving tag, so `onfocus`, `autofocus`, `tabindex`, etc. ride along untouched.

This is a specific case of a broader lesson: an allowlist is only as strong as its enumeration. Anything the enumeration didn't anticipate — an unknown tag name, an unknown attribute, a dynamically-set attribute value (see [[202607080005 SVG animate can set attributes a static filter blocks directly]]) — is implicitly allowed.

## Links
- Applied in: [[202607040004 XSS via custom non-standard HTML elements]]
- Same "filter didn't anticipate this" theme: [[202607080005 SVG animate can set attributes a static filter blocks directly]]
