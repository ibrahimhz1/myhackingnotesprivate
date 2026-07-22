---
title: GoSpider Crawl Output
date: 2026-07-23
tags:
  - thm
  - nahamstore
  - gospider
  - crawl
  - recon
publish: false
draft: false
description: "GoSpider web crawler output for nahamstore.thm"
---

# GoSpider Crawl Output

## Basic Crawl

**Command:**
```bash
gospider -s http://nahamstore.thm -o output/ -c 5 -t 20
```

**Output (URLs discovered):**

| Type | URL | Status |
|------|-----|--------|
| url | http://nahamstore.thm | 200 |
| href | https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css | - |
| href | http://nahamstore.thm/ | - |
| href | http://nahamstore.thm/returns | - |
| href | http://nahamstore.thm/login | - |
| href | http://nahamstore.thm/register | - |
| href | http://nahamstore.thm/basket | - |
| href | http://nahamstore.thm/product?id=2&name=Sticker+Pack | - |
| href | http://nahamstore.thm/product?id=2 | - |
| href | http://nahamstore.thm/product?id=1&name=Hoodie+%2B+Tee | - |
| href | http://nahamstore.thm/product?id=1 | - |
| form | http://nahamstore.thm | - |
| javascript | http://nahamstore.thm/js/jquery.min.js | - |
| javascript | https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js | - |
| subdomains | http://stock.nahamstore.thm | - |
| subdomains | https://stock.nahamstore.thm | - |

## Deeper Crawl (+2 depth, JS, sitemap, robots)

**Command:**
```bash
~/go/bin/gospider -s http://nahamstore.thm -o output/ -c 5 -t 20 -d 2 --js --sitemap --robots
```

Same results as above with additional linkfinder discoveries from `jquery.min.js` (text/xml, text/plain, text/html, application/x-www-form-urlencoded patterns).

## Authenticated Crawl (3 depth, JS, with cookies)

**Command:**
```bash
gospider -s http://nahamstore.thm -o output/ -d 3 -t 15 --js \
  --cookie "session=f52365de74818eb277a6c4fc837b5218; token=74753f73d5333700f6d1308d0d271c84"
```

**Additional URLs discovered (authenticated):**

| Type | URL | Status |
|------|-----|--------|
| href | http://nahamstore.thm/account/orders | - |
| href | http://nahamstore.thm/account/settings | - |
| href | http://nahamstore.thm/account/addressbook | - |
| href | http://nahamstore.thm/logout | - |
| href | http://nahamstore.thm/account/settings/email | - |
| href | http://nahamstore.thm/account/settings/password | - |
| href | http://nahamstore.thm/account/settings/disable | - |
| url | http://nahamstore.thm/login?redirect_url=/account/settings/disable | 200 |
| url | http://nahamstore.thm/login?redirect_url=/account/settings/email | 200 |
| url | http://nahamstore.thm/login?redirect_url=/account/settings/password | 200 |
| subdomains | http://stock.nahamstore.thm | - |
| subdomains | https://stock.nahamstore.thm | - |

## Additional GoSpider Syntax Reference

### Scope-controlled crawl with subdomain discovery
```bash
~/go/bin/gospider -s http://nahamstore.thm -o out/ \
  -d 3 --js --subs \
  --whitelist "nahamstore.thm" \
  --blacklist "logout,signout,.png,.jpg,.gif,.css,.woff"
```

### Crawl a list of targets (bug bounty in-scope list)
```bash
# Put all in-scope domains in scope.txt
~/go/bin/gospider -S scope.txt -o out/ \
  -c 10 -t 30 \
  -d 2 --js --other \
  --json \
  --cookie "session=TOKEN"
```

### Route through Burp Suite
```bash
~/go/bin/gospider -s http://nahamstore.thm -o out/ \
  -d 3 --js \
  -p http://127.0.0.1:8080 \
  --cookie "session=f52365de74818eb277a6c4fc837b5218; token=74753f73d5333700f6d1308d0d271c84"
```

### Extract only URLs with parameters (XSS/SQLi targets)
```bash
~/go/bin/gospider -s http://nahamstore.thm -o out/ \
  -t 30 -c 10 -d 3 \
  --js | \
  grep "=" | \
  grep -v ".js\|.css\|.png\|.jpg" | \
  sort -u > params.txt
```
