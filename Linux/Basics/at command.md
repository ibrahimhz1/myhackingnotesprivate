---
title: at command
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
---

> Can be used to schedule job only once
> 
> Name of the Service is ‘atd’

## Syntax:

```
at <HH:MM>
atq    --> List ‘at’ jobs
atrm <at_number> -->  Remove a job
```

## Examples

```
# at 12:30
> echo "hello"
> ./makefile.sh
> ctrl + d --> to exit

# atq --> to list atq jobs

# atrm <at number> --> to delete specific at job
```

## Other Examples

```
	# at 4:45 AM 10122021
	# at 4PM +4 days
	# at now +5 hours
	# at 9:00 AM Mon
	# at 9:00 AM tomorrow
	# at 10:00 AM next month
	# at 10:00 AM Nov 30
```