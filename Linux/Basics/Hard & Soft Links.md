---
title: Hard & Soft Links
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
### Links are pointer to the file

### Types of Links in Linux

1. Soft Links
2. Hard Links

---

## Soft Link

> Link will be removed if original file removed or deleted

```
# ln -s <path of the file to link> <link name>
```

## Hard Link

> Renaming, Deleting or removing the file will not effect the link

```
# ln <path of the file to link> <link name>
```