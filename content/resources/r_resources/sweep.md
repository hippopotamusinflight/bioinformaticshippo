---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: sweep
menu:
  r_resources:
    name: sweep
    weight: 
title: sweep
toc: true
type: docs
weight: 
---

<!--
1. replace r_resources with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-DD e.g. 2020-05-20
3. replace sweep with page name e.g. dplyr
4. replace  with weight e.g. 20
-->

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

[https://bioinfomagician.wordpress.com/2014/08/12/my-favorite-commands-part3-sweep-function-in-r/](https://bioinfomagician.wordpress.com/2014/08/12/my-favorite-commands-part3-sweep-function-in-r/)

### sweep()
- apply different values to data in different col/rows
```{r}
data <- matrix(seq(1,12),ncol=4,nrow=3,byrow=TRUE)
data
sweep(data, 2, c(3, 4, 5, 6), "-")
```

example:
- **standardize gene expression using sweep**
- (normalized expression - median expression)/median absolute deviation
    - similar to zscore standardization ((x - mean)/sd)
- need to subtract different median expression (different for each gene),
    - and divide different MAD (different for each gene),
    - `sweep()` can help
```{r}
standardize <- function(z) {
  rowmed <- apply(z, 1, median)
  rowmad <- apply(z, 1, mad)  # median absolute deviation
  rv <- sweep(z, 1, rowmed,"-")  #subtracting median expression
  rv <- sweep(rv, 1, rowmad, "/")  # dividing by median absolute deviation
  return(rv)
}
data <- data.frame(data)
colnames(data) <- c("sample1","sample2","sample3","sample4")
rownames(data) <- c("gene1","gene2","gene3")

data[1,] %>% class()
data[1,] %>% as.matrix() %>% median()
data[1,] %>% as.matrix() %>% mad()
((data[1,1]) - (data[1,] %>% as.matrix() %>% median()))/data[1,] %>% as.matrix() %>% mad()

print(data)
print(standardize(data))
```

### EOF