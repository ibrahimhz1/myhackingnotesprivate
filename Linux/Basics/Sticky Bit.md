---
title: Sticky Bit
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
---

> Restricted Deletion Flag or Sticky bit = Sticky Bit

> Only file owner, directory owner can delete or rename a file or directory

> show as ‘t’ or ‘T’

---

## Example

```
// Make an collabrative directory where group members can store their config files 
// Each user have privacy, means that not any user can modify file of any user or delete it
// ibrahimhz and geek user is in same primary group called coders

// login as ibrahimhz
# mkdir collabs
# chmod g+w collabs --> giving write permission to group
# cd collabs
# touch ibrahimfile.txt

// login as geek user
# cd collabs
# rm ibrahimfile.txt
<give yes to prompt> --> the file is deleted because group has full access rwx
// to prevent this problem sticky bit is used 

// login as ibrahimhz
# chmod +t collabs
# cd collabs
# touch ibrahimhzfile

// login as geek user
# cd collabs
# rm ibrahimhzfile.txt
<give yes to prompt> --> the file is not deleted because sticky bit is set by ibrahimhz user, only ibrahimhz user can delete or modify the files in the directory
```