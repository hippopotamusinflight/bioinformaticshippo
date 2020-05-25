---
date: "2020-05-20T00:00:00+01:00"
draft: false
linktitle: set operations
menu:
  r_resources:
    name: set operations
    weight: 
title: set operations
toc: true
type: docs
weight: 
---

## set ops (vectors)
```
x <- LETTERS[1:8]; x      # "A" "B" "C" "D" "E" "F" "G" "H"
y <- LETTERS[5:10]; y     # "E" "F" "G" "H" "I" "J"
```


### setdiff()
- setdiff(x,y)
- what's in x, but not in y
```{r, results="hide"}
setdiff(x,y)              # "A" "B" "C" "D"
```

### union()
- union(x,y)
- what's in x, y and both x and y
```{r, results="hide"}
union(x,y)                # "A" "B" "C" "D" "E" "F" "G" "H" "I" "J"
```

### intersect()
- intersect(x,y)
- what's in both x and y
```{r, results="hide"}
intersect(x,y)            # "E" "F" "G" "H"
```

### setequal()
- setequal(x,y) -> bool
- check if 2 sets are equal
```{r, results="hide"}
setequal(x,y)             # FALSE
setequal(x,x)             # TRUE
```

### is.element()
- is.element(x,y) -> bool
- same as x %in% y
```{r, results="hide"}
is.element("A", x)        # TRUE
"A" %in% x                # TRUE
```
