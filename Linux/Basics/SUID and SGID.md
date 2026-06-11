---
title: SUID and SGID
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
---

## What is SUID ?

> The special permission for the user.

> A file set with SUID, always executes as the user who owns the file, regardless of the user executing the command.

> show as ‘s’ or ‘S’

## How to set SUID ?

```
# chmod u+s <filename>
```

## How to remove SUID ?

```
# chmod u-s <filename>
```

---

## What is SGID ?

> If set on the file, it allows the file to be executed as the group that owns the file

> If set on a directory, any files created in the directory will have their group ownership set to that of the directory owner.

> It also especially useful for directories that are often used in collaborative efforts between members of a group. Any member of the group can access any new file.

## How to set SGID ?

```
# chmod g+s <filename>
# chmod g+s <directory>
```

### Practical Example

```
// Login as ibrahimhz user 
// ibrahimhz creates an directory where his many class mates can create many files init

# mkdir collabs
# chmod 777 collabs --> giving all permissions to read and write and execute
# chmod g+s <directory_name> 

// now login as another user
# cd collabs
# touch myfile1

// now login to ibrahimhz
# cd collabs
# ls -l 
// see the file ownership details
```