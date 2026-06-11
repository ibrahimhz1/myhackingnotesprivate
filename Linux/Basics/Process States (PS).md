---
title: Process States (PS)
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
```
# ps
```

### syntax:

```
ps [option]
ps - show the process of current shell

PID -> unique Process ID
TTY -> terminal type of user logged in to
TIME -> amount of CPU in min and sec that process has been running
CMD -> name of the command that launched the process
```

## To see all running process

```
# ps -e
# ps -A
```

## To see in full format

```
# ps -ef
```

## To see all running process in BSD format

```
# ps aux
```

## To see the process by username

```
# ps -u <username>
# ps -G <group name> 
```

## To see the process tree

```
# ps -ejH
```