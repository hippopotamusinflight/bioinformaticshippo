---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: cut
menu:
  linux_resources:
    name: cut
    weight: 100
title: cut
toc: true
type: docs
weight: 100
---

<br>

```bash
cut -b 1-3,5-7 <file>                      # (byte, 1 to 3, 5 to 7)
cut -c 2,5-7 <file>                        # (character, 2, 5 to 7)
cut -d " " --complement -f 1 state.txt     # (delimiter = space, everything except field 1)
```

<br>






## EOF