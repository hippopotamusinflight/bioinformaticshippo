---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: watch
menu:
  linux_resources:
    name: watch
    weight: 100
title: watch
toc: true
type: docs
weight: 100
---

<br>

## continuously watch logfile output
```bash
watch 'head -n 2 <logfile>; tail -f <logfile> | grep <keyword or pattern>'
# beginning 2 lines
# tail grep certain lines only
```

## example
```bash
watch 'grep -E "Epoch|val_accuracy" <logfile> | tail; tail <logfile>'
# grep "Epoch" or "val_accuracy", but get latest only with tail
# also output last lines of logfile
```





<br>

## EOF