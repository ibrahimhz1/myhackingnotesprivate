---
title: Kill command
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
> Kill command is used to terminate process manually

> Syntax:

```
kill [option] [PID]
```

option = signal name or number

PID = Process ID

---

## To see all signal names

```
# kill -l
```

```
# kill PID
# kill -1 PID (to restart the process)
# kill -2 PID (interrupt from keyboard like Ctrl+C)
# kill -9 PID (forcefully terminate the process)
# kill -15 PID (kill process gracefully)
```

## Example

```
# sleep 30s
# ps -ef | grep -i sleep
# kill <PID of sleep process>
```

```
# sleep 40s
# ps -ef | grep -i sleep
# kill -9 <PID of sleep process>
```

```
# sleep 40s
# ps -ef | grep -i sleep
# kill -2 <PID of sleep process>
```

```
# sleep 40s
# ps -ef | grep -i sleep
# kill -15 <PID of sleep process>
```