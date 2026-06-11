---
title: Piping
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
1. uniq
2. Tail
3. more
4. head
5. wc
6. sort
7. less
8. tee
9. args

---

```java
# ls -1 /etc | wc -l
# ls /etc | wc -w
```

```java
# cat file1 file2 | sort 
```

```java
# cat file1 | sort | uniq
```

```java
# cat -n /etc/passwd | head -20 | tail -10
```

```java
# cat /etc/passwd | more
```

```java
# cat /etc/passwd | less
```

```java
# ls -l /home/ibrahimhz/ | tee homefile.txt
# ls -l /home/ibrahimhz/ | tee homefile.txt | wc -l
```

```java
# xargs is used to give command line argument to commands
# ls | xargs echo
# ls | xargs echo "hi"
```

## Task: Take file names from filename.txt file and create those files

```java
# cat filenames.txt | xargs touch ./
```