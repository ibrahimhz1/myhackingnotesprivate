---
title: SED Command
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
---

## Printing the Lines

> -n —> it is used for printing only the selected line numbers only, otherwise it will print all content of the file.

> -p —> print

### Print the particular line

```
# sed -n '10p' filename.txt
```

### Print multiple lines

> ; → is used for and operator (eg: line-1 and line-2) ‘ 1p ; 2p ’

```
# sed -n '1p;10p' filename.txt
```

### Print range of lines

> , → comma is used for range operator (eg: line-1 to line-10) ‘1-10p’

```
# sed -n '1,10p' filename.txt
```

### Print last line

> $ —> is used to print last line of the file

```
# sed -n '$p' filename.txt
```

### Print no.of lines after some line number

```
# sed -n '2,+2p' filename.txt

// prints 2 lines after the 2nd line is printed means 2,3,4 lines are printed
```

### Combining multiple line printings

> Each line operator is separated by and operator ‘ ; ‘

```
# sed -n '1p; 10,15p; $p' filename.txt

// print 1st and 10th to 15th and last line of the file
```

### Printing Odd/Even lines

```
# sed -n '1~2p' filename.txt

// start from 1st line and then start printing every second line until last line reached.
```

# How to Read Expression from File

```
# sed -n -f <expression_file_name> datafile.txt
```

---

# Substitute (Find and Replacement)

> s → substitute (find and replace) Syntax: sed 's/<find>/<replace>/' filename.txt

```
# sed 's/root/toor/' passwdfile.txt

// it only changes the root keyword one time in each line of the file
```

> To replace the keyword globally in the row use ‘g’ suffix at the end

```
# sed 's/root/toor/g' passwdfile.txt
```

### To ignore case sensitivity while substituting

> i → use i option to ignore case sensitivity

```
# sed 's/root/toor/i' passwdfile.txt
# sed 's/root/toor/gi' passwdfile.txt
```

### To substitute in particular line

> before the search operator, give the line number

```
# sed '5 s/root/toor/g' passwdfile.txt
```

### To substitute in range of lines

> before the search operator, give the line numbers separated by ‘ , ’

```
# sed '5,6 s/root/toor/g' passwdfile.txt
```

### To substitute in multiple lines

> -e → used to combine multiple sed commands

```
# sed -e '1 s/root/toor/g' -e '3 s/root/toor/g'  passwdfile.txt

// replacing root to toor in line number 1 and 3 globally 
```

### To substitute in all lines except particular line

```
# sed '2! s/root/toor/g' passwdfile.txt
```

### Control the substitute occurrence

> specify the occurrence number at the end of searching

```
# sed 's/root/toor/2' passwdfile.txt

// 2 -> substitute only the second occurrance 

# sed 's/root/toor/2g' passwdfile.txt

// 2g -> means substitute globally after the second occurrance, it will not substitute first occurrance
```

### Insert into the file

> -i → option is used to write into the file

> use this option with caution **!!!**

```
# sed -i -e '1 s/root/toor/g' -e '10 s/root/toor/g' passwdfile.txt 
```

### Comment the lines in file

> in Linux # is used to comment the line

```
# sed '5 s/^/#/' passwdfile.txt
// comment the line number 5 
# sed '5,10 s/^/#/' passwdfile.txt
// comment the lines from 5th line to l0th line

// ^ -> used to point starting of the line
```

### Uncomment the Line in file

```
# sed '5 s/^#//' passwdfile.txt

// selecting the starting character and replacing with blank
```

### Delete the line

```
# sed '5d' passwdfile.txt

// delete the 5th line

# cat -n passwdfile.txt | sed '5d'
# cat -n passwdfile.txt | sed '1,5d'
```

### Keep only selected lines and delete all other lines

```
# cat -n passwdfile.txt | sed '5,10!d'

// keep only 5 to 10th line in the file and delete the rest of the lines
```

### Insert before the line

```
# sed '5i ibrahimhz is mad' passwdfile.txt

// insert 'ibrahimhz is mad ' before the line number 5
```

### Insert after the line

```
# sed '5a ibrahimhz is mad' passwdfile.txt

// insert 'ibrahimhz is mad' after the line number 5
```

### Search keyword

```
# sed -n '/India/p' filename.txt
```

### Multiple expression

```
# sed -n -e '/India/p' -e '/Germany/p' filename.txt
```

### Search and Edit the line

```
# sed '/Ibrahimhz/ s/root/toor/g' datafile.txt
```

### How to delete a line ?

> d option is used to delete the line

```
# sed '1d' datafile.txt

// delete the first line
```

### Delete Last Line ?

```
# sed '$d' datafile.txt
```

### Delete range of lines ?

```
# sed '2,4d' datafile.txt
```

### Search and delete the line ?

```
# sed '/India/d' datafile.txt

// delete the line containing India
```

### Delete Empty line ?

```
# sed '/^$/d' datafile.txt

// checks the empty character in first place in the line and delete it
```

### write the sed command output to another file ?

```
# sed '/India/ w indianUsers.txt' datafile.txt

// searches India in the lines and write those selected lines to new file called indianUsers.txt
```

### Append the content to next line ?

```
# sed '/Loki/ a Hello User' datafile.txt
```

### Change the line content ?

```
# sed '4 c Hello User' datafile.txt

// change the 4th line to Hello User
```

### How to check weather file contains hidden characters ?

```
# sed -n 'l' datafile.txt

// show the content with special charactes
```

### How to wrap the content of the file ?

```
# sed -n 'l 20' datafile.txt

// show only 20 characters in each line and wrap other to next line 
```

### How to read from file and insert into next line ?

```
# sed '3 r externalfile.txt' datafile.txt

// read the externalfile.txt content and insert after the line 3 
```

### Control search occurrence ?

```
# sed '/India/ q' datafile.txt

// Search India in file and quit the serach after the first search keyword 'India' is found
```

### Insert the command’s output to the line ?

> e means expression means command output

```
# sed '2 e date' datafile.txt

// execute the date command, then insert the output content to the 2nd line of the file as expression
```

### Printing with certain rules? character sets

```
# sed -n '/[AC]/p' namelist.txt

// print only those lines that has A or C in the lines
```

### Printing with certain rules? character range

```
# sed -n '/[A-D]/p' namelist.txt

// print only those lines that has A B C D in the lines
```

### posix classes ?

> posix classes is used to print the lines which has space, upper, lower, digit, alphabet, punchtuate charactes init.

```
examples :-
# sed -n '[[:space:]]' datafile.txt
# sed -n '[[:upper:]]' datafile.txt
# sed -n '[[:lower:]]' datafile.txt
# sed -n '[[:digit:]]' datafile.txt
# sed -n '[[:alpha:]]' datafile.txt
# sed -n '[[:punct:]]' datafile.txt
```