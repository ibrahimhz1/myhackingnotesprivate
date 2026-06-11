---
title: Manage Process in Linux
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
## Jobs

- jobs will show active jobs

## bg

- resume jobs to the background

## fg

- resume job to the foreground

---

```
# sleep 30s
Ctrl+Z -> stops the process
```

## How to resume specific jobs

```
# fg %<job-id>
# bg %<job-id> 
```

---

# Nice value

> niceness scale goes from -20 to 19. The lower the number, more priority that task gets.

Example

```
# sudo nice -n 5 process
# sudo nice -n -10 sleep 30s
```

## nohup

> if you want your process keep running even after closing your terminal, you can use nohup

```
# nohup process & 
# nohup ./script.sh &

# nohup process > /dev/null 2>&1 &
# nohup ./script.sh > /dev/null 2>&1 &
```

> nohup command will create a nohup.out file for logging purpose