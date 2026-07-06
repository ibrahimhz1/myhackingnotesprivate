---
title: 10_Lab10-DOM XSS in document.write sink using source location.search inside a select element
date: 2026-06-25
tags:
  - 
publish: true
draft: false
description: ""
---
---

This lab contains a DOM-based cross-site scripting vulnerability in the stock checker functionality. It uses the JavaScript `document.write` function, which writes data out to the page. The `document.write` function is called with data from `location.search` which you can control using the website URL. The data is enclosed within a select element.

To solve this lab, perform a cross-site scripting attack that breaks out of the select element and calls the `alert` function.

---

Explore the application and understand it : 

**Basic understanding**: 
Type of Application: Ecommerce Platform 
Users can check the stock of the product by selecting the option by dropdown

---


![[Pasted image 20260701221432.png]]


![[Pasted image 20260701221505.png]]

>Check the page source 

![[Pasted image 20260701221601.png]]

>The Product page have the `<script>` 

```js
<script> 
	var stores = ["London","Paris","Milan"]; 
	var store = (new URLSearchParams(window.location.search)).get('storeId'); 
	document.write('<select name="storeId">'); 
	if(store) { 
	document.write('<option selected>'+store+'</option>'); 
	} 
	for(var i=0;i<stores.length;i++) { 
		if(stores[i] === store) { 
			continue; 
		} 
		document.write('<option>'+stores[i]+'</option>'); 
	} 
	document.write('</select>'); 
</script>
```


>In the 2nd line, from the URL parameter `storeId` value is fetched and stored in the variable `store`

>but normally when we open the page by clicking the product picture from Home page, it does not use the `storeId` in the URL 

### Code Explanation

>Step 1 : create list named as `stores` with 3 city names 
>Step 2 : get value of `storeId` parameter from the URL and store in the variable `store`
>Step 3 : create an `<select>` tag DOM element 
>Step 4 : if `store` exists, means if there is `storeId` passed in the URL, then create an `<option>` tag DOM element with the `storeId` value fetched form the URL 
>Step 5 : then loop till the `stores` length
>Step 6: if `stores` list contains the same value as value fetched from `storeId`, then skip that iteration 
>Step 7: if `stores` list does not contains the same value as value fetched form `storeId`, then create the `<option>` tag DOM element
>Step 8: then create `<select>` tag DOM element, to finally close the `<select>` tag 

### Payload Planning

>The `storeId` value from the URL is directly appended inside the `<option>{storeId_value}</option>` 
>So, we will try to break out of the `<option>` and `<select>` tag and then insert an `<img>` tag with `onerror` event to call the JavaScript code. 

### Payload: 
+
```
&storeId=</option></select><img/src=x onerror="alert('xss')"/>
```

![[Pasted image 20260701223232.png]]


![[Pasted image 20260701223156.png]]


>The XSS is triggered 

>Check the source code now 

![[Pasted image 20260701223409.png]]

>As expected, we break out of the `<option>` and `<select>` tag, and then `<img>` tag is injected with `onerror` event. Thus JavaScript is executed. 

---


