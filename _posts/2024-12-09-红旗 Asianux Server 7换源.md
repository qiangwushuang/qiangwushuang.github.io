---
layout: post
title: 红旗 Asianux Server 7换源
date: 2024-12-09
Author: qws 
tags: [红旗,linux,Asianux Server 7]
comments: true
toc: false
---

1. 查看redhat版本  
```shell  
cat /etc/redhat-release
CentOS Linux release 7.3.1611 (Core)
```  
2. 卸载yum  
```shell  
rpm -aq | grep yum | xargs rpm -e --nodeps
rpm -qa |grep python-urlgrabber|xargs rpm -e --nodeps
rpm -qa |grep python-iniparse|xargs rpm -e --nodeps
rm -rf /etc/yum
```  
3. 下载centos7版本yum  
```shell  
wget https://repo.huaweicloud.com/centos-vault/7.3.1611/os/x86_64/Packages/python-iniparse-0.4-9.el7.noarch.rpm
wget https://repo.huaweicloud.com/centos-vault/7.3.1611/os/x86_64/Packages/python-urlgrabber-3.10-8.el7.noarch.rpm
wget https://repo.huaweicloud.com/centos-vault/7.3.1611/os/x86_64/Packages/yum-3.4.3-150.el7.centos.noarch.rpm
wget https://repo.huaweicloud.com/centos-vault/7.3.1611/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
wget https://repo.huaweicloud.com/centos-vault/7.3.1611/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-40.el7.noarch.rpm
```  
4. 安装yum  
```shell  
rpm -ivh *.rpm
```  
5. 切换源  
> 此处使用华为源  
```shell  
curl https://mirrors.huaweicloud.com/artifactory/os-conf/centos/centos-7.repo >> /etc/yum.repos.d/CentOS-Base.repo
yum clan all
yum makecache
```