---
title: Cron Job
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
---

> A **cron job** is **a command that schedules tasks to run on a server at a specified time or at regular intervals**. Cron jobs are often used to schedule tasks that are executed periodically.

### For creating Cron Job, we use crontab command

```
# crontab -e --> editing cron jobs file to add cron jobs
# crontab -l --> listing all current cron jobs
```

### Cron job Format

```
* * * * * <command1> && <command2> && <command3>
```

![](https://linuxhandbook.com/content/images/2020/06/crontab-explanation.png)

## Example

```
# crontab -e 
10 12 * * 3 cd /home/ibrahimhz/scripts/weeklyScript_12_10.sh
```