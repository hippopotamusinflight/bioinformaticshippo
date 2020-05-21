---
date: "2019-05-05T00:00:00+01:00"
draft: false
linktitle: Set_Operations
menu:
  r:
    name: SetOperations
    weight: 3
title: Set Operations
toc: true
type: docs
weight: 3
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
