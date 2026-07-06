---
title: 17_Lab17-Reflected XSS with some SVG markup allowed
date: 2026-07-05
tags:
  - 
publish: true
draft: false
description: ""
---
---

This lab reflects user input in a canonical link tag and escapes angle brackets.

To solve the lab, perform a cross-site scripting attack on the home page that injects an attribute that calls the `alert` function.

To assist with your exploit, you can assume that the simulated user will press the following key combinations:

- `ALT+SHIFT+X`
- `CTRL+ALT+X`
- `Alt+X`

Please note that the intended solution to this lab is only possible in Chrome.

---

### Payload

payload sink is URL 

at the end of the lab URL append this payload: 

```
?'accesskey='x'onclick='alert(1)
```

and press ; 
ALT + X
CTRL + ALT + X 
SHIFT + ALT + X
CTRL + SHIFT + X
X 

to trigger the payload execution 

