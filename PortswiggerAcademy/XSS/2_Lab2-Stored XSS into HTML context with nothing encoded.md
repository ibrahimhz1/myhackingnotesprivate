---
title: Lab2-Stored XSS into HTML context with nothing encoded
date: 2026-06-12
tags:
  - 
publish: true
draft: false
description: ""
---
This lab contains a stored cross-site scripting vulnerability in the comment functionality.
To solve this lab, submit a comment that calls the `alert` function when the blog post is viewed.

Explore the application and understand it

**Basic understanding**: 

---
Type of Application: Blogging Platform
Users can add comments for the posts

---

![[Pasted image 20260612012259.png]]

Fields where user can submit inputs: 
1. comment field
2. name field
3. email field
4. website field

Now, lets fill the details and post an comment. 
![[Pasted image 20260612012759.png]]

![[Pasted image 20260612012822.png]]

![[Pasted image 20260612012901.png]]
>The comments are displayed here. 

Lets check the page source: 
![[Pasted image 20260612013422.png]]

>Three fields from user inputs are shown in the post's comment section: 
1. Comment field
2. Name field 
3. website field

>Lets inject the payloads on all these 3 fields and check which one triggers XSS. 

Start with simple payloads: 
```
<script>alert('xss')</script>
```

![[Pasted image 20260612013841.png]]

![[Pasted image 20260612013930.png]]

![[Pasted image 20260612013944.png]]

![[Pasted image 20260612014032.png]]
>The xss payload in the name field is just displayed in the page. 

Lets check the page source to find the sink
![[Pasted image 20260612014158.png]]
>payload string is inserted as it is without any sanitization. 

but the **Username** field is sanitized properly with HTML-encoding 
![[Pasted image 20260612014322.png]]

>Since the User inputs are not sanitized, the payloads injected into the HTML sink and XSS is executed

### Solution: 
Always validate the User input and sanitize it before it is processed. 

---
