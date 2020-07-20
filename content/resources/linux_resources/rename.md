---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: mass renaming
menu:
  linux_resources:
    name: mass renaming
    weight: 100
title: mass renaming
toc: true
type: docs
weight: 100
---

<br>

## find (maxdepth, regex) + perl + xargs + mv
- do below without `mv` first to check io
- then execute mass rename with `mv`
```bash
find . -maxdepth 1 -type f
-regex ".\/.*something.*\.txt" | 
perl -pe 'print $_; s/.\/find/.\/replace/' | 
xargs -n2 
mv
```

<br>

## find + exec mv
- more efficient, but can't check io
```bash
find path_A -name '*AAA*' -exec mv -t path_B {} +
```


## EOF