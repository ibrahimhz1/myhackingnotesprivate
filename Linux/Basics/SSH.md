---
title: SSH
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
---

## Basic SSH Connection method

```
// Syntax
ssh <user>@<ip-address>

// Example
# ssh ibrahimhz@192.168.136.20
```

## Creating an Config file for SSH for easier Connection

```
# cd .ssh
# touch config
# vim config

~/.ssh/config --> filename
Host myserver
		 Hostname 192.168.136.20
		 Port 22
		 User ibrahimhz
// save the file and exit

// now ssh into the server
# ssh myserver
```

---

# Using Public / Private keys

### Create the public and private key using ssh-keygen

```
# mkdir .ssh/ssh_keys/server1
# mkdir .ssh/ssh_keys/server2

// ssh keys for server-1
# ssh-keygen
(give filename with path for public and private key): ~/.ssh/ssh_keys/server1/sv2Key
passphrase: 
confirm passphrase:

// ssh keys for server-2
# ssh-keygen
(give filename with path for public and private key): ~/.ssh/ssh_keys/server2/sv2Key
passphrase: 
confirm passphrase:
 
```

## Add the Identity field in Config file

```
# cd .ssh/
# vim config
~/.ssh/config --> filename
Host myserver1
		 Hostname 192.168.136.20
		 Port 22
		 User ibrahimhz
		 IdentityFile ~/.ssh/ssh_keys/server1/sv1Key
		 
Host myserver2
		 Hostname 192.168.136.22
		 Port 22
		 User bingohz
		 IdentityFile ~/.ssh/ssh_keys/server2/sv2Key

// save and exit
```

## Create an authorized_keys file in target server

```
// login into the target server

# cd .ssh
# touch authorized_keys
# chmod 600 authorized_keys

# vim authorized_keys
~/.ssh/authorized_keys
// add the public key phrase into the file

// save and exit
```

## Now ssh into the server1 by only using the hostname

```
# ssh server1
// it will login into the server without asking the password
```

# Using the ssh-copy-id command to copy public key to remote server

```
# ssh-copy-id -i <public-key_file-path> <username@server-ip-address>
# ssh-copy-id -i ~/.ssh/ssh_keys/kalios/kaliKey.pub ibrahimhz@192.168.136.22
```

---

# Manage SSH Keys

## Generate key with different type and with comment

```
# ssh-keygen -t ed25519 -C "windows:server1"
# ssh-copy-id -i ~/.ssh/server1_id_ed25519.pub root@192.168.136.22
# ssh -i <private_key_path> root@192.168.136.22 
```

## SSH Agent to load the key passphrase in cache

```
# ps aux | grep ssh-agent
# eval "$(ssh-agent)"
# ps aux | grep ssh-agent
# ssh-add ~/.ssh/ssh_keys/<private_key_file>
// type passphrase. and it will load into the cache memory
# ssh root@<ip-address>
// it will automatically log in to the server
```

---

# SSH Server Configuration

### /etc/ssh/sshd_config

```
disable password authentication
change port number
```