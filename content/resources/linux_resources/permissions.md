---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: permissions
menu:
  linux_resources:
    name: permissions
    weight: 100
title: permissions
toc: true
type: docs
weight: 100
---

<br>

## syntax
```
dir
 |
 drwx     rwx    rwx
(owner) (group) (other)
   u       g      o
```

<br>

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

<br>

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










<br>

## EOF