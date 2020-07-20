---
date: "2020-05-23T00:00:00+01:00"
draft: false
linktitle: networking
menu:
  linux_resources:
    name: networking
    weight: 100
title: networking
toc: true
type: docs
weight: 100
---

<br>

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

<br>

## lsof
- find open port and kill process
```bash
$ lsof -i:8000
> COMMAND   PID    USER    FD  TYPE  DEVICE               SIZE/OFF  NODE  NAME
  ssh       71504  minghan 7u  IPv6  0x570e3d71af3c38d9   0t0       TCP   localhost:irdmi (LISTEN)

$ kill 71504
```






<br>

## EOF