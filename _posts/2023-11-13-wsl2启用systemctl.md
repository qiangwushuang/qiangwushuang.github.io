---
layout: post
title: wsl2启用systemctl
date: 2023-11-12
Author: qws 
tags: [wsl,systemctl]
comments: true
toc: false
---
1. linux下配置wsl.conf文件，添加systemd配置项
```shell
sudo vim /etc/wsl.conf
``` 
```conf
[boot]
systemd=true
```
2. win重启linux子系统
```cmd
wsl.exe --shutdown
```
