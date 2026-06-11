---
title: Wildcards
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
## Types of Wildcards

1. - → one or more after the star
2. ? → single character
3. [ ] → range of character
4. ^ → beginning of the line
5. $ → end of the line

---

```
# ls -l file*
# ls -l *.txt
```

```
# ls -l ????dom
# ls -l file??123
// pattern
```

```
# ls -l [abc]123
// a or b or c then 123 at the end 

# ls -l *[0-9]
# ls -l *[0-5]*
```

```
# cat namelist
ram
raju
vijay
puja

# cat namelist | grep ^r
ouput: ram 
			 raju
			 
# cat namelist | grep am$
output: ram
```