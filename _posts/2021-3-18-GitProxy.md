---
layout: post
title: git设置代理
date: 2021-03-15
Author: qiangwushuang 
tags: [git,proxy]
comments: true
toc: false
---

1. 配置http代理  
   ```shell
   git config --global http.proxy http://127.0.0.1:1080
   git config --global https.proxy http(s)://127.0.0.1:1080
   ```  
2. 配置socks代理  
   ```shell
   git config --global http.proxy 'socks5://127.0.0.1:1080'
   git config --global https.proxy 'socks5://127.0.0.1:1080'
   ```
3. 取消代理  
   ```shell
   git config --global --unset http.proxy
   git config --global --unset https.proxy
   ```