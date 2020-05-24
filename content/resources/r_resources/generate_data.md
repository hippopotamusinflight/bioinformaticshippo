---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: generate numbers
menu:
  r_resources:
    name: generate numbers
    weight: 1010
title: generate numbers
toc: true
type: docs
weight: 1010
---

<!--
1. replace r_resources with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-2020-05-23 e.g. 2020-05-20
3. replace generate numbers with page name e.g. dplyr
4. replace 1010 with weight e.g. 20
-->

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
knitr::opts_chunk$set(message = FALSE)
```

[http://www.cookbook-r.com/Numbers/Generating_random_numbers/](http://www.cookbook-r.com/Numbers/Generating_random_numbers/)
```{r, results="hide"}
library(tidyverse)
```

### seq()
seq(from, to, by)
```{r}
seq(1, 10, by = pi)
```

### rep()
rep(sequence, each, times, len)
```{r}
rep(c("a","b"), each = 2)
rep(c("a","b"), each = 2, times = 4)
rep(c("a","b"), each = 2, times = 4, len = 5)
```

### runif()
runif(elements, min, max)  
returns floats by default  
```{r}
set.seed(420)
runif(5, 1, 10)
# use floor, ceiling or round to get integers
runif(5, 1, 10) %>% floor()
runif(5, 1, 10) %>% ceiling()
```

### sample()
sample(range, elements, replace?)  
can set with or without replacement  
```{r}
set.seed(420)
sample(1:100, 5, replace = TRUE)
sample(1:100, 5, replace = FALSE)
```


### normal functions
http://seankross.com/notes/dpqr/  

#### dnorm
dnorm(x, mean = 0, sd = 1, log = FALSE)  
- probability density for the normal distribution  
- value = height of PDF of normal distribution
```{r, fig.height=3, fig.width=4}
dnorm(0, mean = 0, sd = 1) # default values, dnorm = 0.3989423
zscores = seq(-3,3,by=0.1)
dvalues = dnorm(zscores)
plot(dvalues, # Plot where y = values and x = index of the value in the vector
     xaxt = "n", # Don't label the x-axis
     type = "l", # Make it a line plot
     main = "pdf of the Standard Normal",
     xlab= "Z-score") 

# These commands label the x-axis
axis(1, at=which(dvalues == dnorm(0)), labels=c(0))
axis(1, at=which(dvalues == dnorm(1)), labels=c(-1, 1))
axis(1, at=which(dvalues == dnorm(2)), labels=c(-2, 2))
```

#### pnorm
pnorm(q, mean = 0, sd = 1, lower.tail = TRUE, log.p = FALSE)  
- probability distribution  
- inverse of qnorm (given zscore, what's the cumulative probability)  
- value = integral (area) from -inf to q of PDF of normal distribution  
```{r}
pnorm(0, 0, 1) # default args, pnorm = 0.5
pnorm(2, 5, 3) # 0.1586553
```

#### qnorm
qnorm(p, mean = 0, sd = 1, lower.tail = TRUE, log.p = FALSE)  
- quantile function  
- inverse of pnorm (given quantile, what's the zscore?)  
```{r}
qnorm(0.5) # 0
qnorm(0.99) # 2.326348 (zscore of 99th quantile of normal distribution)
```

#### rnorm
rnorm(n, mean = 0, sd = 1)  
- random deviates  
- generates a vector of normally distributed random numbers  
```{r}
rnorm(5, mean = 0, sd = 1) # 1.15572257  0.36096234  0.01558131  0.57173629 -0.85518190
```

### EOF