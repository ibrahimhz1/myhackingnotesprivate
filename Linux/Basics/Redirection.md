---
title: Redirection
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
### There are 3 redirects:

> Basically, when we run commands in terminal, below three files are created.

|File|File Descriptor|
|---|---|
|Standard Input (stdin)|0|
|Standard Output (stdout)|1|
|Standard Error (stderr)|2|

---

# Output: (stdout = 1)

- Output of a command is shown in terminal
- To route output in file using >
- example

```
# hostname > filename
```

- To append output in existing file using >>
- example

```
# pwd >> filename
```

---

# Error: (stderr = 2)

- if any command gives you error then it is considered as stderr - 2
- we can redirect the error to a file:
- Example

```
# cd /root/ 2> error_file
```

- To redirect both standard output and error to a file:
- Example

```
# cd /root/ > error_file 2>&1
```

```
# hostname >> std_err_out 2>&1
# cd /root/ >> std_err_out 2>&1
# cat std_err_out
```

```
# find /etc -t f 1> ~/output.txt 2> ~/errors.txt
# find /etc -t f > ~/output.txt 2> ~/errors.txt
# find /etc -t f >> ~/output.txt 2>> ~/errors.txt
```

# Input ( stdin = 0 )

> input is used when feeding file content to a file

Example

```
# cat < filename
# cat << EOF
# wc -l filename
# wc -l < filename
```