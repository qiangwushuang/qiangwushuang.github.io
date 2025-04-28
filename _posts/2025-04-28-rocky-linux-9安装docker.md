---
layout: post
title: rocky linux 9 安装docker-ce
date: 2025-04-28
Author: qiangwushuang 
tags: [rocky linux,docker,docker-ce]
comments: true
toc: false
---

在Rocky Linux 9.x上安装docker-ce  
1. 可选操作，切换为阿里云源
```shell  
sed -e 's|^mirrorlist=|#mirrorlist=|g' \
    -e 's|^#baseurl=http://dl.rockylinux.org/$contentdir|baseurl=https://mirrors.aliyun.com/rockylinux|g' \
    -i.bak \
    /etc/yum.repos.d/rocky*.repo

```
2. 安装docker-ce
```shell
# 添加Docker Repo
dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
```  
执行上面步骤的时候有可能报错：  
```shell
No such command: config-manager. Please use /bin/dnf --help
It could be a DNF plugin command, try: "dnf install 'dnf-command(config-manager)'"
```  
需要首先安装 dnf-plugins-core
```shell 
dnf install dnf-plugins-core
```  
然后使用config-manager重新添加docker repo。  

```shell
# 更新源
dnf update

# 安装docker-ce
dnf install -y docker-ce
```  
完成。