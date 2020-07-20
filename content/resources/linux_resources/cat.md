---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: cat
menu:
  linux_resources:
    name: cat
    weight: 100
title: cat
toc: true
type: docs
weight: 100
---

<br>

## view wc -l without filename
```bash
cat <file> | wc -l > output
```

<br>

## view hidden characters in file
```bash
cat -A <file>        # view all characters (tab = ^I)
cat -E <file>        # shows "\n" as "$"
```

## view csv files with "," in field
- https://www.stefaanlippens.net/pretty-csv.html
```bash
cat data.csv | perl -pe 's/((?<=,)|(?<=^)),/ ,/g;' | column -t -s, | less -S
```







<br>

## EOF