---
title: API Endpoint Discovery
date: 2026-07-23
tags:
  - api
  - endpoints
  - kiterunner
  - ffuf
  - recon
  - methodology
publish: false
draft: false
description: "Tools and techniques for discovering API endpoints"
---

# API Endpoint Discovery

## Kiterunner - API Endpoint Discovery

Purpose-built for API recon. Far better than regular directory fuzzers for APIs.

### Basic scan with API-specific wordlist

```bash
kr scan http://target.com/api -w routes-large.kite
```

### With text wordlist

```bash
kr brute http://target.com/api \
  -w /usr/share/seclists/Discovery/Web-Content/api/api-endpoints.txt
```

### With authentication

```bash
kr scan http://target.com/api \
  -w routes-large.kite \
  -H "Authorization: Bearer <token>"
```

## FFUF for API Fuzzing

### API version fuzzing

```bash
ffuf -u http://target.com/api/FUZZ/users \
  -w versions.txt   # v1, v2, v3, beta, dev, test, internal
```

### API endpoint fuzzing

```bash
ffuf -u http://target.com/api/v1/FUZZ \
  -w /usr/share/seclists/Discovery/Web-Content/api/objects.txt
```

### HTTP method fuzzing on found endpoints

```bash
ffuf -u http://target.com/api/v1/users \
  -X FUZZ \
  -w methods.txt   # GET,POST,PUT,DELETE,PATCH,OPTIONS,HEAD
```
