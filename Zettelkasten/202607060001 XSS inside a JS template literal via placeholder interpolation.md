---
id: "202607060001"
title: XSS inside a JS template literal via placeholder interpolation
date: 2026-07-06
type: case
tags: [xss, reflected-xss, template-literal]
source: "[[21_Lab21-Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped]]"
publish: false
---

```js
var message = `0 search results for 'xyz123>abc...'`;
document.getElementById('searchMessage').innerText = message;
```

Every dangerous character (`< > ' " \` \\`) is Unicode-escaped before insertion — normally enough to prevent breaking out of the template literal. But the sink is a template literal, and [[202607060002 Template literal interpolation executes without breaking the backticks]] means breaking out was never necessary in the first place: the `${}` placeholder syntax is itself live JavaScript regardless of what's escaped around it.

```
${alert()}
```

Confirmed the placeholder works even nested inside single or double quotes within the template (`some' ${alert()} '`), because `${}` is parsed by the JS engine as an expression slot the moment it appears between backticks — quote characters around it are irrelevant to whether it fires.

## Links
- Underlying mechanism: [[202607060002 Template literal interpolation executes without breaking the backticks]]
- Contrast — the "close the string" approach this makes unnecessary: [[202606250002 Breaking out of a JS string literal via quote, semicolon, and comment]]
