---
layout: post
title: 新浪相册作为图床使用
date: 2021-02-24
Author: qiangwushuang 
tags: [图床]
comments: true
toc: false
---

在浏览某网站的时候发现网站的图片都是https://\*tva4\*.sinaimg.cn/\*/\*.jpg格式的，访问速度非常快，研究下发现原来网站把新浪相册的图片作为了自己的图床使用，这个思路非常好。  
下面实测一下如何使用新浪微博作为图床。  
- 登录新浪相册[https://photo.weibo.com](https://photo.weibo.com/)  
- 选择上传  
![上传](https://image.baidu.com/search/down?url=https://tva1.sinaimg.cn/large/8343d05bly1gnyf69dx8hj20ko0793zs.jpg)
- 选择或者新建一个专辑
![专辑](https://image.baidu.com/search/down?url=https://tva1.sinaimg.cn/large/8343d05bly1gnyf65fyj4j20jn080t8q.jpg)
因flashplayer已经被抛弃，因此可以使用普通上传方式上传图片  
- 选择图片上传
![上传](https://image.baidu.com/search/down?url=https://tva1.sinaimg.cn/large/8343d05bly1gnyfb8qyqsj20f506xq31.jpg)
- 保存发布
![保存并发布](https://image.baidu.com/search/down?url=https://tva1.sinaimg.cn/large/8343d05bly1gnyfc8a2kfj20iq0bqt9x.jpg)
- 查看图片链接
![图片链接](https://image.baidu.com/search/down?url=https://tva1.sinaimg.cn/large/8343d05bly1gnyfbcmcvwj20jt094n1a.jpg)
得到的图片链接为
> https://wx1(1/2/3/4).sinaimg.cn/\*/\*.jpg (默认的缩略图)  
因wx1/wx2/wx3/wx4不允许图片外联，可以将图片前缀修改为下面的任意内容：
```
tva1.sinaimg.com、 tva2.sinaimg.com、 tva3.sinaimg.com、 tva4.sinaimg.com
tvax1.sinaimg.com、tvax2.sinaimg.com、tvax3.sinaimg.com、tvax4.sinaimg.com
```  
因此可用的外链为(前缀可以任意替换)  
> https://\*.sinaimg.cn/small/\*.jpg (小图)  
> https://\*.sinaimg.cn/bmiddle/\*.jpg (中图)  
> https://\*.sinaimg.cn/large/\*.jpg (大图)  