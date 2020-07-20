---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: diff and comm
menu:
  linux_resources:
    name: diff and comm
    weight: 100
title: diff and comm
toc: true
type: docs
weight: 100
---

<br>

## diff

### difference after cut|sort 2 files
```bash
diff <(cut -f2 fileA | sort) <(sort fileB)
```

### more flags
```bash
diff -y -W 120 File_1.txt File_2.txt --suppress-common-lines
# -y        side by side, in 2 columns
# -W 120    maximum number of columns to print
```

<br>

## comm
- single output only
```bash
comm -23 <(sort fileA) <(sort fileB)    # A∩B' (lines in A but not in B)
comm -13 <(sort fileA) <(sort fileB)    # A'∩B (lines in B but not in A)
comm -12 <(sort fileA) <(sort fileB)    # A∩B (lines in both A and B)

# 3 columns (fileA_unique   common  fileB_unique)
# -1     suppress column 1 (lines unique to fileA)
# -2     suppress column 2 (lines unique to fileB)
```





<br>

## EOF