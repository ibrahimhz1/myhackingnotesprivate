---
id: "202607230001"
title: "MOC: Cross-Site Scripting"
date: 2026-07-23
type: moc
tags: [xss, moc]
publish: false
---

Map of content for the XSS Zettelkasten pilot — 27 case notes (one per PortSwigger lab, source-linked back to [[PortswiggerAcademy/XSS]]) and 24 concept notes (reusable techniques, extracted so they're stated once and linked from every case that uses them). This folder is a sandbox: the original lab notes are untouched and still live-published; nothing here is `publish: true` yet.

## By sink type

**Direct HTML sink (no encoding)**
- [[202606120001 Reflected XSS into an unsanitized HTML context]]
- [[202606120002 Stored XSS into an unsanitized HTML context]]

**`document.write` / DOM tag breakout**
- [[202606120003 DOM XSS via document.write into an image src attribute]]
- [[202606250003 DOM XSS by breaking out of a select and option element]]

**`innerHTML`**
- [[202606130001 DOM XSS via an innerHTML sink]]

**jQuery `$()`**
- [[202606200001 DOM XSS via a jQuery href sink using a javascript URI]]
- [[202606200004 DOM XSS via a jQuery selector sink using hashchange]]

**`eval()` / JS string context**
- [[202607010001 DOM XSS via eval on a JSON search response]]
- [[202606250001 Reflected XSS by breaking out of a JavaScript string literal]]
- [[202607050003 Reflected XSS into a JS string with single quote and backslash escaped (unsolved)]]
- [[202607050004 Reflected XSS into a JS string with angle brackets and quotes encoded, single quote escaped]]
- [[202607060001 XSS inside a JS template literal via placeholder interpolation]]

**HTML attribute breakout**
- [[202606200008 Reflected XSS by breaking out of an HTML attribute value]]
- [[202606240001 Stored XSS via an anchor href fully replaced with a javascript URI]]
- [[202607050005 Stored XSS defeating escaping via HTML-entity-encoded quote smuggling]]

**Filter / allowlist evasion**
- [[202607040001 Stored DOM XSS bypassing a single-pass angle-bracket filter]]
- [[202607040003 Reflected XSS via body onresize and an attribute-triggered resize]]
- [[202607040004 XSS via custom non-standard HTML elements]]
- [[202607050001 SVG animateTransform onbegin as an alert trigger]]
- [[202607050002 accesskey attribute XSS triggered by a keyboard shortcut]]
- [[202607080004 Reflected XSS via SVG animate setting a blocked href attribute]]
- [[202607090001 Parenthesis and quote-free JS execution inside a javascript URI]]

**CSP bypass**
- [[202607120001 Dangling-markup CSRF-token theft under a strict CSP]]
- [[202607170001 CSP bypass via report-uri header injection]]

**Impact / chaining**
- [[202607060003 Stored XSS to steal session cookies via out-of-band fetch]]
- [[202607080001 Using stored XSS to bypass CSRF protection]]

**Unresearched / open threads**
- [[202607080003 Reflected XSS with AngularJS sandbox escape without eval (unresearched)]]

## Concept glossary (reusable techniques, referenced from multiple cases)

- [[202606120004 Choosing onload vs onerror for an injected image tag]]
- [[202606130002 innerHTML never executes script tags]]
- [[202606130003 Reliable non-script event payloads for DOM and innerHTML sinks]]
- [[202606200002 javascript URI as an XSS vector when href is fully controlled]]
- [[202606200003 Bypassing javascript colon string filters]]
- [[202606200005 jQuery's dual personality in the dollar function]]
- [[202606200006 hashchange fires only on a hash transition not initial load]]
- [[202606200007 iframe onload self-trigger delivery for hash-based DOM XSS]]
- [[202606250002 Breaking out of a JS string literal via quote, semicolon, and comment]]
- [[202607010002 eval() as a JavaScript execution sink]]
- [[202607010003 Backslash escape-neutralization bypass]]
- [[202607040002 String.replace with a string argument only replaces the first match]]
- [[202607040005 Allowlists that permit unknown tags still allow arbitrary attributes]]
- [[202607050006 HTML entities decode before use in an inline event handler's JS context]]
- [[202607060002 Template literal interpolation executes without breaking the backticks]]
- [[202607060004 Exfiltrating data via fetch and an out-of-band collaborator]]
- [[202607080002 XSS defeats CSRF defenses because the victim's browser supplies both the token and the session]]
- [[202607080005 SVG animate can set attributes a static filter blocks directly]]
- [[202607080006 Brute-forcing an unknown tag or attribute allowlist with ffuf]]
- [[202607090002 Comma operator plus onerror and throw calls a function without its name or parens]]
- [[202607090003 javascript URIs are URL-decoded after HTML parsing before JS execution]]
- [[202607120002 Dangling markup captures the rest of the page as an unterminated attribute value]]
- [[202607170002 script-src-elem overrides script-src for script elements]]
- [[202607170003 A CSP directive is only as strong as its own value's input sanitization]]

## Recurring theme across this whole set

A striking number of these bypasses reduce to the same shape: **a defense checks one representation of the input (raw string, static attribute, first pass through a replace) while the browser or server acts on a different, later representation (decoded, dynamically-set, re-scanned) of the same value.** See [[202607010003 Backslash escape-neutralization bypass]], [[202607050006 HTML entities decode before use in an inline event handler's JS context]], [[202607090003 javascript URIs are URL-decoded after HTML parsing before JS execution]], and [[202607080005 SVG animate can set attributes a static filter blocks directly]] — four different mechanisms, one recurring root cause worth checking for by default on any new target.
