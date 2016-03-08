---
title: Mac OS X 系统升级Mysql数据库
title-en: Mac OS X update MysqlServer
date: 2016-02-17 11:45:25 +0800
categories: Mysql
tags: mac mysql
layout: post
---

### Mac OS X 系统升级Mysql数据库
> 本片博文主要介绍如何对MacOS上的Mysql数据库进行升级

1. 首先停止Mysql服务 `sudo /usr/local/mysql/spport-files/mysql.server stop`
2. 然后下载你需要的Mysql安装包
3. 安装好后你的文件会存在 `/usr/local/mysql-5.7.2-osx10.10-x86_64`
	并且mysql的链接也会指向同样的位置 `/usr/local/mysql`
	而之前的数据库文件应该在同样的目录中 `/usr/local/mysql-5.3.2-osx`
4. 然后将老的数据库目录下的数据文件复制过去
	` sudo cp -rf /usr/local/mysql-5.3.2-osx/data  /usr/local/mysql-5.7.2-osx10.10-x86_64/data`
5. 设置正确的权限 `sudo chown -R _mysql /usr/local/mysql-5.7.2-osx10.10-x86_64/data `
6. 启动Mysql 然后修复数据库 `sudo /usr/local/mysql/spport-files/mysql.server start`
7. 运行升级程序 `sudo /usr/local/mysql/bin/mysql_upgrade`
8. 如果出错，就在运行一次，随后重启Mysql服务 `sudo /usr/local/mysql/spport-files/mysql.server restart `
9. 查看Mysql版本 `/usr/local/mysql/bin/mysql`
10. 重新设定root密码 `/usr/local/mysql/bin/mysqladmin -u root password 'yourpassword'`





	 
