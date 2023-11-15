---
layout: post
title: 解决非root用户操作docker无权限问题
date: 2023-11-13
Author: qws 
tags: [wsl,docker]
comments: true
toc: false
---

将当前用户加入到docker用户组
```shell
sudo usermod -aG docker ${USER}
sudo newgrp docker
```
