---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: copy
menu:
  linux_resources:
    name: copy
    weight: 100
title: copy
toc: true
type: docs
weight: 100
---

<br>

## copy except certain files
- using extglob
```bash
shopt -s extglob             # enable extended shell glob
cp !(excluding*) /dest_dir/
shopt -u extglob             # disable extended shell glob
```






<br>

## EOF