---
title: Linux File Permissions
date: 2026-06-11
tags:
  - 
publish: false
draft: false
description: ""
---
## Permission types

1. read
2. write
3. execute

---

## Types of Permission levels

1. u → current user
2. g → group users
3. o → other than group users
4. a → all users

---

## How to change permission

```
# chmod u+r <filename> (adding read permission)
# chmod u-r <filename> (removing read permission)

# chmod ugo+r <filename> (adding read permission to user,group,other)
# chmod a+r   <filename> (adding read permission to all users)
# chmod a+rwx <filename> (adding read,write,execute permission to all users)
```

---

# Numeric mode

|Number|Permission type|Symbol|
|---|---|---|
|0|No permission|- - -|
|1|Execute|- - x|
|2|Write|- w -|
|3|Execute + Write|- w x|
|4|Read|r - -|
|5|Read + Execute|r - x|
|6|Read + Write|r w -|
|7|Read + Write + Execute|r w x|

## chmod 765 <filename>

7 → for user

6 → for group

5 → for other users

---

# Change Ownership

```
# chown -c <which user to assign ownership> <filename>
// needs admin previlages
```

# Change Group ownership

```
# chgrp -c <which group to assign ownership> <filename>
// needs admin previlages
```