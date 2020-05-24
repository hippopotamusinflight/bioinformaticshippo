---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: networking
menu:
  linux_resources:
    name: networking
    weight: 20
title: networking
toc: true
type: docs
weight: 20
---

<!--
1. replace linux_resources with dir in /content/subdir/ e.g. r_resources
2. replace 2020-05-23 with YYYY-MM-DD e.g. 2020-05-20
3. replace ssh with page name e.g. dplyr
4. replace 20 with weight e.g. 20
-->

## ssh flags
- [https://www.ssh.com/ssh/command](https://www.ssh.com/ssh/command)
```bash
-L [bind address]    # port:host:hostport (e.g. 8000:localhost:8888)
-N                   # do not execute remote commands
-f                   # requests ssh to go to background before command executes
-p port              # port to connect to on remote host (e.g. -p 8000)
-q                   # quiet mode
-X                   # enable X11 forwarding
```

## lsof
- find open port and kill process
```bash
$ lsof -i:8000
> COMMAND   PID    USER    FD  TYPE  DEVICE               SIZE/OFF  NODE  NAME
  ssh       71504  minghan 7u  IPv6  0x570e3d71af3c38d9   0t0       TCP   localhost:irdmi (LISTEN)

$ kill 71504
```








## EOF