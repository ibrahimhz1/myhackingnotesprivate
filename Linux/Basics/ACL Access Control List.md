---
title: ACL Access Control List
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
> It allows you to give more specific set of permissions to a file or directory without changing the base ownership and permissions.

> Command : setfacl & getfacl

### getfacl = get file access control list

### setfacl = set file access control list

```
# getfacl <filename>
```

## Commands

> For adding permission for user:

```
# setfacl -m u:<username>:rwx <target file>
```

> For adding permission for group:

```
# setfacl -m g:<group>:rwx <target file>
```

> To remove a specific entry:

```
# setfacl -x u:<user> <target file>
```

> To remove all entries

```
# setfacl -b <target file>
```

> To set permission to the entire files within the directory

```
# setfacl -Rm u:<user>:rw <target directory name>
```