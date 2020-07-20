---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: general sysadmin
menu:
  linux_resources:
    name: general sysadmin
    weight: 10
title: general sysadmin
toc: true
type: docs
weight: 10
---

## untar unzip
```bash
$ tar -xzf file.tar.gz
```

<br>

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

<br>

## version
- get linux version
```bash
$ cat /etc/*-release
or
$ lsb_release -a
```

<br>

## disk space
```bash
$ df -k /some/dir/    # in numerics
$ df -h /some/dir/    # in human readable format
```

<br>

## get dir size
```bash
dir1$ du -h          # human readable
dir1$ du -hs         # only size of dir1, no subdir
```

<br>

## cd with breadcrumbs
```bash
$ cd dir1/
$ pushd dir2/
$ popd               # returns to dir1
```

<br>

## pwd with real path
- ignore symbolic links, prints full path
```bash
pwd -P
```

<br>

## ls view dir file size in MB
```bash
ls -lah
```








<br>

## EOF