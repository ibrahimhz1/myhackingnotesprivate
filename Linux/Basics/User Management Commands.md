---
title: User Management Commands
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
### Tasks:

> create user without home folder 
> create user with home folder 
> create system user 
> create group add user to group 
> add user to multiple groups 
> list all users 
> list all users from shadows 
> list all groups 
> list all groups of the user 
> delete user 
> delete user with home directory 
> delete group 
> remove user from group 
> set user password 
> show only particular user info from /etc/passwd 
> show only particular group info from /etc/group 
> change user home directory to different directory 
> move content from current home directory to new home directory 
> print $HOME variable of the user 
> lock the useraccount 
> unlock the useraccount 
> check expiry date of user account 
> set and change expiry date for the useraccount 
> set comment in user account (full name) & verify in /etc/passwd 
> change the primary group of user 
> change supplimentary group of user 
> change login name or rename username 
> lock the password of user account change UID of user

---

### List Existing User Accounts

```jsx
cat /etc/passwd
sudo cat /etc/shadow
```

### List Number of Existing User

```jsx
cat /etc/passwd | wc -l
```

### Create User Account

```jsx
sudo useradd ibrahimhz
```

### Create User Account with Home directory

```jsx
sudo useradd -m ibrahimhz
```

### Create System Account

```jsx
sudo useradd -r sysaccount
cat /etc/passwd | grep sysaccount
```

### Delete User Account without deleting home directory

```jsx
sudo userdel ibrahimhz
```

### Delete User Account along with home directory

```jsx
sudo userdel -r ibrahimhz
```

### Change Username or Login Name of the user account

```jsx
sudo usermod -l ibrahimhz ibrahimdmc
```

### Add Comment to User Account

```jsx
sudo usermod -c "Mohammmed Ibrahim" ibrahimhz
```

### Set Password for User

```jsx
sudo passwd ibrahimhz
```

### Change User’s HOME directory

```jsx
sudo usermod -d /home/NewHome ibrahimhz
```

### Move User’s HOME directory to Another directory

```jsx
sudo usermod -m -d /home/LatestHome ibrahimhz
```

### Check Expiry of User Account

```jsx
sudo chage -l ibrahimhz
```

### Set Expiry date to User Account

```jsx
sudo chage -E 2024-04-30 ibrahimhz
```

### Change UID of User Account

```jsx
sudo usermod -u 2002 ibrahimhz
```

### Lock the User Password to disable login

```jsx
sudo usermod -L ibrahimhz
```

### Unlock the User password to enable login

```jsx
sudo usermod -U ibrahimhz
```

### Create Group

```jsx
sudo groupadd gamers
```

### Delete Group

```jsx
sudo groupdel gamers
```

### Rename Group Name

```jsx
sudo groupmod newGamers gamers
```

### Add User to an Group as Supplementary group

```jsx
sudo usermod -G newGamers ibrahimhz
```

### Append User to Group as Supplementary group

```jsx
sudo usermod -aG newGamers,linux-admins,hackers ibrahimhz
```

### Change User’s Default group

```jsx
sudo usermod -g newGamers ibrahimhz
```

### Remove User from Group

```jsx
sudo gpasswd -d ibrahimhz newGamers
```