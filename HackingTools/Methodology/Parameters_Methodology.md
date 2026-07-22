---
title: Parameter Discovery Methodology
date: 2026-07-23
tags:
  - parameters
  - recon
  - burp
  - methodology
publish: false
draft: false
description: "Methodology for discovering hidden parameters in web applications"
---

# Parameter Discovery Methodology

## Param Miner (Burp Extension)

Right-click any request → Extensions → Param Miner → Guess params

## Where Parameters Exist

| Location | Example |
|----------|---------|
| URL query string | `?id=1&sort=name` |
| POST body (form) | `username=admin&password=test` |
| POST body (JSON) | `{"id":1,"role":"user"}` |
| POST body (XML) | `<id>1</id>` |
| HTTP Headers | `X-User-ID: 1`, `X-Role: admin` |
| Cookies | `session=abc; user_id=1; role=user` |
| Path parameters | `/user/1/profile/settings` |
| Fragment identifiers | `#/admin/dashboard` |
| GraphQL variables | `query{ user(id:1){ name } }` |
| WebSocket messages | `{"action":"getUser","id":1}` |
| Multipart form data | file upload with extra fields |

## Methodology Step by Step

### 1A: Burp Suite Passive Crawl

Every request that passes through Burp is logged. Browse the entire application manually.

### 1B: Active Parameter Guessing

Use Param Miner to guess hidden parameters on each endpoint.

### 2: Analyze Responses

Look for differences in:
- Response length
- Response status code
- Response time
- Error messages revealing parameter names
- Reflection in HTML/JS
