---
title: Compression and Archiving
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---

## Archive files or directory

```jsx
# tar -cvf <arhivefile_name.tar> <target_directory or file>
# tar -cvf <archivefile.tar> file1 file2 file3 file4 directory1 directory2
```

## Extract from Archive or Zipped file

```jsx
# tar -xvf <archive_filename.tar>
# tar -xzvf <archive_file.tar.gz>
```

## Compress or zip Archive File

```jsx
# gzip <archive.tar>
```

## Unzip or Decompress compressed file

```jsx
# gunzip <archive.tar.gz>
```

## Archive and Zip/Compress at same time

```jsx
# tar -czvf <archive.tar.gz> <folder>
```

---

## Flags:

> c → create compress

> v → Verbose

> f → File

> x → Extract

> z → zip with compress size