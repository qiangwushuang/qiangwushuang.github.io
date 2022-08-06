---
layout: post
title: WindowsServer2022安装3DExperiencePlatformR2019X
date: 2022-07-28
Author: qiangwushuang 
tags: [Windows Server 2022,3DExperiencePlatformR2019x,Install]
comments: true
toc: false
---

# 说明
此文章记录使用Windows Server 2022操作系统安装整个3DExperience Platform R2019X的过程，使用个人零碎时间安装，因此安装可能会持续很长一段时间。  
文章会尽量详细记录从操作系统安装到3DE安装完成的整个过程，如安装遇到问题也会在下面进行详细记录解决方法。因此整个文章可能会比较乱。
## 一些缩写  
|文本|缩写|说明|
|-|-|-|
|3DExperience_Platform_R2019x|3DE||
|Windows Server 2022|Windows||
|VMWare Workstation Pro 16 |VMWare||
|MSSQL Server|SQLServer||

# 安装介质
## 操作系统  
* Windows Server 2022 
## 3DExperiencePlatform  
V6R2019x.AM_3DEXP_Platform.AllOS.1-9.tar  
V6R2019x.AM_3DEXP_Platform.AllOS.2-9.tar  
V6R2019x.AM_3DEXP_Platform.AllOS.3-9.tar  
V6R2019x.AM_3DEXP_Platform.AllOS.4-9.tar  
V6R2019x.AM_3DEXP_Platform.AllOS.5-9.tar  
V6R2019x.AM_3DEXP_Platform.AllOS.6-9.tar  
V6R2019x.AM_3DEXP_Platform.AllOS.7-9.tar  
V6R2019x.AM_3DEXP_Platform.AllOS.8-9.tar  
V6R2019x.AM_3DEXP_Platform.AllOS.9-9.tar  
# 安装方式
VMware Workstation Pro 16 虚拟机安装

# 安装过程
## Day1  

1. 安装Windows Server 2022操作系统  
在VMWare中安装，安装过程略
2. 检查包是否完整  
检查工具路径：\AM_3DEXP_Platform.AllOS\1\0data\intel_a\DSYinsMediaCheck.exe，双击运行检查程序，通过有PASS提示。
3. 使用DS Installer工具进行安装  
工具路径："\AM_3DEXP_Platform.AllOS\1\setup.exe"


### SQLServer数据库配置  
3DNotification数据库
```shell
USE master;
-- Create database
CREATE DATABASE x3dnotification COLLATE SQL_Latin1_General_CP1_CI_AS;
-- Minimizing deadlock potential
ALTER DATABASE x3dnotification SET READ_COMMITTED_SNAPSHOT ON;
-- Create logins
CREATE LOGIN x3ds WITH PASSWORD = 'Asdf1234';
CREATE LOGIN x3ds_admin WITH PASSWORD = 'Asdf1234';
USE x3dnotification;
-- Create users on correct DATABASE (after use) and with correct schema
CREATE USER x3ds FOR LOGIN x3ds WITH DEFAULT_SCHEMA = x3dnotification;
CREATE USER x3ds_admin FOR LOGIN x3ds_admin WITH DEFAULT_SCHEMA = x3dnotification;
-- Create schema
CREATE SCHEMA x3dnotification AUTHORIZATION x3ds_admin;
-- Grant access
GRANT CREATE TABLE, ALTER, SELECT, INSERT, UPDATE, DELETE ON DATABASE::x3dnotification TO x3ds_admin;
GRANT SELECT, INSERT, UPDATE, DELETE ON SCHEMA::x3dnotification TO x3ds;
```

3DComment数据库
```shell
USE master;
-- Create database
CREATE DATABASE x3dcomment COLLATE SQL_Latin1_General_CP1_CI_AS;
-- Minimizing deadlock potential
ALTER DATABASE x3dcomment SET READ_COMMITTED_SNAPSHOT ON;
-- Create logins
CREATE LOGIN x3ds WITH PASSWORD = 'Asdf1234';
CREATE LOGIN x3ds_admin WITH PASSWORD = 'Asdf1234';
USE x3dcomment;
-- Create users on correct DATABASE (after use) and with correct schema
CREATE USER x3ds FOR LOGIN x3ds WITH DEFAULT_SCHEMA = x3dcomment;
CREATE USER x3ds_admin FOR LOGIN x3ds_admin WITH DEFAULT_SCHEMA = x3dcomment;
-- Create schema
CREATE SCHEMA x3dcomment AUTHORIZATION x3ds_admin;
-- Grant access
GRANT CREATE TABLE, ALTER, SELECT, INSERT, UPDATE, DELETE ON DATABASE::x3dcomment TO x3ds_admin;
GRANT SELECT, INSERT, UPDATE, DELETE ON SCHEMA::x3dcomment TO x3ds;
```

3DSwym
```shell
USE master;
-- Create databases
CREATE DATABASE x3dswym_social COLLATE SQL_Latin1_General_CP1_CI_AS;
CREATE DATABASE x3dswym_media COLLATE SQL_Latin1_General_CP1_CI_AS;
CREATE DATABASE x3dswym_widget COLLATE SQL_Latin1_General_CP1_CI_AS;
-- Minimizing deadlock potential
ALTER DATABASE x3dswym_social SET READ_COMMITTED_SNAPSHOT ON;
ALTER DATABASE x3dswym_media SET READ_COMMITTED_SNAPSHOT ON;
ALTER DATABASE x3dswym_widget SET READ_COMMITTED_SNAPSHOT ON;
-- Set database compatibility level to 110
ALTER DATABASE x3dswym_social SET COMPATIBILITY_LEVEL = 110;
-- Create logins
CREATE LOGIN x3ds WITH PASSWORD = 'Asdf1234';
CREATE LOGIN x3ds_admin WITH PASSWORD = 'Asdf1234';

USE x3dswym_social;
-- Create users on correct DATABASE (after use) and with correct schema
CREATE USER x3ds FOR LOGIN x3ds WITH DEFAULT_SCHEMA = x3dswym_social;
CREATE USER x3ds_admin FOR LOGIN x3ds_admin WITH DEFAULT_SCHEMA = x3dswym_social;
-- Create schema
CREATE SCHEMA x3dswym_social AUTHORIZATION x3ds_admin;
-- Grant access
GRANT CREATE TABLE, ALTER, SELECT, INSERT, UPDATE, DELETE ON DATABASE::x3dswym_social TO x3ds_admin;
GRANT SELECT, INSERT, UPDATE, DELETE ON SCHEMA::x3dswym_social TO x3ds;
USE x3dswym_media;
-- Create users on correct DATABASE (after use) and with correct schema
CREATE USER x3ds FOR LOGIN x3ds WITH DEFAULT_SCHEMA = x3dswym_media;
CREATE USER x3ds_admin FOR LOGIN x3ds_admin WITH DEFAULT_SCHEMA = x3dswym_media;
-- Create schema
CREATE SCHEMA x3dswym_media AUTHORIZATION x3ds_admin;
-- Grant access
GRANT CREATE TABLE, ALTER, SELECT, INSERT, UPDATE, DELETE ON DATABASE::x3dswym_media TO x3ds_admin;
GRANT CREATE TABLE ON DATABASE::x3dswym_media TO x3ds;
GRANT SELECT, INSERT, UPDATE, DELETE, ALTER ON SCHEMA::x3dswym_media TO x3ds;
USE x3dswym_widget;
-- Create users on correct DATABASE (after use) and with correct schema
CREATE USER x3ds FOR LOGIN x3ds WITH DEFAULT_SCHEMA = x3dswym_widget;
CREATE USER x3ds_admin FOR LOGIN x3ds_admin WITH DEFAULT_SCHEMA = x3dswym_widget;
-- Create schema
CREATE SCHEMA x3dswym_widget AUTHORIZATION x3ds_admin;
-- Grant access
GRANT CREATE TABLE, ALTER, SELECT, INSERT, UPDATE, DELETE ON DATABASE::x3dswym_widget TO x3ds_admin;
GRANT SELECT, INSERT, UPDATE, DELETE, ALTER ON SCHEMA::x3dswym_widget TO x3ds;
```