---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: sort and uniq
menu:
  linux_resources:
    name: sort and uniq
    weight: 100
title: sort and uniq
toc: true
type: docs
weight: 100
---

<br>

## sort by column
```bash
sort -t "," -k2,2 -n -r -u <file> 
# -t      field separator,
# -k2,2   column 2 only, 
# -n      numerical sort (or else lexicographical, 10 comes before 1), 
# -r      reverse, 
# -u      return first line of duplicated values of column 2 after sorting (need to use with -n if numerical column)
```

<br>

## other sort options
```bash
# -k2.1,2.2  sort by field 2 position 1 to field 2 position 2
# -k3n       sort by field 3 onward
# -R         scramble list order
# -f         ignore case
# -s         "stabilizes sort", if ignore case, keeps original input order
```

<br>

## sort + uniq 

### to count unique lines
```bash
sort <file> | uniq -ic
# -i   ignore case
# -c   count
```

<br>

### to print duplicated lines ONCE
```bash
sort <file> | uniq -d
# -d   print duplicated lines once
```

<br>

### to print ALL duplicated lines
```bash
sort <file> | uniq -iD
# -D   print all duplicated lines
```

<br>

### to print number of duplicated items
```bash
sort <file> | uniq -D | uniq -ic
```

<br>

### to print TOTAL number of duplicated lines
```bash
sort <file> | uniq -idc
# -d   print duplicated lines once
```

<br>

### uniq as to first "w" characters
```bash
uniq -w 2 <file>   # if first 2 char same, then line count as duplicate
```

<br>

## to get unique lines based on a column
- need AWK
```bash
sort -nk3 <file> | awk -F"[. ]" '!a[$2]++'
# sorts by 3rd column numerically
# awk removes duplicates based on 2nd column
```

<br>

## EOF