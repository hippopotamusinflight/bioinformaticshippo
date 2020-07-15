---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: using image
menu:
  docker_resources:
    name: using image
    weight: 10
title: using image
toc: true
type: docs
weight: 10
---

<!--
1. replace docker_resources with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-DD e.g. 2020-05-20
3. replace using image with page name e.g. dplyr
4. replace 10 with weight e.g. 20
-->

## example
```
$ docker run -it -v ~/some_dir:/gatk/some_dir -rm broadinstitute/gatk4
(gatk) root@f674e9d906ce:/gatk# 
```

## commands
```bash
$ docker run -it                     # creates interactive shell in container
$ docker run -v /dir:/gatk/dir       # mount volume

$ docker pull [IMAGE]                # pull an image from a registry
$ docker images                      # lists docker images pulled
$ docker run --rm [IMAGE]            # removes a container automatically after it exits (auto clean up)
$ docker ps                          # list docker processes still running
$ exit
```




## EOF