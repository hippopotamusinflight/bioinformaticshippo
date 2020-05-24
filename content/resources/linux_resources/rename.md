---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: mass renaming
menu:
  linux_resources:
    name: mass renaming
    weight: 
title: mass renaming
toc: true
type: docs
weight: 
---

<!--
1. replace linux_resources with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-DD e.g. 2020-05-20
3. replace mass renaming with page name e.g. dplyr
4. replace  with weight e.g. 20
-->

## find + perl
- do below without mv first to check io
- then execute mass rename with mv
```bash
find . -type f | perl -pe 'print $_; s/.\/find/.\/replace/' | xargs -n2 mv
```

## find in current folder
- not recursive
```bash
find ./ -name "target"
```

## find with regex
```bash
find . -type f -regex ".\/name_starting_with.*"
```


## EOF