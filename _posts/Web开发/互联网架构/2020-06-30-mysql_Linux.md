---
layout: post
title : 「Web开发-互联网架构」 mysql_Linux
date: 2020-06-30
tags: [Web开发, 互联网架构]
categories: [Web开发-互联网架构]
---

# mysql

[MySql数据库安装](https://www.jianshu.com/p/6ecff9fecddd)

## 安装

``` bash
# 下载 rpm 文件 
wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
# 安装 rpm文件
yum -y localinstall mysql80-community-release-el7-3.noarch.rpm
# 安装mysql
yum -y install mysql-community-server
```

## 启动

``` BASH
# 启动mysql
systemctl start mysqld
# 端口查看
netstat -tnal|grep 3306
# tcp6       0      0 :::3306        :::*     LISTEN 
# tcp6       0      0 :::33060       :::*     LISTEN 

# 设置开机启动
systemctl enable mysqld
systemctl daemon-reload
# 重启命令
systemctl restart mysqld
```

## 用户设置

``` sql
grep "A temporary password is generated for root@localhost" /var/log/mysqld.log
2020-01-22T08:59:00.825035Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: 0jTSLvaCbc, A

# 登录mysql,这里要输入的密码就是前文提到的 `0jTSLvaCbc` ,不同的机器此密码不同,灵活观察
mysql -uroot -p

-- 以下操作均在mysql命令行下执行
-- 修改root密码 随机密码生成地址:https://suijimimashengcheng.51240.com/
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Root233!@#'; 

-- 创建可远程登录用户
CREATE USER 'mysql'@'%' IDENTIFIED BY 'Mysql233!@#'; 

-- 用户授权
GRANT ALL ON *.* TO 'mysql'@'%'; 

-- 下面修改密码策略, 否则会登录失败
use mysql; 
ALTER USER 'mysql'@'%' IDENTIFIED BY 'Mysql233!@#' PASSWORD EXPIRE NEVER; 
ALTER USER 'mysql'@'%' IDENTIFIED WITH mysql_native_password BY 'Mysql233!@#'; 
FLUSH PRIVILEGES; 
```
