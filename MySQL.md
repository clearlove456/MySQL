# MySQL

mysql默认的端口号时3306

5.7插入中文时会报错 而8.0版本可以



## 1连接数据库

mysql -uroot -p  连接数据库

mysql -uroot -p -hlocalhost -P13306 //选择版本号连接  要选择连接远程数据库时将-h后的换成IP地址即可

quit 退出

net stop mysql80 停止mysql的进程服务

net start mysql80 开启mysql的进程服务



show databases; 显示所有数据库

create database + 名字; 创建数据库

use + 数据库名; 选择数据库使用

create table + 表名（变量）； 创建表

show tables; 显示当前数据库下的所有表

show variables like 'character_%'; 显示所有字符集