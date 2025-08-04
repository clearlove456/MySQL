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

# 运算符
# 1.算术运算符 + - * / %

SELECT 100 + '1'
FROM DUAL;

SELECT 100 + 'a'
FROM DUAL;

SELECT 100 + null
FROM DUAL;


/* 	
		2.比较运算符 
				2.1 = <=> <>(!=) < <= > >=
						一边数字，一边字母时 字母会隐式转换
						两边都是字母时不会
				2.2 IS NULL, IS NOT NULL, LEAST, GREATEST, BETWEEN AND, ISNULL, IN, NOT IN, LIKE, REGEXP, RLIKE  
								 

*/

SELECT *
FROM employees
WHERE commission_pct = NULL; #null参与运算结果为null 0数据

SELECT *
FROM employees
WHERE commission_pct <=> NULL;

SELECT NULL <=> NULL # 可比较NULL
FROM DUAL;

SELECT LEAST(first_name,last_name), GREATEST(first_name,last_name)
FROM employees;

SELECT employee_id, last_name, salary
FROM employees
WHERE salary BETWEEN 6000 AND 8000; # 闭区间

#查询部门为10 20 30的员工信息
SELECT employee_id, last_name, department_id
FROM employees
WHERE department_id IN (10, 20, 30);

#查询工资不是6000 7000 8000的员工信息
SELECT employee_id, last_name, department_id, salary
FROM employees
WHERE salary NOT IN (6000, 7000, 8000);

#LIKE模糊查询

/*
重名时需用转义字符\
		% 不确定个数字符 0个或多个
		— 一个不确定的字符
*/
# 查询last_name中含a的员工信息
SELECT *
FROM employees
WHERE last_name LIKE '%a%';

# 查询last_name中含a的员工信息且包含e
SELECT *
FROM employees
WHERE last_name LIKE '%a%' AND last_name LIKE '%e%';

# 查询last_name中第二个字符是a的员工信息
SELECT *
FROM employees
WHERE last_name LIKE '_a%';

# 逻辑运算符
/*
		AND OR XOR
		&& ||   ^   ~  << >> &
*/

# 1.选择工资不在5000到12000的员工的姓名和工资
SELECT last_name, salary
FROM employees
WHERE salary NOT BETWEEN 5000 AND 12000;

# 2.选择在20或50号部门工作的员工姓名和部门号
SELECT last_name, department_id
FROM employees
WHERE department_id IN (20, 50);

# 3.选择公司中没有管理者的员工姓名及job_id
SELECT last_name, job_id
FROM employees
WHERE manager_id IS NULL;

# 4.选择公司中有奖金的员工姓名，工资和奖金级别
SELECT last_name, salary, commission_pct
FROM employees
WHERE commission_pct IS NOT NULL;

# 5.选择员工姓名的第三个字母是a的员工姓名
SELECT last_name
FROM employees
WHERE last_name LIKE "__a%";

# 6.选择姓名中有字母a和k的员工姓名
SELECT last_name
FROM employees
WHERE last_name LIKE "%a%" AND last_name LIKE "%k%"

# 7.显示出表 employees 表中 first_name 以 'e'结尾的员工信息
SELECT *
FROM employees
WHERE first_name LIKE "%e";

# 8.显示出表 employees 部门编号在 80-100 之间的姓名、工种
SELECT last_name, job_id
FROM employees
WHERE department_id BETWEEN 80 AND 100;

# 9.显示出表 employees 的 manager_id 是 100,101,110 的员工姓名、工资、管理者id
SELECT last_name, salary, manager_id
FROM employees
WHERE manager_id IN (100, 101, 110);



#排序与分页
/*
		1.列的别名只能在ORDER BY用 不能再WHERE用
		
*/
SELECT *
FROM employees
ORDER BY department_id DESC;


# 二级排序
SELECT department_id, salary
FROM employees
ORDER BY department_id, salary; 


# 分页 mysql使用limit实现数据的分页

# 每页显示20条记录
SELECT *
FROM employees
LIMIT 0, 20;      #偏移量 数量


# 每页显示20条记录 显示第二页
SELECT *
FROM employees
LIMIT 20, 20;      #偏移量 数量

# 每页显示20条记录 显示第三页
SELECT *
FROM employees
LIMIT 40, 20;      #偏移量 数量

# 每页显示pagesize条数据，此时显示第page页
SELECT *
FROM employees
LIMIT (page - 1) * pagesize, pagesize;


SELECT *
FROM employees
WHERE salary > 6000
LIMIT 0, 20;

# 8.0新特性
SELECT *
FROM employees
LIMIT 2 OFFSET 31; # 数量 偏移量

#1. 查询员工的姓名和部门号和年薪，按年薪降序,按姓名升序显示
SELECT last_name, department_id, salary * 12 annual_salary
FROM employees
ORDER BY annual_salary DESC, last_name;

#2. 选择工资不在 8000 到 17000 的员工的姓名和工资，按工资降序，显示第21到40位置的数据
SELECT last_name, salary
FROM employees
WHERE salary NOT BETWEEN 8000 AND 17000
ORDER BY salary DESC
LIMIT 20, 20;

#3. 查询邮箱中包含 e 的员工信息，并先按邮箱的字节数降序，再按部门号升序
SELECT *
FROM employees
WHERE email LIKE "%e%"
ORDER BY LENGTH(email) DESC, department_id;



# 多表查询

DESC locations;

# 查询员工名为'Abel'的人在哪个城市工作
SELECT *
FROM employees
WHERE last_name = 'Abel';

/*
		多表的查询如何实现
*/

SELECT employee_id, department_name
FROM employees, departments
WHERE employees.department_id = departments.department_id; #加入连接条件

SELECT employee_id, department_name, employees.department_id #相同的字段得确定是哪一张表
FROM employees, departments
WHERE employees.department_id = departments.department_id; #加入连接条件

#可以给表起别名， 在select 与 where 中使用表的别名
SELECT employee_id, department_name
FROM employees t1, departments t2 #一旦使用别名就不能再使用本名了
WHERE t1.department_id = t2.department_id; #加入连接条件


# n张表至少有n-1个连接条件
SELECT employee_id, last_name, department_name, city
FROM employees t1, departments t2, locations t3
WHERE t1.department_id = t2.department_id && t2.location_id = t3.location_id;


/*
	多表查询的分类
			1.等值连接 vs 非等值连接
			2.自连接 vs 非自连接
			3.内连接 vs 外连接
*/

# 非等值连接
SELECT *
FROM job_grades;

SELECT last_name, salary, grade_level
FROM employees t1, job_grades t2
WHERE t1.salary BETWEEN t2.lowest_sal AND t2.highest_sal;


#自连接 查询employees表中的每一个员工对应的经理id
SELECT t1.employee_id, t1.last_name, t2.employee_id, t2.last_name
FROM employees t1, employees t2
WHERE t1.manager_id = t2.employee_id;

#内连接 外连接
/*
	什么是内连接 上述所写都是内连接 合并具有同一列的两个以上的行，结果集不包含一个表与另一个表不匹配的行
	
*/  
 
 
#外连接
 
#查询所有员工的last_name, department_name 有所有基本是外连接 因为含有不匹配数据
SELECT last_name, department_name
FROM employees t1, departments t2
WHERE t1.department_id = t2.department_id; 
 
#sql92语法实现外连接 ----MySQL不支持sql92的左外连接语法了
SELECT last_name, department_name
FROM employees t1, departments t2
WHERE t1.department_id = t2.department_id(+); 
 
# ------------------------------------以上都是sql92语法
 
 
#sql99语法实现多表查询
 
#内连接
SELECT last_name, department_name, city
FROM employees t1 inner JOIN departments t2
ON t1.department_id = t2.department_id
JOIN locations t3
ON t2.location_id = t3.location_id; 

#左外连接
SELECT last_name, department_name
FROM employees t1 LEFT OUTER JOIN departments t2
ON t1.department_id = t2.department_id;

#右外连接
SELECT last_name, department_name
FROM employees t1 RIGHT OUTER JOIN departments t2
ON t1.department_id = t2.department_id;

#UNION关键字的使用
/*
		使用UNION时SELECT的得一样
		UNION 两者集合去重        可以UNION来满外连接
		UION ALL 两者集合不去重
*/

#满外连接 mysql不支持FULL OUTER JOIN语法
SELECT last_name, department_name
FROM employees t1 FULL OUTER JOIN departments t2
ON t1.department_id = t2.department_id;


#7种join实现

#内连接
SELECT employee_id, department_name
FROM employees t1 JOIN departments t2
ON  t1.department_id = t2.department_id;

#左外连接
SELECT employee_id, department_name
FROM employees t1 LEFT OUTER JOIN departments t2
ON  t1.department_id = t2.department_id;

#右外连接
SELECT employee_id, department_name
FROM employees t1 RIGHT OUTER JOIN departments t2
ON  t1.department_id = t2.department_id;

# 左外去内
SELECT employee_id, department_name
FROM employees t1 LEFT OUTER JOIN departments t2
ON  t1.department_id = t2.department_id
WHERE t2.department_id IS NULL;

#右外去内
SELECT employee_id, department_name
FROM employees t1 RIGHT OUTER JOIN departments t2
ON  t1.department_id = t2.department_id
WHERE t1.department_id IS NULL;

#满外连接
# 1 左外连接 UNION 右外去内
SELECT employee_id, department_name
FROM employees t1 LEFT OUTER JOIN departments t2
ON  t1.department_id = t2.department_id
UNION
SELECT employee_id, department_name
FROM employees t1 RIGHT OUTER JOIN departments t2
ON  t1.department_id = t2.department_id
WHERE t1.department_id IS NULL;

# 2 右外连接 UNION 左外去内 

# 3 内连接 UNION 右外去内 UNION 左外去内
SELECT employee_id, department_name
FROM employees t1 JOIN departments t2
ON  t1.department_id = t2.department_id
UNION
SELECT employee_id, department_name
FROM employees t1 LEFT OUTER JOIN departments t2
ON  t1.department_id = t2.department_id
WHERE t2.department_id IS NULL
UNION
SELECT employee_id, department_name
FROM employees t1 RIGHT OUTER JOIN departments t2
ON  t1.department_id = t2.department_id
WHERE t1.department_id IS NULL;



```



