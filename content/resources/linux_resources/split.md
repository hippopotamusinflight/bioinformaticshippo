---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: split
menu:
  linux_resources:
    name: split
    weight: 100
title: split
toc: true
type: docs
weight: 100
---

<br>

## split 1 billion lines per file
```bash
split -d -l 1000000000 <input>  <output>
# -d    output file names to end w/ 1,2,3,...
# -l    lines in each file
```

<br>

## split by size
```bash
split -b 200M <input>  <output>
# -b    bytes
```

<br>

## csplit can use regex for splitting
```bash
csplit <input> 4 {1}
```





<br>

## EOF