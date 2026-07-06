---
title: Browser behaviour on closed tags
date: 2026-07-04
tags:
  - 
publish: true
draft: false
description: ""
---
>If we try to insert only </u> closing tag of HTML element in the HTML page, The browser **does not display the closing `</u>`** because it is interpreted as an HTML closing tag, not as text.

```html
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Sample Document</title>

</head>

<body>

    <p>example</u></p>

</body>

</html>
```


![[Pasted image 20260704154844.png]]

