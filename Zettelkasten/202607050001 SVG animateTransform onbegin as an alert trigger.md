---
id: "202607050001"
title: SVG animateTransform onbegin as an alert trigger
date: 2026-07-05
type: case
tags: [xss, reflected-xss, svg, filter-bypass]
source: "[[16_Lab16-Reflected XSS with some SVG markup allowed]]"
publish: false
---

Filter allows some SVG markup through. `<animateTransform>`'s `onbegin` fires as soon as the animation starts — at insertion time, not tied to any resource load — making it a reliable trigger when direct event handlers on ordinary tags are stripped but SVG animation elements are not:

```html
<svg><animateTransform onbegin=1>
```

Same underlying mechanism as the `<animate onbegin>` entry in [[202606130003 Reliable non-script event payloads for DOM and innerHTML sinks]] — restated here because the constraint (SVG markup specifically allowed, other tags blocked) is what makes it the *only* viable option rather than just the most reliable one.

## Links
- General payload family: [[202606130003 Reliable non-script event payloads for DOM and innerHTML sinks]]
