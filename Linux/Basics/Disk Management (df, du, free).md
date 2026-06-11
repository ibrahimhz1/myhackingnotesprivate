---
title: Disk Management (df, du, free)
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
---
# df , du , free
---

# df - disk <filesystem>

> df displays the amount of disk space available on the file system containing each file name argument.

```
# df -h 
// displays the information about the file system available space
# df -h /
```

---

# du - Disk Utilization

> Summarize disk usage of the set of FILEs, recursively for directories.

```
# du -h 
# du -h /FilePath/
# du -hc /filePath/ --> c -> for total volume
```

> If you want to see even size of the directory also then use a

```
# du -ahc /filepath/ 
# du -ahc -BM /filepath/ --> BM -> shows in MegaBytes
# du -ahc -BG /filepath/ --> BG -> shows in GigaBytes
# du -ahc -BK /filepath/ --> BK -> shows in KiloBytes
```

---

# Free

> Display amount of free and used memory in the system

```
# free
# free -h --> human readable format
# free -s N --> keep refreshing memory after N sec
# free -h -s 2 --> keep refreshing every 2 seconds
# free -c N --> Exit after repeating N times 
# free -h -c 2 --> exit after showing for two seconds 
```