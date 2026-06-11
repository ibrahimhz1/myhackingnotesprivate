---
title: Environment Variables
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
## To list all environment variables

```
# env
# printenv
```

## To print particular variable value

```
# echo $<VARIABLE_NAME>
# echo $HOME
# echo $HOSTNAME
# echo $MAIL
```

## To Set Environment Varibles Temporary

```
# export TESTVAR=10
```

## To Set Environment Variables Permanently

```
# vim .bashrc
export TESTVAR='abc'
// save and close the .bashrc file
# source .bashrc // reloads the .bashrc file
```

## To set the Environment Variables Globally and Permanent

```
# vim /etc/profile or /etc/bashrc
export TESTVAR='xyz'
// save and exit 
# source /etc/profile
```

## HOW TO UNSET Temporary Environment VARIABLES

```
# export $TEST=100
# unset <Environment Varible Name>
# unset TEST
```