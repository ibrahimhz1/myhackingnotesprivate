---
title: CUT command
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
---

> to deal with characters

## To get first characters of each line

```
# cut -c1 namelist.txt
```

## To get multiple characters of lines

```
# cut -c1,3,5 namelist.txt --> get character 1,3,5 of each line 
```

## To get range of characters of lines

```
# cut -c1-5 namelist.txt --> get range of characters from 1 to 5
```

## To get column

```
# cut -d, -f 2 filename.csv

-d -> delimiter
-f -> field number
```

## Replace delimiter to another

```
# cut -d, -f 1- filename.txt --output-delimiter=" | "

-f 1- --> means full field from 1 to end
```

## Combining cut with awk command to filter out

```
#  ls -l | awk '{print $NF}' | cut -c1-4
```