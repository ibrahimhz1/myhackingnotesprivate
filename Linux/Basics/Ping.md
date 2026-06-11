---
title: Ping
date: 2026-06-11
tags:
  - 
publish: false
draft: false
description: ""
---
---

## What ping can do ?

- check network connectivity
- check internet connection
- check network interface card
- check latency on network
- DNS resolution

```
# ping <hostname>
# ping <ip-address>
# ping -c 3 <hostname/ip> (for three times only)
# ping -i 2 (interval to send packets)
# ping -c 5 -q www.google.com (only give you summary)
# ping -f www.google.com (to send packets as fast as possible to test the network performance)
# ping -s 500 (change size of the packet)
# ping -w 10 (to stop printing after 10 sec)
# ping -D www.google.com (also print the timestamp to remember the time)
# ping -a www.google.com (audible)
```