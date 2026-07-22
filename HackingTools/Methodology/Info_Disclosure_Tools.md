---
title: Info Disclosure & Content Analysis Tools
date: 2026-07-23
tags:
  - info-disclosure
  - nikto
  - nuclei
  - waybackurls
  - gospider
  - wpscan
  - recon
  - methodology
publish: false
draft: false
description: "Tools for information disclosure, content analysis, and CMS fingerprinting"
---

# Info Disclosure & Content Analysis Tools

## Nikto

```bash
# Basic scan
nikto -h http://target.com

# With authentication
nikto -h http://target.com -id admin:password

# SSL
nikto -h https://target.com -ssl

# Specific plugins
nikto -h http://target.com -Plugins "headers,cookies,favicon"

# Save output
nikto -h http://target.com -o nikto.html -Format htm
```

## Nuclei

```bash
# Full recon scan
nuclei -u http://target.com \
  -t technologies/ \
  -t exposures/ \
  -t misconfiguration/ \
  -t cves/ \
  -o nuclei_output.txt

# Just exposures (info disclosure)
nuclei -u http://target.com -t exposures/

# Just misconfigurations
nuclei -u http://target.com -t misconfiguration/

# Specific tags
nuclei -u http://target.com -tags apache,php,nginx,wordpress

# Severity filter
nuclei -u http://target.com -severity critical,high,medium
```

## Waybackurls / Gau - Historical URL Discovery

```bash
# waybackurls - fetch URLs from Wayback Machine
waybackurls target.com | tee wayback_urls.txt

# gau (getallurls) - fetches from multiple sources
gau target.com | tee gau_urls.txt

# Filter for interesting files
cat gau_urls.txt | grep -E "\.(php|asp|aspx|jsp|json|xml|sql|bak|old|zip|tar|log)$"

# Find parameters in historical URLs
cat wayback_urls.txt | grep "?" | sort -u
```

## GoSpider / Hakrawler - Web Crawlers

```bash
# gospider - fast web spider
gospider -s http://target.com \
  -o gospider_output \
  -c 10 \
  --depth 5 \
  --other-source \
  --include-subs

# hakrawler
echo http://target.com | hakrawler -depth 3 -plain | tee hakrawler_output.txt

# With authentication
echo http://target.com | hakrawler -cookie "session=xyz" -depth 3
```

---

## CMS-Specific Recon Tools

### WordPress

```bash
# wpscan - comprehensive WP scanner
wpscan --url http://target.com \
  --enumerate u,p,t,cb,dbe \
  --api-token YOUR_API_TOKEN \
  -o wpscan_output.txt

# Enumerate users only
wpscan --url http://target.com --enumerate u

# Check for vulnerable plugins
wpscan --url http://target.com --enumerate vp

# Password brute force
wpscan --url http://target.com \
  --usernames admin \
  --passwords /usr/share/wordlists/rockyou.txt
```

### Joomla

```bash
joomscan -u http://target.com
```

### Drupal

```bash
droopescan scan drupal -u http://target.com
```
