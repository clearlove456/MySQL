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



## SELECT基本语法

select

select * from 表名；

```sql
select *
from employees;

SELECT employee_id, last_name, salary
FROM employees
ORDER BY salary;

# 6.列的别名
# as:全称alias(别名)，可以省略
# 列的别名可以使用双引号
SELECT employee_id emp_id, last_name AS lname, salary "money"
FROM employees
ORDER BY salary;

# 7.去除重复行
# 查询员工表中一共有多少的部门id
SELECT DISTINCT department_id
FROM employees;

#错误的情况
SELECT salary, DISTINCT department_id
FROM employees;

SELECT DISTINCT department_id, salary
FROM employees;

# 8.空值参与运算
# 空值：NULL
# NULL不等同于 0, '', 'null'
# 空值参与运算
SELECT employee_id, salary "月工资", salary * (1 + commission_pct) * 12 "年工资", commission_pct
FROM employees;

SELECT employee_id, salary "月工资", salary * (1 + IFNULL(commission_pct, 0)) * 12 "年工资", commission_pct
FROM employees;

# 9.着重号`  出现的字段与系统有冲突重合时使用着重号修饰
SELECT *
FROM `order`; 

# 10.查询常数 会给常数列的每一行都使用相同的值(常数)
SELECT '尚硅谷', employee_id, last_name
FROM employees;

# 11.显示表结构 显示了表中的字段的详细信息
DESCRIBE employees;

/*
		过滤记录
		SELECT 字段1， 字段2
		FROM 表名
		WHERE 条件;
*/

# 12.过滤数据
SELECT *
FROM employees
WHERE department_id = 90;

# 查询last_name为"King"的员工信息
SELECT *
FROM employees
WHERE last_name = "King"

/*
		1.查询员工12个月的工资总合，并其别名为ANNUAL SALARY
		2.查询employees表中去除重复的job_id以后的数据
		3.查询工资大于12000的员工的姓名和工资
		4.查询员工号为176的员工的姓名和部门号
		5.显示表department的结构，并查询其中的全部数据
*/

SELECT employee_id, last_name, salary * 12 "ANNUAL SALARY"
FROM employees;

SELECT DISTINCT job_id
FROM employees;

SELECT last_name, first_name, salary
FROM employees;
WHERE salary > 12000;

SELECT last_name, department_id
FROM employees
WHERE employee_id = 176;

DESCRIBE departments;
SELECT *
FROM departments;
```



