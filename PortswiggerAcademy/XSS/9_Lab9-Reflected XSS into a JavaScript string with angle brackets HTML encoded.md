---
title: 9_Lab9-Reflected XSS into a JavaScript string with angle brackets HTML encoded
date: 2026-06-25
tags:
  - 
publish: true
draft: false
description: ""
---
---

This lab contains a reflected cross-site scripting vulnerability in the search query tracking functionality where angle brackets are encoded. The reflection occurs inside a JavaScript string. To solve this lab, perform a cross-site scripting attack that breaks out of the JavaScript string and calls the `alert` function.

---

Explore the application and understand it : 

**Basic understanding**: 
Type of Application: Blogging Platform
Users can search the blog posts with the search box 
Users can add comments for the posts

--- 

>search random text in the search box 

![[Pasted image 20260625212600.png]]

![[Pasted image 20260625212541.png]]

>See the page source for this resultant page 

![[Pasted image 20260625212709.png]]

>There is an JavaScript code 

```js
<script>
var searchTerms = 'sdfsdf13';
document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
</script>
```

>**Explanation:** 
>Step 1 - The searched text is assigned to `searchTerms` variable  
>Step 2 - The searched text is encoded with URI encoding
>Step 3 - An `<img>` is created with the `searchTerms` variable attached at the end of the `src` attribute's value of the `<img>` tag 
>Step 4 - Then this `<img>` tag is rendered by `document.write()` 


### Payload planning 

The payload will be URL encoded when it reaches at the `<img>` creating line 

so lets try to inject payload before that when search string is assigned to `searchTerms` variable

we can close the single quote `'` and call the JavaScript code, `alert()` function 

### Payload: 

```
'; alert('XSS');//
```

```
var searchTerms = 'sdfsdf13'; alert('XSS');//
```


![[Pasted image 20260625213637.png]]

>XSS is executed


### Vulnerability : 

>The vulnerability was in the poor code
>1. The user input was never sanitized
>2. The user input got directly injected without any input validation, in the result it leads to XSS 

### Solution: 

>Implement input validation for user input value 


---

