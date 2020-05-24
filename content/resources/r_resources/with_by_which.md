---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: with by which
menu:
  r_resources:
    name: with by which
    weight:
title: with by which
toc: true
type: docs
weight:
---

<!--
1. replace r_resource with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-DD e.g. 2020-05-20
3. replace with by which with page name e.g. dplyr
4. replace  with weight e.g. 20
-->

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
knitr::opts_chunk$set(message = FALSE)
```

```{r, results="hide"}
library(tidyverse)
```

- [https://www.statmethods.net/stats/withby.html](https://www.statmethods.net/stats/withby.html)
- both with() and by() derives from DATA and BY from SAS  

### with()
- with(dataset, expression)
- with() applies an expression to a dataset
```{r}
data1 <- iris[,4:5] %>% filter(Species != "setosa")
with(data1, t.test(Petal.Width ~ Species))

# or you have to create 2 vectors
versi <- data1 %>% filter(Species == "versicolor") %>% select(-Species)
verg <- data1 %>% filter(Species == "virginica") %>% select(-Species)
t.test(versi, verg)

# actually t.test() has formula interface...
t.test(Petal.Width ~ Species, data = data1)
```

- so... what's the point of with()?  
- [https://stackoverflow.com/questions/42283479/when-to-use-with-function-and-why-is-it-good](https://stackoverflow.com/questions/42283479/when-to-use-with-function-and-why-is-it-good)
- some old functions doesn't have a "data" argument,  
- need to retype dataframe name each time to get columns
```{r}
# without with(), we would be stuck here:
z = mean(mtcars$cyl + mtcars$disp + mtcars$wt); z

# using with(), we can clean this up:
z = with(mtcars, mean(cyl + disp + wt)); z
```

- tidyverse functions all have "data" argument, so with() is not needed as much  
- **do not use attach()**, if you update colnames, sometimes old colnames attached to GlobalEnv might not sync  

### by()
- by(data, factorlist, function)  
- by() applies a function to each level of a factor(s)  
```{r}
iris[,5] %>% class()
by(iris, iris$Species, function(x) mean(x$Petal.Length))
by(iris, iris$Species, function(x) mean(x$Petal.Length * 10 + 2))
```

### which()
- find index of TRUE in vector
```{r}
which(letters == "g")
which(letters[1:5] != "b")
which(letters[1:5] != "b" & letters[1:5] != "a")
```

### EOF