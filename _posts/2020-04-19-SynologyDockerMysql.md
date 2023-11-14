---
layout: post
title: 群晖Docker安装MySQL及初始配置
date: 2022-04-19
Author: qiangwushuang 
tags: [docker,mysql]
comments: true
toc: false
---

## 一、安装Docker套件
套件中心直接安装即可
## 二、下载mysql-server映像
1. 打开Docker套件
2. 切换到注册表选项卡，并搜索MySQL
![搜索](https://i0.wp.com/tva1.sinaimg.cn/large/8343d05bgy1h1fdwdstohj20t90fpgo6.jpg)
3. 下载
![下载](https://i0.wp.com/tva1.sinaimg.cn/large/8343d05bgy1h1fdzdcgkgj20tg0flju2.jpg)
## 三、启动映像
1. 启动
切换到映像选项卡，并找到下载的mysql/mysql-server映像，使用页面上方的启动按钮启动映像
![启动](https://i0.wp.com/tva1.sinaimg.cn/large/8343d05bgy1h1fe17yxqmj20tg0fm0v3.jpg)
2. 端口设置
在高级设置中切换到端口设置选项卡，并配置本地端口
![端口设置](https://i0.wp.com/tva1.sinaimg.cn/large/8343d05bgy1h1fe66qy7ej20i00fa0tb.jpg)
3. 环境配置
此配置设置ROOT用户密码
```shell
MYSQL_ROOT_PASSWORD=123456
```
![环境设置](https://i0.wp.com/tva1.sinaimg.cn/large/8343d05bgy1h1fe8fro44j20hy0f7my0.jpg)
4. 应用后启动
## 四、启用远程连接
1. 群晖Docker中切换到容器选项卡，选择mysql-server容器，点击详情，切换到终端机选项卡，并新增终端
![详情](https://i0.wp.com/tva1.sinaimg.cn/large/8343d05bgy1h1ff2z4e6cj20tj0ylq5d.jpg)
2. 在终端中输入以下命令
```shell
mysql -uroot -p #登录
use mysql #切换到mysql数据库
update user set host='%' where user = 'root'; #启用root用户任意位置登录
flush privileges;   #更新权限
```  
随后就可以使用远程机器登录mysql-server