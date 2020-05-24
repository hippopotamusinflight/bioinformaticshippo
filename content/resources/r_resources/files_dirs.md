---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: files and directories
menu:
  r_resources:
    name: files and directories
    weight: 1020
title: files and directories
toc: true
type: docs
weight: 1020
---

<!--
1. replace r_resources with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-DD e.g. 2020-05-20
3. replace files and directories with page name e.g. dplyr
4. replace 1020 with weight e.g. 20
-->

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
knitr::opts_chunk$set(message = FALSE)
```

[http://theautomatic.net/2018/07/11/manipulate-files-r/](http://theautomatic.net/2018/07/11/manipulate-files-r/)<br>
[https://astrostatistics.psu.edu/su07/R/html/base/html/files.html](https://astrostatistics.psu.edu/su07/R/html/base/html/files.html)


### get and change working dir
```{r}
getwd()
setwd("/path/to/wd/")
```

using "here" package
```{r}
library(here)
here::here()
here::set_here()
```

### check if files, dirs exists
```{r}
file.exists("test.txt") # [1] FALSE
dir.exists("test_dir") # [1] TRUE
```

### create files, dirs
```{r}
dir.create("test_dir")
file.create(file.path("test_dir", "test.txt"), overwrite = TRUE)
file.create(file.path("test_dir", "test.csv"))

# create a whole bunch of files
file.path("test_dir", paste0("file", 1:5, ".txt"))
sapply(file.path("test_dir", paste0("file", 1:5, ".txt")), file.create)
```

### edit file
```{r}
file.edit(file.path("test_dir", "test.txt"))
```

### delete files
```{r}
file.remove(file.path("test_dir", "test.txt"))
dir.create(file.path("test_dir", "to_be_removed"))
unlink(file.path("test_dir", "to_be_removed"), recursive = TRUE)
```

### copy files, dirs
```{r}
dir.create(file.path("test_dir", "inner_dir"))
file.copy(file.path("test_dir", "test.txt"), file.path("test_dir", "inner_dir"), overwrite = TRUE)
```

### symlink
```{r}
file.symlink(from, to)
```


### list files in dir
```{r}
list.files(file.path("test_dir"))
list.files(file.path("test_dir"), recursive = TRUE) # lists files in inner_dir
list.files(file.path("test_dir"), recursive = TRUE, full.names = TRUE) # get full path, starting from current dir

# use pattern to subset list.files() results
list.files(file.path("test_dir"), pattern = ".csv")
```

### get file info
```{r}
file.info(file.path("test_dir", "test.txt"))
#                   size isdir mode               mtime               ctime               atime uid gid   uname grname
# test_dir/test.txt    0 FALSE  644 2020-05-04 17:46:56 2020-05-04 17:46:56 2020-05-04 17:52:52 501  20 minghan  staff

file.mtime(file.path("test_dir", "test.txt")) # [1] "2020-05-04 17:46:56 EDT"
# file.ctime(file.path("test_dir", "test.txt")) # no such function...
file.info(file.path("test_dir", "test.txt"))$ctime # [1] "2020-05-04 17:46:56 EDT"
```

### get file basename
```{r}
basename(file.path("test_dir", "test.csv"))
```

### get dir name of a file
```{r}
dirname(file.path("test_dir", "test.csv"))
```

### get file extension
```{r}
library(tools)
file_ext(file.path("test_dir", "test.csv"))
```

### open a GUI window to select file
```{r}
# file.choose()
```

### move a file
```{r}
# install.packages("filesstrings")
library(filesstrings)
file.move(file.path("test_dir", "file1.txt"), file.path("test_dir", "inner_dir"))
```

### EOF