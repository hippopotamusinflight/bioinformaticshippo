---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: sed
menu:
  linux_resources:
    name: sed
    weight: 100
title: sed
toc: true
type: docs
weight: 100
---

<br>

## extract lines
- `-n` = no stdout
```bash
sed -n '2p' <file>              # print 2nd line
sed -n '10,33p' <file>          # line 10 up to line 33
sed -n '1p;3p' <file>           # line 1 AND 3
sed -n '2,$p' <file>            # line 2 to the END
```

<br>

## line /find/replace/
```bash
sed 's/..$//' <input.txt> > <output.txt>      # delete last 2 characters from each line
```







## EOF