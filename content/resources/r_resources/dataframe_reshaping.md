---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: dataframe reshape
menu:
  r_resources:
    name: dataframe reshape
    weight: 
title: dataframe reshape
toc: true
type: docs
weight: 
---

<!--
1. replace r_resources with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-DD e.g. 2020-05-20
3. replace dataframe reshape with page name e.g. dplyr
4. replace  with weight e.g. 20
-->

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
knitr::opts_chunk$set(message = FALSE)
```

```{r}
library(tidyverse)
library(knitr)
```

## data reshaping
- [https://tidyr.tidyverse.org/](https://tidyr.tidyverse.org/)
- tidying data
- make data "one case per row", "one feature per column", "one value per cell"

| data reshaping terminology |  |  |
|----------------------------|---------|--------|
| tidyr | gather | spread |
| reshape(2) | melt | cast |
| spreadsheets | unpivot | pivot |
| databases | fold | unfold |

<br>
- simple transpose doesn't work  
- some contents are correct, some are not  
<br>
- `gather()` - moves columns into rows
- `spread()` - moves rows into columns

### untidy data
```{r}
seps <- read_csv("http://www.mm-c.me/mdsi/hospitals93to98.csv")
seps %>% dim()
seps %>% head()
```
<br>
problems:  
- PatientDays and Separations are together
- years are colnames

#### step1 - gather()
- push data in columns to rows
- gather(data, key, value, key_range, na.rm, convert)  
    key = name of new "naming" variable (year)  
    value = name of new "result" variable (value)  
    convert = if TRUE, converts what looks like numeric to numeric, char to char  
<br>
- gather() moves colnames into a "key" column, gathers col values into a single "values" column
```{r}
inprog <- seps %>% gather(., key=year, value=value, FY1993:FY1998); inprog %>% head()
```

#### step2 - spread()
- spread value of a value column (value) across new columns  
- long to wide  
- spread(data, key, value, fill = NA, convert = FALSE, drop = TRUE, sep = NULL)  
    key = column you want to split apart (Field)  
    value = column you want to use to populate the new columns (value)  
    fill = if combinations doesn't exist  
```{r}
rearrag <- spread(inprog, key = Field, value = value); rearrag %>% head()
# every row is a unique combination of col values
```

### separate()
- separate(data, col, into, sep)
```{r}
rearrag %>% separate(., col = year, into = c("leader", "year"), sep = 2) %>% head()
rearrag %>% separate(., col = year, into = c("leader", "year"), sep = "19") %>% head()
```

- whatever is in sep will be gone, so need preceded-by and followed-by stringR syntax
```{r}
# https://stackoverflow.com/questions/9756360/split-character-data-into-numbers-and-letters
# (?<=[A-Za-z]) means preceded by a letter
# (?=[0-9]) means followed by a number
# the middle is where you want to separate
rearrag %>% separate(., col = year, into = c("leader", "year"), sep = "(?<=[A-Za-z])(?=[0-9])") %>% head()
```

### unite()
- unite(data, cols, col, sep)
```{r}
sep_d = rearrag %>% separate(., col = year, into = c("leader", "year"), sep = "(?<=[A-Za-z])(?=[0-9])")
sep_d %>% unite(., leader, year, col = year, sep = "") %>% head()

# unite 3 columns
sep_d2 = sep_d %>% separate(., col = year, into = c("century", "year"), sep = "(?<=19)(?=9)"); sep_d2 %>% head()
sep_d2 %>% unite(., leader, century, year, col = year, sep = "_") %>% head()
```

### EOF