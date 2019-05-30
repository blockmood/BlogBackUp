---
title: mysql Can’t connect to local MySQL server through socket
date: 2019-05-30 08:12:24
tags: Centos
---

## centos7 下解决mysql-server找不到安装包问题

### 第一步：安装从网上下载文件的wget命令
```
[root@master ~]# yum -y install wget
```
### 第二步：下载mysql的repo源
```
[root@master ~]# wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm 
```
### 第三步：安装mysql-community-release-el7-5.noarch.rpm包

```
[root@master ~]# rpm -ivh mysql-community-release-el7-5.noarch.rpm
```

### 第四步：查看下

```
[root@master ~]# ls -1 /etc/yum.repos.d/mysql-community*
/etc/yum.repos.d/mysql-community.repo
/etc/yum.repos.d/mysql-community-source.repo
```

会获得两个mysql的yum repo源：/etc/yum.repos.d/mysql-community.repo，/etc/yum.repos.d/mysql-community-source.repo。

### 第五步：安装mysql

```
[root@master ~]# yum install mysql-server ||  mysql
```

如果发现 Centos7下mysql Can’t connect to local MySQL server through socket的解决方法

```
 /etc/my.cnf 置空  然后 mysql 直接登录
```


## 支持用户允许远程连接数据库
```
grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
flush privileges;
```

## 新建用户远程连接数据库

```
grant all on *.* to admin@'%' identified by '123456' with grant option; 
flush privileges;
```