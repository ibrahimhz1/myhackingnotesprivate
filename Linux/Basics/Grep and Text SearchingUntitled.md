---
title: Grep and Text SearchingUntitled
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
## Grep

```jsx
# grep [option] [pattern] [filename]
# grep ibrahim /etc/passwd
// case sensitive off
# grep -i ibrahim /etc/passwd
// return the remaining result but not searching names
# grep -iv ibrahim /etc/passwd
// returns number of times the keyword occurs
# grep -c ibrahim /etc/passwd
// search exact word
# grep -w ibrahim /etc/passwd
// returns with line number
# grep -n ibrahim /etc/passwd
// to search given keyword in multiple files
# grep ibrahim file1 file2 file3 
# grep -n ibrahim file1 file2 file3
// suppress file names while searching keyword in file
# grep -h ibrahim file1 file2 file3
// To search multiple keywords in a file
# grep -e ibrahim -e kalilinux file
# grep -n -e "ibrahim" -e kalilinux file
// To search multiple keywords in multiple files
# grep -e raju -e baburao file1 file2 file3
# grep -n -e raju -e baburao file1 file2 file3
// To only print file name in which matches given keyword 
# grep -l ibrahim file1 file2 file3
# grep -l -e ibrahim -e kali -e ubuntu file1 file2 file3 
// To get the keywords/pattern from a file and match with another file
# grep -f keywords.txt file1
# grep -f keywords.txt file1 file2 file3 
// To print the matching line which start with the give pattern
# grep "^keyword" file1
// To print the matching line which end with the give pattern
# grep "keyword$" file1
// To search keyword in all the files in the directory 
# grep -R "keyword" directory/
# grep -R -f keywords.txt grepseaches/

// We can use egrep command to search mutliple keywords
# egrep "key1|key2|key3" filename

// only search but not print in terminal 
# grep -q "keyword" file
# echo $? // checking the exit status of the previous command.

// If you want to suppress error message
# grep -s "keyword" file
```

