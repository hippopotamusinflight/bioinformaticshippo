---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: error handling
menu:
  r_resources:
    name: error handling
    weight: 
title: error handling
toc: true
type: docs
weight: 
---

<!--
1. replace r_resources with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-DD e.g. 2020-05-20
3. replace error handling with page name e.g. dplyr
4. replace  with weight e.g. 20
-->

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
knitr::opts_chunk$set(message = FALSE)
```

## stderr messages

### stop()
- halts execution
```{r}
logit <- function(x){
  if( any(x < 0 | x > 1) ){
    stop('x not between 0 and 1')
  }
  log(x / (1 - x) )
}
logit(0.4)
# error message without stop(): NaNs produced[1] NaN
# error message with stop(): Error in logit(-1) : x not between 0 and 1
```

### warning()
- still allows function to execute,  
- but throws a warning
```{r}
# using ifelse() + warning
# output NA and warning
logit <- function(x){
  x = ifelse( any(x < 0 | x > 1), NA, x )
  if(any(is.na(x))){
    warning('x not between 0 and 1')
  }
  log(x / (1 - x) )
}
logit(-1)
```

### print(), paste(), cat(), message(), warning(), stop()
[https://stackoverflow.com/questions/36699272/why-is-message-a-better-choice-than-print-in-r-for-writing-a-package](https://stackoverflow.com/questions/36699272/why-is-message-a-better-choice-than-print-in-r-for-writing-a-package)<br>
`print()`, `cat()` sends output to stdout  
`message()`, `warning()`, `stop()` sends output to stderr  
<br>
`print()` - cannot concatenate, prints [ x ] if multiple elements printed; but prints newline automatically  
`paste()` - allows concatenate  
`cat()` - allows concatenate, no [ x ]; but needs to specify newline manually  
<br>
`message()` - allows concat, no [ x ]; also needs to specify newline manually; but can be used with tryCatch()  
`warning()` - have problems when building packages, use message() instead  
`stop()` - 


## exceptions handling
- [http://adv-r.had.co.nz/Exceptions-Debugging.html](http://adv-r.had.co.nz/Exceptions-Debugging.html)

### try()
- allows execution to continue after error
```{r}
f1 <- function(x) {
  log(x)
  return(c(log(x),10))
}
# f1("x")
# Error in log(x) : non-numeric argument to mathematical function
```
```{r}
f1 <- function(x) {
  t <- try({            # capture try() as a variable (if failure, returns an invisible "try-error" object)
    a <- 1              # multiple expressions in try({})
    b <- a + x
    log(b)
    }, silent = TRUE)
  
  return(c(t,class(t),10))       # return try() results and outside results
}
f1("x")
f1(2)
```

### tryCatch()
- take different actions depend on warnings,  
- by mapping conditions to "handlers"  
- built-in handlers = error, warning, message, interrupt  
- catch-all handler = condition  
- handlers return a value, or create a more informative error message
```{r}
tryCatch(message("hello\n"), message=function(e){cat("goodbye\n")})
```

### withCallingHandlers()
- ...

### EOF