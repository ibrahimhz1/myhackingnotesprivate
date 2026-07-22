---
title: XSS
date: 2026-07-23
tags:
  - thm
  - nahamstore
  - xss
  - gospider
  - dalfox
  - vulnerability
publish: false
draft: false
description: "XSS testing pipeline for nahamstore.thm using GoSpider + Dalfox"
---

# XSS

## GoSpider -> Dalfox (XSS Pipeline)

### Step 1: Crawl with GoSpider and extract parameterized URLs

```bash
~/go/bin/gospider -s http://nahamstore.thm -o out/ -d 3 --js \
  --cookie "session=f52365de74818eb277a6c4fc837b5218; token=74753f73d5333700f6d1308d0d271c84" \
  --whitelist "nahamstore.thm" \
  --blacklist "logout,signout,.png,.jpg,.gif,.css,.woff" | \
  grep -oP 'http?://[^ ]+\?[^ ]+' | \
  sort -u > params.txt
```

### Step 2: Pipe URLs to Dalfox

```bash
cat params.txt | dalfox pipe \
  --cookie "session=f52365de74818eb277a6c4fc837b5218; token=74753f73d5333700f6d1308d0d271c84" \
  --silence
```

### Step 3: Run Dalfox with only verified POC output

```bash
cat params.txt | dalfox pipe \
  --cookie "session=f52365de74818eb277a6c4fc837b5218; token=74753f73d5333700f6d1308d0d271c84" \
  --only-poc g \
  --silence \
  --timeout 10 \
  2>/dev/null > dalfox_confirmed.txt
```
