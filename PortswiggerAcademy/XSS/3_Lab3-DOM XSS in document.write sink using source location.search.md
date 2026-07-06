---
title: 3_Lab3-DOM XSS in document.write sink using source location.search
date: 2026-06-12
tags:
  - 
publish: true
draft: false
description: ""
---
This lab contains a DOM-based cross-site scripting vulnerability in the search query tracking functionality. It uses the JavaScript `document.write` function, which writes data out to the page. The `document.write` function is called with data from `location.search`, which you can control using the website URL.

To solve this lab, perform a cross-site scripting attack that calls the `alert` function.

Explore the application and understand it

**Basic understanding**: 

---
Type of Application: Blogging Platform
Users can add comments for the posts
The homepage have search feature (to search the blog posts)

---

Search random text : 
![[Pasted image 20260613091022.png]]

![[Pasted image 20260613091138.png]]

>Lets see the page source: 
![[Pasted image 20260613091600.png]]

>There is an script in the page: 
```js
<script> 
function trackSearch(query) { document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">'); } 

var query = (new URLSearchParams(window.location.search)).get('search'); if(query) { trackSearch(query); } 
</script>
```

Explanation: 
steps: 
>1. It gets the 'search' parameter value from `windows.location.search` through URL, assign it to variable 'query'
>2. calls the `trackSearch` method with parameter variable:`query` got from step 1 
>3. `trackSearch` method calls `document.write` method that writes an `<img>` tag in the HTML document, which includes the user entered searched text. 

Sink: 
>`document.write` with `<img>` **src** attribute is the sink for the XSS payload. 

>Searched text is used in : 
>1. inside `<h1>` tag
>2. inside the `<script>` : the searched text '**query**' is directly appended in the "**src**" attribute string of `<img>` tag 

Lets see the rendered HTML document to see, how the `<img>` is rendered: 
![[Pasted image 20260613092228.png]]

>The `<img>` is rendered : 
```
<img src="/resources/images/tracker.gif?searchTerms=abcdxyz100">
```

### Attack Strategy: 
- break out of the '**src**' attribute 
- Attack on which `<img>` Event : 'onload'
	why not 'onerror' event ? 
		since searched text cannot control the source of the `<img>` tag and only gets injected in the parameter "searchTerms", regardless of the false image address it will not error out so it will not 'onerror' event will not be triggered. 
- onload of the image event, call the alert() function to test the 'XSS' execution

### Payload: 
```
search: " onload=alert('XSS')>
```
![[Pasted image 20260613093508.png]]


![[Pasted image 20260613093530.png]]
>XSS is executed. 

Can try different types of payload: 


---
