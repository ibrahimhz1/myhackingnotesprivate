---
title: Nuclei Scan Output
date: 2026-07-23
tags:
  - thm
  - nahamstore
  - nuclei
  - scan
publish: false
draft: false
description: "Nuclei vulnerability scan pipeline and results for nahamstore.thm"
---

# Nuclei Scan

## GoSpider -> Nuclei (Full Vulnerability Scan Pipeline)

### Step 1: Crawl and extract all URLs with parameters

```bash
~/go/bin/gospider -s http://nahamstore.thm -o out/ -d 3 --js | \
  grep "=" | \
  grep -v ".js\|.css\|.png\|.jpg" | \
  sort -u > all_urls.txt
```

### Step 2 (Alternative): Authenticated crawl via Burp

```bash
~/go/bin/gospider -s http://nahamstore.thm -o out/ \
  -t 30 -c 10 -d 3 \
  --cookie "token=f4b66cd1a821968c517c945f4d0d851b; session=fc693089480001fa764e1e5f72eae2c2" \
  --js \
  -p http://127.0.0.1:8080 | \
  grep "=" | \
  grep -v ".js\|.css\|.png\|.jpg" | \
  sort -u > all_urls.txt
```

### Step 3: Run Nuclei against all discovered URLs

```bash
nuclei -l all_urls.txt \
  -t ~/Tools/nuclei-templates/ \
  -H "Cookie: session=TOKEN" \
  -o nuclei_results.txt
```
