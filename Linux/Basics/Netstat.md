---
title: Netstat
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
---

> Netstat is a command line utility that displays

- network connections for TCP, UDP
- Routing Tables
- A number of network interface
- Network protocol statistics

## To identify number of connection on a given port or IP

```
# netstat -putan | grep <port/ip>
```

## Netstat commands

### ( t → TCP , u → UDP , n → numerical addr , l → listening port , p → PID of the program )

```
// To see all the sockets
# netstat -a

// List all TCP ports
# netstat -at
# netstat -atl --> only list the listening ports

// List all TCP v6 ports
# netstat -6at

// List all the UDP ports
# netstat -au
# netstat -au6

// List all listening ports
# netstat -l 

// List in numberical address
# netstat -ln 

// To view the PID of the program of connection
# netstat -p 
# netstat -atlnp 

// To view the routing table
# netstat -r

// To get the list of all interfaces
# netstat -i

// which port a process is using ? 
# netstat -ap | grep <process_name>

// how to see statistics by protocol ? 
# netstat -s 
```