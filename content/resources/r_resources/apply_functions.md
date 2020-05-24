---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: apply functions
menu:
  r_resources:
    name: apply functions
    weight: 990
title: apply functions
toc: true
type: docs
weight: 990
---

<!--
1. replace r_resources with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-DD e.g. 2020-05-20
3. replace apply functions with page name e.g. dplyr
4. replace ww with weight e.g. 20
-->

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
knitr::opts_chunk$set(message = FALSE)
knitr::opts_chunk$set(warning = FALSE)
library(tidyverse)
library(ggplot2)
```

## apply functions
[https://stackoverflow.com/questions/3505701/grouping-functions-tapply-by-aggregate-and-the-apply-family](https://stackoverflow.com/questions/3505701/grouping-functions-tapply-by-aggregate-and-the-apply-family)

### apply (generic)
- works with matrices or higher dimensional (use sapply for vectors, lists)
- acts on a column or row, or individual elements, depending on FUN
- outputs vector or matrix
```{r, results="hide"}
mat = matrix(1:9, 3, 3); mat
apply(mat, 1, sum)
apply(mat, 2, sum)

# apply to rows and columns
apply(mat, 1, function(x) x*10)
```

### lapply (list apply)
- apply function to each element of a list
- returns a list
```{r, results="hide"}
x = list(1:10)
lapply(x, sqrt)
lapply(mat, function(x) (x+2))
```

### sapply (simple lapply)
- same as lapply, 
- but returns a vector
```{r, results="hide"}
x = list(1:10)
sapply(x, sqrt)
sapply(x, function(x) (x+2)) # custom function with sapply

# matrix input coersed to vector output
sapply(mat, function(x) (x+2))
```

### tapply (tagged apply)
- apply to subset of a vector
- subset defined by a factor (or some other vector)
```{r, results="hide"}
x = 1:20; x
y = factor(rep(letters[1:5], each = 4)); y
tapply(x, y, sum)
```

### mapply (multi-input apply)
- multiple input, can be several datatypes
```{r, results="hide"}
x = letters[seq(1:10)]
y = 2:11
z = rep(seq(0,1),10)
myFunc = function(x,y,z){
  return(paste0(x,y,z))
}
mapply(myFunc, x, y, z) %>% unname()
```

### rapply (recursive apply)
```{r, results="hide"}
# ???
```

### vapply (verified apply)
- specifies output datatype for faster compute
```{r, results="hide"}
x = list(a = 1, b = 1:3, c = 1:100)
y_int = vapply(x, FUN = length, FUN.VALUE = 0L)
class(y_int)
y_num = vapply(x, FUN = length, FUN.VALUE = 0.0)
class(y_num)
```

### EOF