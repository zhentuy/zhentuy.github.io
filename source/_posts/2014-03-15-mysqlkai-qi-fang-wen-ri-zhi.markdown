---
layout: post
title: "mysql开启访问日志"
date: 2014-03-15 06:31:21 -0700
comments: true
categories: mysql
---

想看django ORM 访问数据的时候执行了什么sql，以为打开访问日志很简单。就是个开关吗。没想到挺折腾。

mysql版本是
    Server version: 5.1.69-log Source distribution
首选配置 /etc/my.cnf
    general_log=1
    general_log_file=/var/log/mysql/query.log
    log-output=FILE
general_log也可以写成log，但是这样在mysql的error.log 里面会有一warning  说这个deprecated。
这error log  就是默认开启的mysqld.log，是不靠谱的，我配置完general.log，其实这个文件并不存在的时候
它就不报错了！！等我配置好，重启mysqld，用python做一个查询，
满心欢喜的去看query.log 的时候毛都没有。 文件没创建。不像mysqld.log是mysql的自己创建的。
需要自己建立这个日志文件（在my.cnf里面配置的)，并把所有者改成mysql.
    
    touch /var/log/mysql.general.log
    chown mysql.mysql /var/log/mysql.general.log

再次查询，终于看到mysql查询日志了~~
