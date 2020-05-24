---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: system admin
menu:
  linux_resources:
    name: system admin
    weight: 10
title: system admin
toc: true
type: docs
weight: 10
---

<!--
1. replace linux_resources with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-DD e.g. 2020-05-20
3. replace system admin with page name e.g. dplyr
4. replace 10 with weight e.g. 20
-->

## untar unzip
```bash
$ tar -xzf file.tar.gz
```

## env var
- set up environment variable PICARD
- so don't need to call full path to PICARD library all the time
- just use $PICARD
```bash
$ vim ~/.bash_profile
    export PICARD="/home/<username>/.../build/libs/"
$ source ~/.bash_profile
$ echo $PICARD
> /home/<username>/.../build/libs/
```

## version
- get linux version
```bash
$ cat /etc/*-release
or
$ lsb_release -a
```

## disk space
```bash
$ df -k /some/dir/    # in numerics
$ df -h /some/dir/    # in human readable format
```

## get dir size
```bash
dir1$ du -h          # human readable
dir1$ du -hs         # only size of dir1, no subdir
```

## cd with breadcrumbs
```bash
$ cd dir1/
$ pushd dir2/
$ popd               # returns to dir1
```

## 








## EOF