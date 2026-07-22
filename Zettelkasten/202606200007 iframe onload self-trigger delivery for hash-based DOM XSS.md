---
id: "202606200007"
title: iframe onload self-trigger delivery for hash-based DOM XSS
date: 2026-06-20
type: concept
tags: [xss, delivery, iframe, cross-origin]
publish: false
---

Because [[202606200006 hashchange fires only on a hash transition not initial load]], delivering a hash-based DOM XSS to a victim needs something that changes the hash *after* the vulnerable page has already loaded, from outside the victim's own action.

**Attempt that fails** — attacker page tries to reach across origins into the iframe via JS:

```html
<iframe src="https://vulnerable-site/#junk" id="victim"></iframe>
<script>
  setTimeout(function(){
    document.getElementById('victim').src = 'https://vulnerable-site/#<payload>';
  }, 500);
</script>
```
Blocked: this is the attacker's JS reaching *into* a cross-origin iframe to control it — the browser's cross-origin isolation stops that.

**Working payload** — put the trigger on the iframe element itself, reacting to its own load event rather than reaching inside it:

```html
<iframe src="https://vulnerable-site/#" onload="this.src+='<img src=x onerror=alert(1)>'"></iframe>
```

The distinction that makes this work: `onload` lives on the iframe element **in the attacker's own DOM** — it's the attacker's page reacting to a load event on an element it owns, not attacker JS reaching across the origin boundary to manipulate the iframe's internals. Sequence: iframe loads with empty hash → its own `onload` fires → `this.src +=` appends the payload → the hash changes → `hashchange` fires *inside* the iframe, in the victim origin's context → the vulnerable code runs the payload.

This single self-contained `<iframe>` tag — no separate script block reaching in — is the canonical delivery pattern PortSwigger's lab solution uses, and it generalizes to any hashchange-only DOM XSS.

## Links
- Problem this solves: [[202606200006 hashchange fires only on a hash transition not initial load]]
- Vulnerability being delivered: [[202606200004 DOM XSS via a jQuery selector sink using hashchange]]
