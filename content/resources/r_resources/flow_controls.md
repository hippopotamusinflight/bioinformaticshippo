---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: flow controls
menu:
  r_resources:
    name: flow controls
    weight: 
title: flow controls
toc: true
type: docs
weight: 
---

<!--
1. replace r_resources with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-DD e.g. 2020-05-20
3. replace flow controls with page name e.g. dplyr
4. replace 1040 with weight e.g. 20
-->

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
knitr::opts_chunk$set(message = TRUE)
```

### next statement
- skip 1 iteration
```{r}
x <- 1:5
for (val in x) {
  if (val == 3){
    next
  }
print(val)
}
```

### break statement
- halt entire loop
```{r}
x <- 1:5
for (val in x) {
  if (val == 3){
    break
  }
print(val)
}
```

### invisible(), warning()
- R equivalent of python's pass statement
```{r}
my_func  <- function(x){
  invisible()
}
my_func(100)
```
```{r}
foo = function(){
  # write this tomorrow
  warning("You ran foo and I havent written it yet")
}
foo()
```

### EOF