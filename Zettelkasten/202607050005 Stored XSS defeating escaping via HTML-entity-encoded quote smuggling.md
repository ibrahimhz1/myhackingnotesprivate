---
id: "202607050005"
title: Stored XSS defeating escaping via HTML-entity-encoded quote smuggling
date: 2026-07-05
type: case
tags: [xss, stored-xss, onclick, html-entities]
source: "[[20_Lab20-Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped]]"
publish: false
---

Comment author's `website` value is inserted into both an `href` attribute (HTML-encoded) and an inline `onclick` handler calling `tracker.track('{website}')` (single quotes and backslashes escaped). Testing all of `< > ' " \` \` /` individually against the sink showed: `<`, `>`, `"` HTML-encoded; `'`, `\` backslash-escaped; backtick and `/` passed through untouched.

Because both encodings apply to the *raw* value before it's placed in the HTML, and the HTML parser decodes entities **before** handing the `onclick` attribute's text to the JS engine, an HTML-entity-encoded quote (`&apos;`) survives the app's own escaping (which only looks for a literal `'` character to backslash-escape) and is decoded back into a real `'` by the browser at render time — arriving in front of the JS engine unescaped:

```
http://something&apos;);alert(&apos;
http://something?&apos;-alert(1)-&apos;
```

This is the general principle [[202607050006 HTML entities decode before use in an inline event handler's JS context]]: an escaper looking for literal characters in the *input* can be bypassed by supplying the HTML-encoded form of that character, because the browser's HTML parser performs entity decoding as a separate, later step than the app's string escaping — the two layers don't compose the way the app author assumed.

## Links
- General principle: [[202607050006 HTML entities decode before use in an inline event handler's JS context]]
- Same "smuggle past a layer that only checks the input, not what the next layer sees" family as: [[202607010003 Backslash escape-neutralization bypass]], [[202606200003 Bypassing javascript colon string filters]]
