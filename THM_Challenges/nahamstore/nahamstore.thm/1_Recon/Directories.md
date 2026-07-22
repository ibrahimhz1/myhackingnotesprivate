---
title: Directory & File Brute Force
date: 2026-07-23
tags:
  - thm
  - nahamstore
  - recon
  - feroxbuster
  - ffuf
  - gobuster
publish: false
draft: false
description: "Directory and file discovery techniques used on nahamstore.thm"
---

# Directory & File Brute Force

## Feroxbuster

**Feroxbuster directory and files scan:**

```bash
feroxbuster -u http://nahamstore.thm \
  -a "Mozilla/5.0 (Windows NT 10.0; Win64; x64)" \
  -r -x php,txt,js,py,java,pdf,docx \
  --output nahamstore.thm/ferox-scan.txt
```

**URL paths found:**

- `search-products` → 200 GET (3351 bytes)
- `search` → 200 GET (3138 bytes)
- `register` → 200 GET (3099 bytes)
- `login` → 200 GET (3628 bytes)
- `returns` → 200 GET (22 bytes)
- `product/picture` → 200 GET (97176 bytes)
- `basket` → 200 GET (2465 bytes)
- `uploads` → 301 GET → `http://127.0.0.1/uploads/`
- `staff` → 200 GET (2287 bytes)
- `robots.txt` → 200 GET (13 bytes)
- `js/jquery.min.js` → 200 GET (4254 bytes)

## FFUF

**FFUF (fuzz) to discover directories on port 8000:**

URL paths found:
- `admin`
- `robots.txt`

## GoBuster

### Directory mode

```bash
gobuster dir \
  -u http://target.com \
  -w /usr/share/seclists/Discovery/Web-Content/common.txt \
  -x php,html,txt,bak \
  -t 50 \
  -o gobuster_output.txt
```

### DNS mode - subdomain enumeration

```bash
gobuster dns \
  -d target.com \
  -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt \
  -t 50
```

### Vhost mode

```bash
gobuster vhost \
  -u http://target.com \
  -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```

## Feroxbuster Cheat Sheet

### Basic scan
```bash
feroxbuster -u http://target.com \
  -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt \
  -x php,html,js,txt,bak,old,zip,sql,json,xml,conf,config,log,backup \
  -t 50 \
  --depth 3 \
  -o ferox_output.txt
```

### Authenticated scan (with cookies)
```bash
feroxbuster -u http://target.com \
  -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt \
  -H "Cookie: session=your_session_cookie" \
  -t 50 \
  --depth 3
```

### Recursive scan with status filter
```bash
feroxbuster -u http://target.com \
  -w /usr/share/seclists/Discovery/Web-Content/raft-large-files.txt \
  --filter-status 404,403 \
  --depth 4 \
  -t 40
```

## FFUF Cheat Sheet

### Directory fuzzing
```bash
ffuf -u http://target.com/FUZZ \
  -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt \
  -mc 200,301,302,403 \
  -o ffuf_dirs.json
```

### File fuzzing
```bash
ffuf -u http://target.com/FUZZ \
  -w /usr/share/seclists/Discovery/Web-Content/raft-large-files.txt \
  -mc 200,301,302 \
  -o ffuf_files.json
```

### Extension fuzzing
```bash
ffuf -u http://target.com/indexFUZZ \
  -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt
```

### Subdirectory fuzzing (after finding /api/)
```bash
ffuf -u http://target.com/api/FUZZ \
  -w /usr/share/seclists/Discovery/Web-Content/api/objects.txt
```

## Wordlist Priority Order

### SecLists - most important collection
`/usr/share/seclists/Discovery/Web-Content/`

### Specific wordlists

| Wordlist | Use |
|----------|-----|
| `raft-large-directories.txt` | General directories |
| `raft-large-files.txt` | General files |
| `directory-list-2.3-medium.txt` | Classic dirbuster list |
| `common.txt` | Quick common files |
| `api/objects.txt` | API endpoints |
| `api/api-endpoints.txt` | API paths |
| `big.txt` | Large comprehensive list |

### For backup files specifically:
`/usr/share/seclists/Discovery/Web-Content/quickhits.txt`
