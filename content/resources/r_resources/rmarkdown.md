---
title: "Rmarkdown"
linktitle: "Rmarkdown"
summary:
date: 2020-05-24T22:33:05-04:00
lastmod: 2020-05-24T22:33:05-04:00
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 5
menu:
  r_resources:
    name: "Rmarkdown"
    # parent: pid
    weight: 5
---

<!--
1. replace dirdir with page dir (e.g. r_resources)
2. replace ww with weight, or leave blank to sort pages alphabetically
-->





## horizontal rule
```
***
```

## R chunks options
```
echo=FALSE         show output only, no code
include=FALSE      no output, show code
messages=FALSE     no warnings
eval=FALSE         don't run code, no output
```

## KNITR results
```
results="hide"    no results, run code (similar to include=FALSE)
results="hold"    hold results to output at end of chunk
results="markup"  latex output
results="asis"    for kable
```

## kable
```{r echo=FALSE, results="asis"}
table <- matrix(1:6, 2, 3)
knitr::kable(table)
```

## include_graphics
```{r, echo=FALSE, fig.cap="something", out.width='25%', out.height='25%', fig.align='center'}
knitr::include_graphics("/Users/minghan/Google Drive/Coursera_survival_analysis/images/ss 2020-05-12 at 16.23.53.png")
```

## HTML text color
<span style="color:Tomato">**some text**</span>

## spaces
```
<p>
write text as is     with space.
</p>
```
- or just indent 4 spaces

## some more HTML text tags
```
Tag          Description
<b>          Defines bold text
<em>         Defines emphasized text 
<i>          Defines italic text
<small>      Defines smaller text
<strong>     Defines important text
<sub>        Defines subscripted text
<sup>        Defines superscripted text
<ins>        Defines inserted text
<del>        Defines deleted text
<mark>       Defines marked/highlighted text
```

## RMarkdown front matters
- [https://bookdown.org/yihui/rmarkdown/html-document.html](https://bookdown.org/yihui/rmarkdown/html-document.html)
```
title: "some title"
author: "Hippo"
date: "`r format(Sys.time(), '%Y_%m_%d_%H')`"
output: 
  html_document:
    toc: true
    toc_depth: 4
    toc_float:
      collapsed: false
    fig_width: 4
    fig_height: 3
    fig_caption: true
```


## EOF