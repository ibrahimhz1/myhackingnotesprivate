---
title: FTP Server
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
---

# Install the vsftpd package

```
# yum install vsftpd
# apt install vsftpd
```

## Remote Server FTP Setup:

### Config changes:

> /etc/vsftpd/vsftpd.conf

- anonymous_enable=NO
- Uncomment
    - ascii_upload_enable=YES
    - ascii_download_enable=YES
- May add
    - use_localtime=YES

---

### Disable the firewall.service in Server

```
# sudo systemctl stop firewall.service
```

---

# Client Side configuration

- need root access
- install ftp service (if not already installed)
- yum install ftp

```
# apt install ftp
# yum install ftp
```

# How to transfer file ?

- ftp 192.168.136.137 (ip of remote server)
- Enter username and password
- put <filename>
- mput <filename1> <file2> <file3> —> for copying multiple file

## How to download from remote server ?

```
# 
```