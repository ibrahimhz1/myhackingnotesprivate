---
title: dmesg - Diagnostic Message
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
> Linux utility that displays kernel-related messages retrieved from the kernel ring buffer.

> The ring buffer stores information about hardware, device driver initialization, and messages from kernel modules that take place during system startup.

```
# dmesg
# dmesg -HTx --> (human readable, timestamp, decode)
# dmesg | more
# dmesg | less
# dmesg | head -10
# dmesg | tail -10
```

## dmesg | grep <search string>

> You can Search related to

- memory
- usb
- sda
- ram
- tty
- error

```
# dmesg -HTx | grep usb
# dmesg -HTx | grep ram
```

## Live monitoring

```
# dmesg -w
```

## To clear the ring buffer

```
# dmesg -c 
```

## To filter the messages based on different level

```
# dmesg -l emerg
# dmesg -l alert
# dmesg -l crit
# dmesg -l err
# dmesg -l warn
# dmesg -l notice
# dmesg -l info
# dmesg -l debug
```