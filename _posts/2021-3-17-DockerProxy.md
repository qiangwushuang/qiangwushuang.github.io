---
layout: post
title: Docker设置代理
date: 2021-03-15
Author: qiangwushuang 
tags: [docker,proxy]
comments: true
toc: false
---

> 文章内容引用自:  
> [https://docs.docker.com/config/daemon/systemd/#httphttps-proxy](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)  

1. 在systemd中创建docker的systemd目录 
   ```bash
   sudo mkdir -p /etc/systemd/system/docker.service.d
   ``` 
2. 在docker.service.d中创建http-proxy.conf文件  
   ```bash
   sudo touch /etc/systemd/system/docker.service.d/http-proxy.conf
   ```  
3. 写入文件内容
   ```text
   [Service]
   Environment="HTTP_PROXY=http://proxy.example.com:80"
   ```  
   同样若存在https代理则使用  
   ```text
   [Service]
   Environment="HTTPS_PROXY=https://proxy.example.com:80"
   ```  
4. 重启Docker服务   
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl restart docker
   ```  
5. 验证配置是否加载  
   ```bash
   sudo systemctl show --property=Environment docker
   ```  
   正确加载会输出下面内容
   ```text
   Environment=HTTP_PROXY=http://proxy.example.com:80 HTTPS_PROXY=https://proxy.example.com:443 NO_PROXY=localhost,127.0.0.1,docker-registry.example.com,.corp
   ```
