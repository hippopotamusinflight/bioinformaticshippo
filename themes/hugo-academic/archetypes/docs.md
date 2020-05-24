---
title: "{{ replace .Name "_" " " | title }}"
linktitle: "{{ replace .Name "_" " " | title }}"
summary:
date: {{ .Date }}
lastmod: {{ .Date }}
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: ww
menu:
  dirdir:
    name: "{{ replace .Name "_" " " | title }}"
    # parent: pid
    weight: ww
---

<!--
1. replace dirdir with page dir (e.g. r_resources)
2. replace ww with weight, or leave blank to sort pages alphabetically
-->


## 