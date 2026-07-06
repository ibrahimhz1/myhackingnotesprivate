---
title: 8_Lab8-Stored XSS into anchor href attribute with double quotes HTML-encoded
date: 2026-06-24
tags:
  - 
publish: true
draft: false
description: ""
---
---

This lab contains a stored cross-site scripting vulnerability in the comment functionality. To solve this lab, submit a comment that calls the `alert` function when the comment author name is clicked.

---

Explore the application and understand it : 

**Basic understanding**: 
Type of Application: Blogging Platform
Users can add comments for the posts

---

>Add comments on the post

![[Pasted image 20260625200527.png]]

![[Pasted image 20260625200539.png]]

Click on "Back to blog" link. 

![[Pasted image 20260625200618.png]]

>The comments were added for the post 

It looks like the name is highlighted with colour. It looks like hyperlink

Lets check the page source: 

![[Pasted image 20260625200801.png]]

>The `name` text is wrapped around the `<a>` tag with `website` value in its `href` attribute

![[Pasted image 20260625200934.png]]

>The input type for the website is text, So I am assuming there is no regex pattern matching for checking the URL pattern, or there is no input validation

Lets try `javascript` URI to call the `js` code 

### Payload Execution: 

![[Pasted image 20260625201155.png]]

>Explanation: 
	the `javascript` URI calling the alert() function

![[Pasted image 20260625210709.png]]

>See the page source 

![[Pasted image 20260625210745.png]]

>So, the `href` attribute's value is completely replace with the malicious xss payload input 

in this case the `href` value is controlled fully, 

I tried to break out of the `href` attribute, but i cannot because the `"` (double quotes) is HTML encoded. So could not able to break out of the attribute. That's why I chosen the `javascript` URI payload to fully replace the `href` value

--- 

