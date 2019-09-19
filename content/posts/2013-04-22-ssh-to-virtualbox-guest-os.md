---
title: "ssh to virtualbox guest OS"
date: "2013-04-22"
template: "post"
draft: false
slug: "/posts/ssh-virtualbox-guest-os/"
category: "linux"
tags:
  - "linux"
description: ""
socialImage: ""
---

1. `sudo apt-get install ssh`


2. make sure ssh service is working
    `netstat -tlp`

   you should see following line:

```shell
tcp        0      0 *:ssh                   *:*                     LISTEN
```
3. change virtualbox network setting to: Bridged Adapter with vnic0

4. `ifconfig`
    make sure host OS vnic0 address is in the same net with guest OS p2p1

