---
title: 28_Lab28-Reflected XSS in a JavaScript URL with some characters blocked
date: 2026-07-09
tags:
  - 
publish: true
draft: false
description: ""
---

--- 

This lab reflects your input in a JavaScript URL, but all is not as it seems. This initially seems like a trivial challenge; however, the application is blocking some characters in an attempt to prevent XSS attacks.

To solve the lab, perform a cross-site scripting attack that calls the `alert` function with the string `1337` contained somewhere in the `alert` message.

---




