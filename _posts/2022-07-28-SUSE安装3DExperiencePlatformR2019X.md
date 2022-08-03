---
layout: post
title: SUSE安装3DExperiencePlatformR2019X
date: 2022-07-28
Author: qiangwushuang 
tags: [SUSE,3DExperiencePlatformR2019x,Install]
comments: true
toc: false
---

# 说明
此文章记录使用SUSE Linux Enterprise Server操作系统安装整个3DExperience Platform R2019X的过程，使用个人零碎时间安装，因此安装可能会持续很长一段时间。  
文章会尽量详细记录从操作系统安装到3DE安装完成的整个过程，如安装遇到问题也会在下面进行详细记录解决方法。因此整个文章可能会比较乱。
## 一些缩写  
|文本|缩写|说明|
|-|-|-|
|3DExperience_Platform_R2019x|3DE||
|SUSE Linux Enterprise Server 15 SP4|SUSE||
|VMWare Workstation Pro 16 |VMWare||
|MSSQL Server|SQLServer||

# 安装介质
## 操作系统  
[SUSE Linux Enterprise Server 15 SP4](https://www.suse.com/download/sles/)  
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
### SUSE操作系统安装  
与其他操作系统安装过程类似，仅记录安装过程中遇到的一些小问题。  
#### 问题记录  
1. 使用VMWARE默认LEGACY BIOS固件情况下,挂载SUSE光盘1进行安装时，无法使用键盘进行输入，更改为UEFI BIOS后可以进行输入。<font color='red'>问题原因未知</font>
2. SUSE安装后需要进行注册否则无法使用zypper安装软件，可在[SUSE官网](https://www.suse.com/download/sles/)申请60天的试用。  
下面是安装后使用命令行注册方法，也可在安装过程中输入ActivationCode和EmailAddress。
```shell
SUSEConnect -r <ActivationCode> -e <EmailAddress>
```
### 3DE安装
1. 使用tar解包安装包
```shell
tar -xvf V6R2019x.AM*.tar
```
2. 安装依赖
```shell
sudo zypper install ksh lsb-release
```
3. 检查包是否完整  
```shell
./AM_3DEXP_Platform.AllOS/1/0data/linux_a64/DSYInsMediaCheck
```
完整情况下输出：
```shell
Media check completed
Status: PASSED
```
4. 安装MSSQL Server数据库（可选Oracle），选择MSSQL Server纯粹是个人好奇，之前在windows平台一直使用的是Oracle  
4.1. 微软官方提供了详尽的安装[文档](https://docs.microsoft.com/zh-cn/sql/linux/quickstart-install-connect-suse?view=sql-server-ver15)  
<font color='red'>安装过程中遇到的问题</font>  
文档中第3步，安装时提示gdb找不到，因此需要手动安装gdb到SUSE。  
选择的gdb版本为：[gdb-12.1.tar.gz](http://ftp.gnu.org/gnu/gdb/gdb-12.1.tar.gz)  
gdb安装过程记录：
```shell
wget http://ftp.gnu.org/gnu/gdb/gdb-12.1.tar.gz
tar -zxvf gdb-12.1.tar.gz
sudo zypper install make gcc gcc-c++ gmp-devel    #安装编译工具
./configure
make
sudo make install
```
安装gdb后依旧提示gdb错误，因此安装SQLServer过程中选择断开部分依赖安装SQLServer(不确定是否会出现问题)，以后还是尽量不要选择最新发布的系统版本，经查询GDB还没有支持SLES15 SP4😓手动安装gdb后sql server运行正常。  
SQL Server的一些配置：  

3DPassport数据库
```shell
#登录SQLServer
sqlcmd -S localhost -U sa -P 'Qwerty12345' 

#3DPassport数据库
use master;

#创建passdb数据库
CREATE DATABASE passdb COLLATE SQL_Latin1_General_CP1_CI_AS;

#最大限度减小死锁
ALTER DATABASE passdb SET READ_COMMITTED_SNAPSHOT ON;

#创建登录用户
CREATE LOGIN x3dpass WITH PASSWORD = 'Qwerty12345';
CREATE LOGIN x3dpassadmin WITH PASSWORD = 'Qwerty12345';

USE passdb;

#数据库中创建用户
CREATE USER x3dpass FOR LOGIN x3dpass WITH DEFAULT_SCHEMA = passdb;
CREATE USER x3dpassadmin FOR LOGIN x3dpassadmin WITH DEFAULT_SCHEMA = passdb;

#创建schema
CREATE SCHEMA passdb AUTHORIZATION x3dpassadmin;

#配置权限
GRANT CREATE TABLE, ALTER, SELECT, INSERT, UPDATE, DELETE ON DATABASE::passdb TO x3dpassadmin;
GRANT SELECT, INSERT, UPDATE, DELETE, ALTER ON SCHEMA::passdb TO x3dpass;

#创建令牌数据库
CREATE DATABASE passtkdb COLLATE SQL_Latin1_General_CP1_CI_AS;
#同上
ALTER DATABASE passtkdb SET READ_COMMITTED_SNAPSHOT ON;

CREATE LOGIN x3dpasstokens WITH PASSWORD = 'Qwerty12345';

USE passtkdb;

CREATE USER x3dpasstokens FOR LOGIN x3dpasstokens WITH DEFAULT_SCHEMA = passtkdb;

CREATE SCHEMA passtkdb AUTHORIZATION x3dpasstokens;

GRANT CREATE TABLE, ALTER, SELECT, INSERT, UPDATE, DELETE ON DATABASE::passtkdb TO x3dpasstokens;

```  

3DDashbord数据库
```shell
#3DDashbord数据库
USE master;

#创建dashdb数据库
CREATE DATABASE dashdb COLLATE SQL_Latin1_General_CP1_CI_AS;

ALTER DATABASE dashdb SET READ_COMMITTED_SNAPSHOT ON;

CREATE LOGIN x3ddash WITH PASSWORD = 'Qwerty12345';
CREATE LOGIN x3ddashadmin WITH PASSWORD = 'Qwerty12345';
USE dashdb;
go;
CREATE USER x3ddash FOR LOGIN x3ddash WITH DEFAULT_SCHEMA = dashdb;
CREATE USER x3ddashadmin FOR LOGIN x3ddashadmin WITH DEFAULT_SCHEMA = dashdb;

CREATE SCHEMA dashdb AUTHORIZATION x3ddashadmin;

GRANT CREATE TABLE, ALTER, SELECT, INSERT, UPDATE, DELETE ON DATABASE::dashdb TO x3ddashadmin;
GRANT SELECT, INSERT, UPDATE, DELETE, ALTER ON SCHEMA::dashdb TO x3ddash;
```


5. 安装
```shell
mkdir /var/DassaultSystemes #创建DassaultSystems目录，使用StartTUI.sh安装过程中提示必须预先创建此目录。
cd ./AM_3DEXP_Platform.AllOS/1/
./StartTUI.sh
```

今天安装结束。

## Day2
### 数据库配置

### 3DE安装
1. 3DSearch
只需要配置域名，其他采用默认选项。
2. 3DSpace  

😓在安装3DSpaceIndex的时候使用默认选项一直失败，重试多次没有解决。
尝试重新安装SLES 

1. 安装SLES过程和配置同Day1  
#### 有变化的内容
1. SQLServer采用docker方式进行安装  
<font color='red'>SLES在x86架构上竟然没有可用的源，只用通过二进制的方式进行安装。</font>  

## Day3(非第三天)  
老婆生产出院了，恭喜我家多了一枚小公主👸。
### 安装SLES15SP1
#### 新进展  
在[SUSE官方文档](https://documentation.suse.com/sles/15-SP2/single-html/SLES-container/index.html#cha-containers-basics)里面找到了这一篇文章介绍了如何在SLES上安装Docker的方式。其中4.1使用YaST启用容器模块貌似不是必须的，另外SUSEConnect中的版本号可以根据实际系统情况自己配置。
#### Docker方式安装SQLServer  
查看[微软官方文档](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-ver16&pivots=cs1-bash)

### 继续之前的安装（3DDashbord以及3DPassport查看Day1）


<font color='red'>重新安装依旧失败，因此放弃SUSE安装，重新开贴使用WindowsServer2022进行安装</font>  
错误内容：
![3DSpaceIndex_Install_Error](https://tva1.sinaimg.cn/large/8343d05bgy1h4tsup31imj210a0d0ab9.jpg)

### 尝试使用Red Hat Enterprise Linux 7.9安装
<font color='red'>重新安装依旧失败，因此放弃SUSE安装，重新开贴使用WindowsServer2022进行安装</font> 


