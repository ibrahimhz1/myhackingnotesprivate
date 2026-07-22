---
title: Subdomains & Vhosts
date: 2026-07-23
tags:
  - thm
  - nahamstore
  - subdomains
  - vhost
  - ffuf
  - recon
publish: false
draft: false
description: "Subdomain and virtual host enumeration for nahamstore.thm"
---

# Subdomains & Vhosts

## Subdomain Fuzzing

```bash
# Fuzz subdomain on port 80
ffuf -u http://FUZZ.nahamstore.thm \
  -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

**Found subdomains (port 80):**
1. marketing
2. stock

```bash
# Fuzz subdomain on port 8000
ffuf -u http://FUZZ.nahamstore.thm:8000 \
  -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

**Found subdomains (port 8000):**
1. shop
2. marketing
3. stock

## Vhost Fuzzing

```bash
# General vhost fuzzing
ffuf -u http://target.com \
  -H "Host: FUZZ.target.com" \
  -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt \
  -mc 200,301,302 \
  -fs <default_response_size>
```

### Vhost by changing Hostname

```bash
ffuf -u http://nahamstore.thm \
  -H "Host: FUZZ.nahamstore.thm" \
  -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt \
  -fw 125
```

**Found vhosts:**
1. shop
2. marketing
3. stock

## Subdomain Enumeration via SSRF

Subdomain enumeration was also performed via SSRF (screenshots captured in original CherryNotes).
