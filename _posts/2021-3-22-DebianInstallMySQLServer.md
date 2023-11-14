---
layout: post
title: Debian安装MySQL Server
date: 2021-03-22
Author: qiangwushuang 
tags: [debian,mysql]
comments: true
toc: false
---

1. 配置MySQL PPA  
MySQL团队为Debian提供了官方的[MySQL PPA](http://repo.mysql.com/)，到现在为止最新版本为[0.8.16](http://repo.mysql.com/mysql-apt-config_0.8.16-1_all.deb)  
```bash
wget http://repo.mysql.com/mysql-apt-config_0.8.16-1_all.deb
```
2. 安装MySQL APT Config包  
```bash
sudo dpkg -i mysql-apt-config_0.8.16-1_all.deb
```  
![AptInstall1](https://image.baidu.com/search/down?url=https://tva1.sinaimg.cn/large/8343d05bly1gosocrt87zj20qd0aw74e.jpg)  
![SelectVersion](https://image.baidu.com/search/down?url=https://tva1.sinaimg.cn/large/8343d05bly1gosocw9mrqj20qh0b2wej.jpg)  
3. 更新软件源  
```shell
sudo apt update
```
4. 安装MySQL Server    
```bash
sudo apt install mysql-server
```  
配置数据库密码  
![set passwd](https://image.baidu.com/search/down?url=https://tva1.sinaimg.cn/large/8343d05bly1gosocyp1qkj20qe098746.jpg)

5. 启动MySQL Server  
```bash
sudo systemctl restart mysql.service
```  