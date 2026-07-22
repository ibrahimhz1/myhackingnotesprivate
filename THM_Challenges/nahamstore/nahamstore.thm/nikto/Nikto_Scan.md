---
title: Nikto Scan
date: 2026-07-23
tags:
  - thm
  - nahamstore
  - nikto
  - scan
publish: false
draft: false
description: "Nikto vulnerability scanner commands run against nahamstore.thm"
---

# Nikto Scan

## Commands Used

```bash
# Basic scan
sudo nikto -h nahamstore.thm

# SSL scan (for sites with SSL)
sudo nikto -h https://smvec.ac.in -ssl

# Scan multiple hosts from file
touch nikto_hosts.txt
# add hostname in the nikto_hosts.txt file
sudo nikto -h nikto_hosts.txt

# Save output as HTML
sudo nikto -h http://nahamstore.thm -f htm -o /home/spartan/nahamstore_nikto_scan.html

# Pause between requests (evasion)
sudo nikto -h http://nahamstore.thm -Pause 5

# Tuning (specific tests)
sudo nikto -h http://nahamstore.thm -Tuning 4

# List available plugins
nikto -list-plugins
```
