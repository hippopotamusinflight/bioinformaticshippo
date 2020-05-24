---
date: "2020-05-22T00:00:00+01:00"
draft: false
linktitle: tidyverse dplyr
menu:
  r_resources:
    name: tidyverse dplyr
    weight: 20
title: tidyverse dplyr
toc: true
type: docs
weight: 20
---

```{r, results="hide"}
iris %>% head()
```

## filter()
- filter(.data, TRUE, .preserve = FALSE)
- selects rows where condition is TRUE

```{r, results="hide"}
iris %>% filter(between(Sepal.Length, 4, 4.3)) %>% head()
iris %>% filter(Petal.Length > 6 & Petal.Width < 2)
iris %>% filter(xor(Petal.Length < 6, Petal.Width > 2)) %>% head()
iris %>% filter(Petal.Length %in% c(4.4, 4.3)) %>% head()
```

## scoped filter variants
- filtering across multiple columns, dynamic selection of columns
- returns all columns
```{r, results="hide"}
iris_numerics = iris[,!colnames(iris)=="Species"]
```

### filter_all()
- filter_all(.tbl, .vars_predicate, .preserve = FALSE)
- considering all columns, for each row

```{r, results="hide"}
# all_vars (if all cols have values > 0.3, keep that row)
iris_numerics %>% filter_all(all_vars(. > 0.3)) %>% head()
# any_vars (if any cols have values > 0.3, keep that row)
iris_numerics %>% filter_all(any_vars(. > 4)) %>% head()
```

### filter_if
- filter_if(.tbl, .predicate, .vars_predicate, .preserve = FALSE)
- consider only columns matching .predicate when filtering with .vars_predicate

```{r, results="hide"}
# is.numeric as .predicate
iris %>% filter_if(~ is.numeric(.), all_vars(. > 2)) %>% head()

# selects cols whose 2 < mean < 4 (cols Sepal.Width and Petal.Length)
# all_vars (select rows where both Sepal.Width and Petal.Length have to be < 3)
iris_numerics %>% filter_if(~ mean(.) > 2  & mean(.) < 4, all_vars(. < 3)) %>% head()

# any_vars (any have to be < 3)
iris_numerics %>% filter_if(~ mean(.) > 2  & mean(.) < 4, any_vars(. < 3)) %>% head()
```

### filter_at
- filter_at(.tbl, .vars, .vars_predicate, .preserve = FALSE)
- selects cols with vars() specification (regex)

```{r, results="hide"}
# all_vars (all cols containing "tal", those cols - values must be even)
iris_numerics %>% filter_at(vars(contains("tal")), all_vars(((. * 10) %% 2) == 0)) %>% head()

# select cols with str_which and regex, those cols - values must be even
get_names = iris_numerics %>% names() %>% str_subset(., "^P")
iris_numerics %>% filter_at(vars(get_names), all_vars(((. * 10) %% 2) == 0)) %>% head()
```

## select()
- select(.data, COLNAMES)
- subset cols by colnames

```{r, results="hide"}
get_names = iris_numerics %>% names() %>% str_subset(., "^p")
iris %>% select(get_names) %>% head()

# drop cols
iris %>% select(-Species) %>% head()

# rearrange columns
iris %>% select(Species, everything()) %>% head() # everything()
iris %>% select(-Sepal.Length, Sepal.Length) %>% head() # move with -name

# rename group of variables
get_names = iris_numerics %>% names() %>% str_subset(., "^[:lower:]")
iris %>% select(obs = get_names) %>% head()

# rename multiple variables
vars <- c(var1 = "Sepal.Length", var2 ="Sepal.Width")
iris %>% select(vars) %>% head() # not sure why use !!!var or !!var...
```

## scoped select variants
- all selects some columns, and change colnames

### select_all
- select_all(.tbl, .funs = list(), ...)
- changes all columns, takes func as argument

```{r, results="hide"}
# toupper, changes colnames to UPPERCASE
iris %>% select_all(toupper) %>% head()

# regex replace colnames
iris %>% select_all(., ~ str_replace(., "t", "weeee")) %>% head()
```

### select_if
- select_if(.tbl, .predicate, .funs = list(), ...)
- select_if, uses logical statements, then do something

```{r, results="hide"}
# select by datatype
iris %>% select_if(is.numeric) %>% head()

# multiple ifs
iris %>% select_if(~is.numeric(.) & mean(.) > 5) %>% head() # need ~ since mean(.) > 5 not a function

# n_distinct()
iris %>% select_if(~n_distinct(.) < 10, toupper) %>% head()
```

### select_at
- select_at(.tbl, .vars, .funs = list(), ...)
- select columns with .vars, then do something

```{r, results="hide"}
# select vars without "ar" OR starts_with "c", then convert to UPPERCASE
select_at(mtcars, vars(-contains("ar"), starts_with("c")), toupper) %>% head()

# vars positional
select_at(mtcars, vars(-(1:3)), toupper) %>% head()
```

### custom function
```{r, results="hide"}
is_whole <- function(x) all(floor(x) == x)
select_if(mtcars, is_whole, toupper) %>% head()
```

## rename
- rename(.data, c("new_col1" = old_col1, "new_col2" = old_col2))
- rename specific column(s)
```{r, results="hide"}
iris %>% rename("new_col1" = Sepal.Length, "new_col2" = Sepal.Width) %>% head
```

## scoped rename() variants
- select() drops variables not in selection; rename() retains them

### rename_all
- same as select_all
```{r, results="hide"}
iris %>% rename_all(., ~ str_replace(., "t", "weeee")) %>% head()
```

### rename_if
- rename_if(.tbl, .predicate, .funs = list(), ...)
```{r, results="hide"}
iris %>% rename_if(~n_distinct(.) < 10, toupper) %>% head()
```

### rename_at
- rename_at(.tbl, .vars, .funs = list(), ...)
```{r, results="hide"}
rename_at(mtcars, vars(-(1:3)), toupper) %>% head()
```

## mutate
- mutate(.data, new_col = func(old_col))
- compute new columns, keeps others

```{r, results="hide"}
iris %>% mutate(sepal10 = Sepal.Length*10) %>% head()
iris %>% mutate(Sepal.Length = Sepal.Length*10) %>% head() # mutate in-place
iris %>% mutate(Sepal.Length = Sepal.Length*10, 
                Sepal.Width = Sepal.Width*10) %>% head() # mutate multiple cols in-place
iris %>% mutate(Sepal.Length = NULL, 
                Sepal.Width = Sepal.Width*10) %>% head() # mutate and remove cols

# grouped mutates with group_by()
# without group_by()
iris %>% mutate(Sepal.Length_mean = mean(Sepal.Length)) %>% head()
# with group_by(), new col has grouped calculations
iris %>% group_by(Species) %>% mutate(Sepal.Length_mean = mean(Sepal.Length)) %>% head()

# check group_by() output
i = 1 # setosa
iris %>% group_by(Species) %>% 
  mutate(Sepal.Length_mean = mean(Sepal.Length)) %>% 
  filter(Species == iris %>% select(Species) %>% unique() %>% .[i,]) %>% head() # with group_by()
```

## scoped mutate() variants
- all mutates in-place, unless name given with list())
```{r, results="hide"}
# user-defined function
scale2 = function(x, na.rm = TRUE){
  return(x - mean(x, na.rm = na.rm)/sd(x, na.rm = na.rm))
}
```

### mutate_all
- mutate_all(.tbl, .funs, ...)

```{r, results="hide"}
iris_numerics %>% mutate_all(as.integer) %>% head()
```

### mutate_if
- mutate_if(.tbl, .predicate, .funs, ...)
- get cols by predicate function

```{r, results="hide"}
iris %>% mutate_if(is.numeric, scale2, na.rm = TRUE) %>% head()

# transforming variable type (is. to as.)
iris %>% mutate_if(is.double, as.integer) %>% head() %>% as_tibble()

# mutates in-addition instead of in-place 
iris %>% mutate_if(is.double, list("int" = as.integer)) %>% head() %>% as_tibble()

# mutates with expression-function
iris %>% mutate_if(~n_distinct(.) == 2, as.factor) %>% head()
```

### mutate_at
- mutate_at(.tbl, .vars, .funs, ..., .cols = NULL)
- get cols by vars()

```{r, results="hide"}
iris %>% mutate_at(vars(iris %>% names() %>% str_subset(., "^Sep")), scale2) %>% summarize(mean(Sepal.Length))

# mutates in-addition instead of in-place with list()
iris %>% mutate_at(vars(iris %>% names() %>% str_subset(., "^Sep")), list(scale = scale2)) %>% head()
```

### multiple transformations
```{r, results="hide"}
# new variables created for each transformation
iris %>% mutate_if(is.numeric, list(scale = scale2, log = log)) %>% head()
```

## transmute
- compute new columns, drops all others

```{r, results="hide"}
iris %>% transmute(sepal10 = Sepal.Length*10) %>% head()
```

## arrange
- ordering by multiple cols

```{r, results="hide"}
iris %>% arrange(Sepal.Length, Sepal.Width) %>% head()
iris %>% arrange(desc(Sepal.Length), Sepal.Width) %>% head() 

# group_by() is ignored
iris %>% group_by(Species) %>% arrange(Sepal.Length) %>% print(n=40)
# unless specifically asked
iris %>% arrange(Sepal.Length, .by_group = TRUE) %>% head(50) # even then doesn't work...
```

## count
```{r, results="hide"}
# count is short-hand for group_by() + tally()
iris %>% group_by(Species) %>% tally()
iris %>% count(Species)

# change name of new col
iris %>% count(Species, name = "count_Species")

# multi-level count (e.g. by Species and by homeworld)
starwars %>% count(species, homeworld, sort = TRUE, name = "n_by_Species_by_homeworld")

# add_count short for group_by() + add_tally(), puts a few col with #rows of entire dataframe
mtcars %>% add_count(cyl, name = "count_num_by_cyl") %>% select(count_num_by_cyl, everything()) %>% head(10)

# combine add_count with filter, to filter based on counts of each class of a feature
mtcars %>% add_count(cyl, name = "count_num_by_cyl") %>% filter(count_num_by_cyl == 7) %>% head()
```