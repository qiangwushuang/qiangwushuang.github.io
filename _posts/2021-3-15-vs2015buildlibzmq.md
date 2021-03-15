---
layout: post
title: vs2015编译libzmq
date: 2021-03-15
Author: qiangwushuang 
tags: [zmq][build][visual studio]
comments: true
toc: true
---

ZeroMq是一个开源的消息队列网络框架，支持进程内和进程间的通信。  
> 项目地址:[https://github.com/zeromq0](https://github.com/zeromq)  
> libzmq源码地址:[https://github.com/zeromq/libzmq](https://github.com/zeromq/libzmq)  

下面使用vs2015编译当前[最新稳定版本](https://github.com/zeromq/libzmq/releases/download/v4.3.4/zeromq-4.3.4.zip)  

1. 下载zeromq4.3.4  
2. 使用vs2015打开  
```注意新版zeromq已经不建议使用vs直接进行编译```  
手动修改文件：.\builds\deprecated-msvc\vs2015\libzmq\libzmq.vcxproj  
将文件里面的stream_engine.hpp 修改为 stream_engine_base.hpp  
![img1](https://tva1.sinaimg.cn/large/8343d05bgy1gokj9dr3rzj20i305uwf4.jpg)  
打开项目文件：.\builds\deprecated-msvc\vs2015\libzmq.sln  
3. 配置宏  
ZMQ_IOTHREAD_POLLER_USE_SELECT  
ZMQ_POLL_BASED_ON_SELECT  
ZMQ_USE_CV_IMPL_WIN32API  
![定义宏](https://tva1.sinaimg.cn/large/8343d05bgy1gokjbs6dd5j21080ahdgd.jpg)  
4. 编译成功  