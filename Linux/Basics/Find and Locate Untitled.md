---
title: Find and Locate Untitled
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
# Find

### Basics

```jsx
# find <location where to search> -name <filename or directory to search>
# find /home/ibrahimhz -name myfile
// find with the type file
# find /home/ibrahimhz -type f -name myfiles
// find with the type directory
# find /home/ibrahimhz -type d -name myfiles
```

## Find with the file size

```jsx
# find /path -size 48M
// M - MB, K - KB, G - GB, c - bytes
```

## Find file and directories only of [specific type] in a given path

```jsx
# find /path/ -type f
// f - file
// d - directory
// l - Symbolic link
// b - block device
// s - socket
```

## Find files and directories based on the name

```jsx
# find /path -name filename
```

## How to ignore case in filename while searching file

```jsx
# find <path> -iname <filename>
```

## Search files for given user only

```jsx
# find <path> -user <username>
```

## Search files or directory in only particular directory only without recursive searching

```jsx
# find /home/ibrahimhz -maxdepth 1 -name kali
```

## Search based on i-node number of file

```java
// to create hardlink for file
# ln <filename> <linkfilename>
// to ls with i-node number
# ls -li
# find <path> -inum <inumber>
```

## Search file based on number of links

```java
# find <path> -maxdepth 1 -links <no.of.links>
```

## Search files based on their permissions

```java
# find <path> -perm /u=r
# find <path> -perm 777
```

## Search all files which start with letter a

```java
# find <path> -iname a*
```

## Search all files which are modified / created last.txt file

```java
# find <path> -newer lastfile.txt
```

## Search empty file in path

```java
# find . -maxdepth 1 -empty
# find . -maxdepth 1 -empty -type f
# find . -maxdepth 1 -empty -type d
```

## Search all the empty files in a given directory and at the same time delete them ?

```java
# find <path> -maxdepth 1 -empty -exec rm {} \\;
```

## Search all files whose size are between 1 - 15MB

```java
# find <path> -size +1M -size -15M
```

## Search 15 days old files

```java
# find <path> -mtime 15
```

## Check mlocate is installed

```jsx
# rpm -qa | grep mlocate
// to install mlocate
# yum install mlocate
```

## Locate

```jsx
# sudo updatedb
# locate <filename>
// to locate file and return the count of the result
# locate -c myfile.txt
```

## How to find largest or Biggest file in Linux directory

### 3 ways to find :

1. Using ‘find’ command
2. Using ‘du’ command → disk utilization (du)
3. Using ‘ls’ command

## find command:

```
# find . -maxdepth 1 -type f -exec ls -lh {} \\; | sort -k5rh | head -1 | awk '{print $NF}' | awk -F/ '{print $NF}'
```

## du command:

```
# du -sh * | sort -rh | head -1
```

## ls command

```
# ls -lSh | head -1
```