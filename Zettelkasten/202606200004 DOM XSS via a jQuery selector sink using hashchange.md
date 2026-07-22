---
id: "202606200004"
title: DOM XSS via a jQuery selector sink using hashchange
date: 2026-06-20
type: case
tags: [xss, dom-xss, jquery, hashchange]
source: "[[6_Lab6-DOM XSS in jQuery selector sink using a hashchange event]]"
publish: false
---

```javascript
$(window).on('hashchange', function(){
  var post = $('section.blog-list h2:contains(' + decodeURIComponent(window.location.hash.slice(1)) + ')');
  if (post) post.get(0).scrollIntoView();
});
```

First instinct — treat this like a JS-execution sink and try to close the function call and inject code (`#xyz));alert(123);//`) — fails silently. The reason is [[202606200005 jQuery's dual personality in the dollar function]]: `$()` here is a **selector** sink, not an `eval`-like execution sink, so string-injection tricks that work against `eval()` (see [[202607010002 eval() as a JavaScript execution sink]]) don't apply — jQuery never hands the string to the JS engine as code.

The actual bug is that `$()` also acts as an HTML-element constructor whenever its argument starts with `<`. So the working payload is plain HTML, not a JS-breakout string:

```
#<img src=x onerror=alert(1)>
```

Delivery is the second half of this lab: `hashchange` only fires on a hash **transition**, not on initial page load ([[202606200006 hashchange fires only on a hash transition not initial load]]), so a victim can't just be sent a URL — it needs [[202606200007 iframe onload self-trigger delivery for hash-based DOM XSS]].

## Links
- Sink-type distinction that explains why the naive payload failed: [[202606200005 jQuery's dual personality in the dollar function]]
- Delivery mechanism required because of hashchange semantics: [[202606200006 hashchange fires only on a hash transition not initial load]], [[202606200007 iframe onload self-trigger delivery for hash-based DOM XSS]]
- Same non-script event payload family as: [[202606130003 Reliable non-script event payloads for DOM and innerHTML sinks]]
