---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: permissions
menu:
  linux_resources:
    name: permissions
    weight: 15
title: permissions
toc: true
type: docs
weight: 15
---

<!--
1. replace linux_resources with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-DD e.g. 2020-05-20
3. replace permissions with page name e.g. dplyr
4. replace 15 with weight e.g. 20
-->

## syntax
```
dir
 |
 drwx     rwx    rwx
(owner) (group) (other)
   u       g      o
```

## notation
```bash
$ chmod u=rwx <file>       # give owner read, write, execute
$ chmod 700 <file>         # give owner read, write, execute
$ chmod 777 <file>         # give owner, group, other read, write, execute
```

## octal vs symbolic
```bash
$ chmod u=rwx <file>       # symbolic notation
$ chmod 700 <file>         # octal notation
```

```
octal   binary  symbolic
  7      111      rwx
  6      110      rw-
  5      101      r-x
  4      100      r--
  3      011      -wx
  2      010      -w-
  1      001      --x
  0      000      ---
```












## EOF