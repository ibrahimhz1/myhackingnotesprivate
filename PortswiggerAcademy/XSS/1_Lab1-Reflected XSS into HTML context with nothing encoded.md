---
title: Lab1-Reflected XSS into HTML context with nothing encoded
date: 2026-06-12
tags:
  - 
publish: true
draft: false
description: ""
---
---
This lab contains a simple reflected cross-site scripting vulnerability in the search functionality.
To solve the lab, perform a cross-site scripting attack that calls the `alert` function.

---

There is an search feature in the page: 
![[Pasted image 20260612010025.png]]

Search something and click on "Search" button: 
![[Pasted image 20260612010338.png]]

>The Search string "xyz123" displayed in the page. 

Lets check the page source, to see if how the searched string is passed into the HTML sink. 
![[Pasted image 20260612010535.png]]>The sink is `<h1>` tag. and withing an `''` single quotes.

#### Payload
Lets try the simple payload: 
```
<script>alert('XSS exec')</script>
```

![[Pasted image 20260612010749.png]]
click on "Search" button

![[Pasted image 20260612010823.png]]

>**XSS executed.**

![[Pasted image 20260612011002.png]]

Check the page source: 
![[Pasted image 20260612011146.png]]

>No input sanitization for the user input searched string, hence vulnerable payloads resulted into XSS execution. 

### Solution:
>Sanitize all inputs received from the User. 

---
