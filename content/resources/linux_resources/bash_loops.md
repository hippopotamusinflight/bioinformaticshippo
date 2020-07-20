---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: bash loops
menu:
  linux_resources:
    name: bash loops
    weight: 20
title: bash loops
toc: true
type: docs
weight: 20
---

<br>

## for loop1
```bash
for val in {1..10}; do
    ...
done
```

<br>

## for loop2
```bash
for ((i=1; i<=10; i++)); do
    ...
done
```

<br>

## iterate over user input
```bash
vim test.sh
for var in "$@"; do
    echo "$var"
done

# output
# bash test.sh 1 2 '3 4'
# 1
# 2
# 3 4
```

<br>

## iterate over files in dir
```bash
for FILE in something*; do
    echo ${FILE}
    ...
done
```





<br>

## EOF