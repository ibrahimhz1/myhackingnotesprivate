---
title: awk command
date: 2026-06-11
tags:
  - 
publish: true
draft: false
description: ""
---
---

> Text Processing Command

## To print particular column or field

```
# awk '{print $1}' filename.txt --> printing field1
# awk '{print $4}' filename.txt --> printing field4
```

## To print multiple column or field

```
# awk '{print $1, $3}' filename.txt --> printing field1 and field3
// using , comma will give an space between two columns
```

## To print last column or field

```
# awk '{print $NF}' filename.txt
```

## To add serial number

```
# awk '{print NR, $0}' filename.txt
// $0 means print all column
```

## To add formatting

```
# awk '{print NR ": " $0}' filename.txt
```

## Specifying Field Separator

> By default field is separated by space ‘ ‘

```
# awk -F',' '{print $2}' file3_email_det.csv
// -F, means field is seperated by ','
```

## Condition

```
# awk '{if($3>40000) print $0}' filename.txt
```

## Changing value in output based on condition

```
# awk '{if($2=="Pol"){$3==80000} print $0}' filename.txt
```

## Printing the lines which has Australia in the line

```
# awk '/Australia/ {print $0}' file4_emp_det.csv
/<Search String>/ --> it is used to search string in the content
```

## Print only particular line

```
# awk 'NR=="8" {print $0}' filename.txt
```

## Print range of lines

```
# awk 'NR=="8", NR=="10" {print $0}' filename.txt
```

## Print all lines except first line

```
# awk -F',' 'NR!="1" {print $0}' filename.txt
```

## To check which line is empty

```
# awk 'NF=="0" {print NR}' filename.txt
```

## To check no.of lines in file

```
# awk 'END {print NR}' filename.txt
```

## For loop

```
# awk 'BEGIN {for(i=0; i<10; i++) print i;}' filename.txt
```

## While loop

```
# awk 'BEGIN {while(i<10){i++; print "num is " i }}' filename.txt
```

---