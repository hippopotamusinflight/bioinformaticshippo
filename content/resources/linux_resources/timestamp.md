---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: timestamp
menu:
  linux_resources:
    name: timestamp
    weight: 100
title: timestamp
toc: true
type: docs
weight: 100
---

<br>

## PRINTF TIME
```bash
time1=$(date +%s)

code...

time2=$(date +%s)
secs=$((time2-time1))
printf 'job took %dd:%dh:%dm:%ds\n' $(($secs/86400)) $(($secs%86400/3600)) $(($secs%3600/60)) $(($secs%60))
```





<br>

## EOF