---
id: "202607170002"
title: script-src-elem overrides script-src for script elements
date: 2026-07-17
type: concept
tags: [xss, csp]
publish: false
---

CSP has both a broad directive (`script-src`, governing all script resources/behavior) and narrower, more specific ones (`script-src-elem`, specifically for `<script>` elements). Where both are present, the more specific directive takes precedence for the category of resource it targets:

```
script-src          → broad: governs all script-related behavior
script-src-elem     → specific: governs <script> elements only

If both present → script-src-elem wins for <script> elements,
                   even if script-src itself doesn't include 'unsafe-inline'
```

So injecting `script-src-elem 'unsafe-inline'` as an additional directive re-enables inline `<script>` execution for that response, regardless of how strict the original `script-src` was — the CSP-parsing algorithm's directive-precedence rules do the rest.

## Links
- Applied in: [[202607170001 CSP bypass via report-uri header injection]]
