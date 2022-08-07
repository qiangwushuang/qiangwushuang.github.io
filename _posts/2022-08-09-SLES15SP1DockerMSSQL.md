---
layout: post
title: SLES15SP1Docker下运行mssql2017
date: 2022-08-09
Author: qiangwushuang 
tags: [SUSE Linux Enterprise Server 15 SP1,Docker,MSSQL2017]
comments: true
toc: false
---

1. SLES15SP1安装Docker  
[SLES官方文档](https://documentation.suse.com/sles/15-SP1/html/SLES-all/cha-docker-installation.html)  
```bash
sudo SUSEConnect -p sle-module-containers/15.1/x86_64 -r ''

sudo zypper install docker

sudo systemctl enable docker.service

sudo systemctl start docker.service
```
2. Docker 运行MSSQL2017  
[微软文档](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-linux-2017&preserve-view=true&pivots=cs1-bash#pullandrun2017)  
```bash
sudo docker pull mcr.microsoft.com/mssql/server:2017-latest

sudo docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Asdf1234" \
   -p 1433:1433 --name sql1 --hostname sql1 \
   -d \
   mcr.microsoft.com/mssql/server:2017-latest

```