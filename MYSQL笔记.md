# 一、数据库技术概念

DB：数据库，存储数据的容器

DBNS：数据库管理系统，又称为数据库软件或数据库产品，用于创建或管理DB

SQL：结构化查询语言，用于数据库通信的语言，不是某个数据库软件特有的，而是几乎所有的主流数据库软件通用的语言。

MYSQL属于C/S架构的软件

开启mysql：

（1）net start 服务名

（2）计算机-管理-服务

登录：mysql -h主机名 -p端口号 -u用户名  -p密码

# 二、DQL语言

## 一.常用命令

（1）DESC ：显示表结构

 

（2）distinc：去重 

例如：

```sql
select distinct department_id from employees;
```

 

（3）+号：仅只有作为运算符的功能

​	1.两个操作数都为数值型，则做加法运算

​	2.其中一方为字符型，试图将字符型数值转成数值型

​		① 如果转换成功，则继续做加法运算

​		②如果转换失败，则将字符型数值转换为0

​	3.只要其中一方为null，则结果肯定为null

 

 

（4）IFNULL：判断字段是否为空，若是赋值，否则返回原来的值

例如：

```sql
SELECT IFNULL(commission_pct,0)  FROM employees
```

 

（5）ESCAPE：标明转义字符

例如：

```sql
SELECT  lastname  FROM employees WHERE  last_name LIKE  ‘_$_%’ ESCAPE  ‘$’;
```

 

（6）IS NULL：判断是否为空（可能会触发权权标扫描，加大数据库查询负担）

例如：

```sql
WHERE  salary  IS NULL

WHERE  salary  IS NOT NULL
```

 

（7）IN：相当于判断条件多个OR

例如：

```sql
salary = “×××” OR salary=”××” OR salary=”×“ 相当于 salary IN(×××,××,×)
```

 

（8）<=>：安全等于（即可以判断NULL值，又可以判断普通的数值，可读性较低建议用IS NULL）

 

（9）LIKE：模糊查询

例如：1.位数查询：’__a’

​			2. 最后一位检索：’%a’

​			3. 普通模糊查询：’%a%’

 

（10）<>：不等于（相当于!=）



（11）ORDER BY：排序(放最后)

​	1.降序ORDER BY ××× DESC;

​	2.升序ORDERBY ××× ASC;

 

**面试题：试问select * from employees；和 select * from employees where commission_pct like ‘%%’ and last_name like ‘%%’;结果是否一样?并说明原因**

 

不一样！

 

如果判断的字段有null值，则第一种写法输出含null的第二种不包含null的，若and改为or，lastname中不包含有null所以会输出commission_pct包含null的

## 二.常用函数

### 1.单行函数

#### 字符函数

注：中文占3个字节，字母占一个字节

 

（1）LENGTH（）：函数判断字段的长度并输出

（2）Concat：拼接

例如：

```sql
select concat(last_name,first_name)  AS 姓名
```

（3）upper（小写变大写）、lower（大写变小写）

例如：

```sql
SELECT UPPER(‘john’);

SELECT LOWER(‘john’);
```

（4）substr、substring：索引

 例如：

```sql
SELECT SUBSTR(‘李莫愁爱上了陆展元’,7) （输出结果为陆展元）//截取从指定索引处后面所有字符
```

```sql
SELECT SUBSTR(‘李莫愁爱上了陆展元’,1,3)（输出结果为李莫愁）//截取从指定索引处指定字符长度的字符
```

（5）INSTR：返回子串第一次出现的索引，如果找不到返回0

例如：

```sql
SELECT INSTR(‘杨不因留下爱上了因留下’,’因留下’) //输出结果为3 
```

（6）trim：去除前后某类字（默认为空格）

例如：

```sql
SELECT LENGTH(TRIM(‘  张翠山  ’)) //输出为张翠山但字段长度仍为9
```

```sql
 SELECT TRIM(‘aa’ FROM ‘aaaaaaaaaaaaa张aaaaa翠山aaaaaaaaa‘)//输出字段a为张aaaaa翠山a
```

（7）lpad：用指定的字符实现左填充指定长度

例如：

```sql
SELECT LPAD(‘殷素素’,10,’2’) //输出结果为2222222殷素素
```

```sql
 SELECT LPAD(‘殷素素’,2,’2’)//输出结果为殷素
```

（8）rpad：用指定的字符实现右填充指定长度

（9）Replace：替换

例如：

```
SELECT REPLACE(‘周芷若张无忌爱上了周芷若’,’周芷若’,’赵敏’)//结果为赵敏张无忌爱上了赵敏
```

 

#### 数学函数

（1）round：四舍五入

例如：

```sql
SELECT ROUND(1.65);

SELECT ROUND(1.567,2);//小数点后保留2位结果为1.57
```

（2）ceil：向上取整

例如：

```sql
SELECT CELL(1.02)//结果为2
```

（3）floor：向下取整

（4）Truncate：截断

例如：

```sql
SELECT TRUNCATE(1.69999,1)//结果为1.6
```

（5）mod：取余（相当于%）

 

#### 日期函数

（1）now：返回当前系统日期+时间

例如：

```sql
SELECT NOW();（2020-06-11 17.27.01）
```

（2）curdate：返回当前系统日期，不包含时间

例如：

```sql
SELECT CURDATE();（11:13:35）
```

（3）可以获取指定的部分，年、月、日、小时、分钟、秒

YEAR(),MONTH()...

MONTHNAME（月份显示为英文）

（4）str_todate：将日期格式的字符转换成指定格式的日期

![img](file:///C:\Users\DEAFTS~1\AppData\Local\Temp\ksohtml11924\wps1.jpg) 

例如：

```sql
SELECT STR_TO_DATE(‘1998-3-2’,’%Y-%C-%D’)//1998-03-02
```

（5）date_format：将日期转换成字符

```sql
DATE_FORMAT(‘2018/6/6’,’%Y年%m月%d日’)//2018年6月6日
```

 （6）datediff:计算日期之差

```sql
select datediff(max(hiredate),min(hiredate));
```



#### 其他函数

VERSION()：查看当前数据库版本

DATABASE()：查看当前数据库

USER()：查看当前用户

 

#### 流程控制函数

（1）if函数：if else 的效果

```sql
SELECT IF(10<5,'大','小');//大

SELECT last_name,commission_pct,IF(commission_pct IS NULL,'没奖金，呵呵','有奖金，嘻嘻') 备注
FROM employees;
```

（2）case函数

使用一： switch case 的效果

```sql
case 要判断的字段或表达式
when 常量1 then 要显示的值1或语句1;
when 常量2 then 要显示的值2或语句2;
...
else 要显示的值n或语句n;
end

```

案例：查询员工的工资，要求

部门号=30，显示的工资为1.1倍
部门号=40，显示的工资为1.2倍
部门号=50，显示的工资为1.3倍
其他部门，显示的工资为原工资

```sql
SELECT salary 原始工资,department_id,
CASE department_id
WHEN 30 THEN salary*1.1
WHEN 40 THEN salary*1.2
WHEN 50 THEN salary*1.3
ELSE salary
END AS 新工资
FROM employees;
```

使用二：类似于 多重if

```sql
case 
when 条件1 then 要显示的值1或语句1
when 条件2 then 要显示的值2或语句2
。。。
else 要显示的值n或语句n
end
```

案例：查询员工的工资的情况
如果工资>20000,显示A级别
如果工资>15000,显示B级别
如果工资>10000，显示C级别
否则，显示D级别

```sql
SELECT salary
CASE 
WHEN salary>20000 THEN 'A'
WHEN salary>15000 THEN 'B'
WHEN salary>10000 THEN 'C'
ELSE 'D'
END AS 工资级别
FROM employees;
```

### 2.分组函数

功能：用作统计使用，又称为聚合函数或统计函数或组函数

分类：
sum 求和、avg 平均值、max 最大值 、min 最小值 、count 计算个数

特点：
1、sum、avg一般用于处理数值型
   max、min、count可以处理任何类型
2、以上分组函数都忽略null值

3、可以和distinct搭配实现去重的运算

4、count函数的单独介绍
一般使用count(*)用作统计行数

5、和分组函数一同查询的字段要求是group by后的字段

#### 简单的使用

```sql
SELECT SUM(salary) FROM employees;
SELECT AVG(salary) FROM employees;
SELECT MIN(salary) FROM employees;
SELECT MAX(salary) FROM employees;
SELECT COUNT(salary) FROM employees;


SELECT SUM(salary) 和,AVG(salary) 平均,MAX(salary) 最高,MIN(salary) 最低,COUNT(salary) 个数
FROM employees;

SELECT SUM(salary) 和,ROUND(AVG(salary),2) 平均,MAX(salary) 最高,MIN(salary) 最低,COUNT(salary) 个数
FROM employees;
```

####参数支持哪些类型

```sql
SELECT SUM(last_name) ,AVG(last_name) FROM employees;
SELECT SUM(hiredate) ,AVG(hiredate) FROM employees;

SELECT MAX(last_name),MIN(last_name) FROM employees;

SELECT MAX(hiredate),MIN(hiredate) FROM employees;

SELECT COUNT(commission_pct) FROM employees;
SELECT COUNT(last_name) FROM employees;
```



####是否忽略null

```sql
SELECT SUM(commission_pct) ,AVG(commission_pct),SUM(commission_pct)/35,SUM(commission_pct)/107 FROM employees;

SELECT MAX(commission_pct) ,MIN(commission_pct) FROM employees;

SELECT COUNT(commission_pct) FROM employees;
SELECT commission_pct FROM employees;
```




####和distinct搭配

```sql
SELECT SUM(DISTINCT salary),SUM(salary) FROM employees;

SELECT COUNT(DISTINCT salary),COUNT(salary) FROM employees;
```



####count函数的详细介绍

```sql
SELECT COUNT(salary) FROM employees;


SELECT COUNT(*) FROM employees;//防止每个列都有null值导致未统计

SELECT COUNT(1) FROM employees;//count(1)，其实就是计bai算一共有多少du符合条件的行。

1并不是表示第zhi一个字段，而是表示一个固dao定值。其实就可以想成表中有这么一个字段，这个字段就是固定值1，count(1)，就是计算一共有多少个1。

同理，count(2)，也可以，得到的值完全一样，count('x')，count('y')都是可以的。一样的理解方式。在你这个语句理都可以使用，返回的值完全是一样的。就是计数。

count(*)，执行时会把星号翻译成字段的具体名字，效果也是一样的，不过多了一个翻译的动作，比固定值的方式效率稍微低一些。

其实就是在表前面加了一个都为1的常数列
```

效率：
MYISAM存储引擎下  ，COUNT(*)的效率高
INNODB存储引擎下，COUNT(*)和COUNT(1)的效率差不多，比COUNT(字段)要高一些


####和分组函数一同查询的字段有限制

```sql
SELECT AVG(salary),employee_id  FROM employees;
```

## 三.分组查询

语法：

​	select 查询列表
​	from 表
​	【where 筛选条件】
​	group by 分组的字段
​	【order by 排序的字段】;

特点：
1、和分组函数一同查询的字段必须是group by后出现的字段
2、筛选分为两类：分组前筛选和分组后筛选

|            | 针对的表           | 位置       | 连接的关键字 |
| ---------- | ------------------ | ---------- | ------------ |
| 分组前筛选 | 原始表             | group by前 | where        |
| 分组后筛选 | group by后的结果集 | group by后 | having       |

**问题1：分组函数做筛选能不能放在where后面**
答：不能

**问题2：where——group by——having**

答：一般来讲，能用分组前筛选的，尽量使用分组前筛选，提高效率

3、分组可以按单个字段也可以按多个字段
4、可以搭配着排序使用

引入：查询每个部门的员工个数

```sql
SELECT COUNT(*) FROM employees WHERE department_id=90;
```



### 1.简单的分组

案例1：查询每个工种的员工平均工资

```sql
SELECT AVG(salary),job_id
FROM employees
GROUP BY job_id;
```

案例2：查询每个位置的部门个数

```sql
SELECT COUNT(*),location_id
FROM departments
GROUP BY location_id;
```




###2.可以实现分组前的筛选

案例1：查询邮箱中包含a字符的 每个部门的最高工资

```sql
SELECT MAX(salary),department_id
FROM employees
WHERE email LIKE '%a%'
GROUP BY department_id;
```

案例2：查询有奖金的每个领导手下员工的平均工资

```sql
SELECT AVG(salary),manager_id
FROM employees
WHERE commission_pct IS NOT NULL
GROUP BY manager_id;
```



###3.分组后筛选

案例：查询哪个部门的员工个数>5

①查询每个部门的员工个数

```sql
SELECT COUNT(*),department_id
FROM employees
GROUP BY department_id;
```

② 筛选刚才①结果

```sql
SELECT COUNT(*),department_id
FROM employees

GROUP BY department_id

HAVING COUNT(*)>5;
```

案例2：每个工种有奖金的员工的最高工资>12000的工种编号和最高工资

```sql
SELECT job_id,MAX(salary)
FROM employees
WHERE commission_pct IS NOT NULL
GROUP BY job_id
HAVING MAX(salary)>12000;
```

案例3：领导编号>102的每个领导手下的最低工资大于5000的领导编号和最低工资

```sql
manager_id>102

SELECT manager_id,MIN(salary)
FROM employees
GROUP BY manager_id
HAVING MIN(salary)>5000;
```




###4.添加排序

案例：每个工种有奖金的员工的最高工资>6000的工种编号和最高工资,按最高工资升序

```sql
SELECT job_id,MAX(salary) m
FROM employees
WHERE commission_pct IS NOT NULL
GROUP BY job_id
HAVING m>6000
ORDER BY m ;
```




###5.按多个字段分组

案例：查询每个工种每个部门的最低工资,并按最低工资降序

```sql
SELECT MIN(salary),job_id,department_id
FROM employees
GROUP BY department_id,job_id
ORDER BY MIN(salary) DESC;
```

## 四.连接查询

含义：又称多表查询，当查询的字段来自于多个表时，就会用到连接查询

笛卡尔乘积现象：表1 有m行，表2有n行，结果=m*n行

发生原因：没有有效的连接条件
如何避免：添加有效的连接条件

分类：

​	1.按年代分类：
​		（1）sql92标准:仅仅支持内连接
​		（2）sql99标准【推荐】：支持内连接+外连接（左外和右外）+交叉连接

​	2.按功能分类：
​		（1）内连接：
​				①等值连接
​				②非等值连接
​				③自连接
​		（2）外连接：
​				①左外连接
​				②右外连接
​				③全外连接
​		（3）交叉连接

```sql
SELECT * FROM beauty;

SELECT * FROM boys;

SELECT NAME,boyName FROM boys,beauty
WHERE beauty.boyfriend_id= boys.id;
```



### 1.sql92标准

#### (一)等值连接

① 多表等值连接的结果为多表的交集部分
②n表连接，至少需要n-1个连接条件
③ 多表的顺序没有要求
④一般需要为表起别名
⑤可以搭配前面介绍的所有子句使用，比如排序、分组、筛选

案例1：查询女神名和对应的男神名

```sql
SELECT NAME,boyName 
FROM boys,beauty
WHERE beauty.boyfriend_id= boys.id;
```

案例2：查询员工名和对应的部门名

```sql
SELECT last_name,department_name
FROM employees,departments
WHERE employees.`department_id`=departments.`department_id`;
```



#####1)为表起别名

①提高语句的简洁度
②区分多个重名的字段

注意：如果为表起了别名，则查询的字段就不能使用原来的表名去限定

查询员工名、工种号、工种名

```sql
SELECT e.last_name,e.job_id,j.job_title
FROM employees  e,jobs j
WHERE e.`job_id`=j.`job_id`;
```



##### 2)两个表的顺序是否可以调换

查询员工名、工种号、工种名

```sql
SELECT e.last_name,e.job_id,j.job_title
FROM jobs j,employees e//先调用from后面的所以select后面要写e.,j.
WHERE e.`job_id`=j.`job_id`;
```




#####3)可以加筛选

案例：查询有奖金的员工名、部门名

```sql
SELECT last_name,department_name,commission_pct

FROM employees e,departments d
WHERE e.`department_id`=d.`department_id`
AND e.`commission_pct` IS NOT NULL;
```



案例2：查询城市名中第二个字符为o的部门名和城市名

```sql
SELECT department_name,city
FROM departments d,locations l
WHERE d.`location_id` = l.`location_id`
AND city LIKE '_o%';
```



##### 4)可以加分组

案例1：查询每个城市的部门个数

```sql
SELECT COUNT(*) 个数,city
FROM departments d,locations l
WHERE d.`location_id`=l.`location_id`
GROUP BY city;
```



案例2：查询有奖金的每个部门的部门名和部门的领导编号和该部门的最低工资

```sql
SELECT department_name,d.`manager_id`,MIN(salary)
FROM departments d,employees e
WHERE d.`department_id`=e.`department_id`
AND commission_pct IS NOT NULL
GROUP BY department_name,d.`manager_id`;
```


#####5)可以加排序

案例：查询每个工种的工种名和员工的个数，并且按员工个数降序

```sql
SELECT job_title,COUNT(*)
FROM employees e,jobs j
WHERE e.`job_id`=j.`job_id`
GROUP BY job_title
ORDER BY COUNT(*) DESC;
```




#####6)可以实现三表连接？

案例：查询员工名、部门名和所在的城市

```sql
SELECT last_name,department_name,city
FROM employees e,departments d,locations l
WHERE e.`department_id`=d.`department_id`
AND d.`location_id`=l.`location_id`
AND city LIKE 's%'

ORDER BY department_name DESC;
```



#### (二)非等值连接

案例1：查询员工的工资和工资级别

```sql
SELECT salary,grade_level
FROM employees e,job_grades g
WHERE salary BETWEEN g.`lowest_sal` AND g.`highest_sal`
AND g.`grade_level`='A';

/*
select salary,employee_id from employees;
select * from job_grades;
CREATE TABLE job_grades
(grade_level VARCHAR(3),
 lowest_sal  int,
 highest_sal int);

INSERT INTO job_grades
VALUES ('A', 1000, 2999);

INSERT INTO job_grades
VALUES ('B', 3000, 5999);

INSERT INTO job_grades
VALUES('C', 6000, 9999);

INSERT INTO job_grades
VALUES('D', 10000, 14999);

INSERT INTO job_grades
VALUES('E', 15000, 24999);

INSERT INTO job_grades
VALUES('F', 25000, 40000);

*/
```




####(三)自连接



案例：查询 员工名和上级的名称

```sql
SELECT e.employee_id,e.last_name,m.employee_id,m.last_name
FROM employees e,employees m
WHERE e.`manager_id`=m.`employee_id`;
```

###2.sql99标准

语法：
	select 查询列表
	from 表1 别名 【连接类型】
	join 表2 别名 
	on 连接条件
	【where 筛选条件】
	【group by 分组】
	【having 筛选条件】
	【order by 排序列表】
	

分类：
内连接（★）：inner
外连接
	左外(★)：left 【outer】
	右外(★)：right 【outer】
	全外：full【outer】
交叉连接：cross 

####(一)内连接

语法：

​	select 查询列表
​	from 表1 别名
​	inner join 表2 别名
​	on 连接条件;

分类：
	等值
	非等值
	自连接

特点：
	①添加排序、分组、筛选
	②inner可以省略
	③ 筛选条件放在where后面，连接条件放在on后面，提高分离性，便于阅读
	④inner join连接和sql92语法中的等值连接效果是一样的，都是查询多表的交集



##### 1)等值连接

案例1.查询员工名、部门名

```sql
SELECT last_name,department_name
FROM departments d
 JOIN  employees e
ON e.`department_id` = d.`department_id`;
```



案例2.查询名字中包含e的员工名和工种名（添加筛选）

```sql
SELECT last_name,job_title
FROM employees e
INNER JOIN jobs j
ON e.`job_id`=  j.`job_id`
WHERE e.`last_name` LIKE '%e%';
```



案例3.查询部门个数>3的城市名和部门个数，（添加分组+筛选）

①查询每个城市的部门个数

②在①结果上筛选满足条件的

```sql
SELECT city,COUNT(*) 部门个数
FROM departments d
INNER JOIN locations l
ON d.`location_id`=l.`location_id`
GROUP BY city
HAVING COUNT(*)>3;
```



案例4.查询哪个部门的员工个数>3的部门名和员工个数，并按个数降序（添加排序）

①查询每个部门的员工个数

```sql
SELECT COUNT(*),department_name
FROM employees e
INNER JOIN departments d
ON e.`department_id`=d.`department_id`
GROUP BY department_name
```



② 在①结果上筛选员工个数>3的记录，并排序

```sql
SELECT COUNT(*) 个数,department_name
FROM employees e
INNER JOIN departments d
ON e.`department_id`=d.`department_id`
GROUP BY department_name
HAVING COUNT(*)>3
ORDER BY COUNT(*) DESC;
```



案例5.查询员工名、部门名、工种名，并按部门名降序（添加三表连接）

SELECT last_name,department_name,job_title
FROM employees e
INNER JOIN departments d ON e.`department_id`=d.`department_id`
INNER JOIN jobs j ON e.`job_id` = j.`job_id`

ORDER BY department_name DESC;

#####2)非等值连接

查询员工的工资级别

```sql
SELECT salary,grade_level
FROM employees e
 JOIN job_grades g
 ON e.`salary` BETWEEN g.`lowest_sal` AND g.`highest_sal`;
```



查询工资级别的个数>20的个数，并且按工资级别降序

```sql
 SELECT COUNT(*),grade_level
FROM employees e
 JOIN job_grades g
 ON e.`salary` BETWEEN g.`lowest_sal` AND g.`highest_sal`
 GROUP BY grade_level
 HAVING COUNT(*)>20
 ORDER BY grade_level DESC;
```




 #####3)自连接

案例1.查询员工的名字、上级的名字

```sql
 SELECT e.last_name,m.last_name
 FROM employees e
 JOIN employees m
 ON e.`manager_id`= m.`employee_id`;
```



案例2.查询姓名中包含字符k的员工的名字、上级的名字

```sql
SELECT e.last_name,m.last_name
 FROM employees e
 JOIN employees m
 ON e.`manager_id`= m.`employee_id`
 WHERE e.`last_name` LIKE '%k%';
```




 ####(二)外连接

应用场景：用于查询一个表中有，另一个表没有的记录

 特点：
 1、外连接的查询结果为主表中的所有记录
	如果从表中有和它匹配的，则显示匹配的值
	如果从表中没有和它匹配的，则显示null
	外连接查询结果=内连接结果+主表中有而从表没有的记录
 2、左外连接，left join左边的是主表
    右外连接，right join右边的是主表
 3、左外和右外交换两个表的顺序，可以实现同样的效果 
 4、全外连接=内连接的结果+表1中有但表2没有的+表2中有但表1没有的

引入：查询男朋友 不在男神表的的女神名

```sql
 SELECT * FROM beauty;
 SELECT * FROM boys;
```



左外连接

```sql
 SELECT b.*,bo.*
 FROM boys bo
 LEFT OUTER JOIN beauty b
 ON b.`boyfriend_id` = bo.`id`
 WHERE bo.`id` IS NULL;
```



案例1：查询哪个部门没有员工

(1)左外

```sql
 SELECT d.*,e.employee_id
 FROM departments d
 LEFT OUTER JOIN employees e
 ON d.`department_id` = e.`department_id`
 WHERE e.`employee_id` IS NULL;
```

(2)右外

```sql
 SELECT d.*,e.employee_id
 FROM employees e
 RIGHT OUTER JOIN departments d
 ON d.`department_id` = e.`department_id`
 WHERE e.`employee_id` IS NULL;
```

(3)全外

```sql
 USE girls;
 SELECT b.*,bo.*
 FROM beauty b
 FULL OUTER JOIN boys bo
 ON b.`boyfriend_id` = bo.id;
```

(4)交叉连接

```sql
 SELECT b.*,bo.*
 FROM beauty b
 CROSS JOIN boys bo;
```

(5)图例说明

![image-20200617085436505](D:\Typora\images\image-20200617085436505.png)

 ###3.sql92和 sql99pk
功能：sql99支持的较多
可读性：sql99实现连接条件和筛选条件的分离，可读性较高



## 五.子查询

含义：
出现在其他语句中的select语句，称为子查询或内查询
外部的查询语句，称为主查询或外查询

分类：
按子查询出现的位置：
	select后面：
		仅仅支持标量子查询
	from后面：
		支持表子查询
	where或having后面：★
		标量子查询（单行） √
		列子查询  （多行） √
		行子查询
	exists后面（相关子查询）
		表子查询

按结果集的行列数不同：
	标量子查询（结果集只有一行一列）
	列子查询（结果集只有一列多行）
	行子查询（结果集有一行多列）
	表子查询（结果集一般为多行多列）




###1.where或having后面
分类：

（1）标量子查询（单行子查询）

（2）列子查询（多行子查询）

（3）行子查询（多列多行）

特点：
	①子查询放在小括号内
	②子查询一般放在条件的右侧
	③标量子查询，一般搭配着单行操作符使用

> < >= <= = <>

列子查询，一般搭配着多行操作符使用
in、any/some、all

④子查询的执行优先于主查询执行，主查询的条件用到了子查询的结果

####（1）标量子查询★

案例1：谁的工资比 Abel 高?

①查询Abel的工资

```sql
SELECT salary
FROM employees
WHERE last_name = 'Abel'
```



②查询员工的信息，满足 salary>①结果

```sql
SELECT *
FROM employees
WHERE salary>(

	SELECT salary
	FROM employees
	WHERE last_name = 'Abel'

);
```



案例2：返回job_id与141号员工相同，salary比143号员工多的员工 姓名，job_id 和工资

①查询141号员工的job_id

```sql
SELECT job_id
FROM employees
WHERE employee_id = 141
```

②查询143号员工的salary

```sql
SELECT salary
FROM employees
WHERE employee_id = 143
```



③查询员工的姓名，job_id 和工资，要求job_id=①并且salary>②

```sql
SELECT last_name,job_id,salary
FROM employees
WHERE job_id = (
	SELECT job_id
	FROM employees
	WHERE employee_id = 141
) AND salary>(
	SELECT salary
	FROM employees
	WHERE employee_id = 143

);
```



案例3：返回公司工资最少的员工的last_name,job_id和salary

①查询公司的 最低工资

```sql
SELECT MIN(salary)
FROM employees
```

②查询last_name,job_id和salary，要求salary=①

```sql
SELECT last_name,job_id,salary
FROM employees
WHERE salary=(
	SELECT MIN(salary)
	FROM employees
);
```



案例4：查询最低工资大于50号部门最低工资的部门id和其最低工资

①查询50号部门的最低工资

```sql
SELECT  MIN(salary)
FROM employees
WHERE department_id = 50
```

②查询每个部门的最低工资

```sql
SELECT MIN(salary),department_id
FROM employees
GROUP BY department_id
```



③ 在②基础上筛选，满足min(salary)>①

```sqlite
SELECT MIN(salary),department_id
FROM employees
GROUP BY department_id
HAVING MIN(salary)>(
	SELECT  MIN(salary)
	FROM employees
	WHERE department_id = 50


);
```




非法使用标量子查询

```sql
SELECT MIN(salary),department_id
FROM employees
GROUP BY department_id
HAVING MIN(salary)>(
	SELECT  salary
	FROM employees
	WHERE department_id = 250


);
```



####（2）列子查询（多行子查询）★
案例1：返回location_id是1400或1700的部门中的所有员工姓名

①查询location_id是1400或1700的部门编号

```sql
SELECT DISTINCT department_id
FROM departments
WHERE location_id IN(1400,1700)
```



②查询员工姓名，要求部门号是①列表中的某一个

```sql
SELECT last_name
FROM employees
WHERE department_id  <>ALL(
	SELECT DISTINCT department_id
	FROM departments
	WHERE location_id IN(1400,1700)


);
```




案例2：返回其它工种中比job_id为‘IT_PROG’工种任一工资低的员工的员工号、姓名、job_id 以及salary

①查询job_id为‘IT_PROG’部门任一工资

```sql
SELECT DISTINCT salary
FROM employees
WHERE job_id = 'IT_PROG'
```



②查询员工号、姓名、job_id 以及salary，salary<(①)的任意一个

```sql
SELECT last_name,employee_id,job_id,salary
FROM employees
WHERE salary<ANY(
	SELECT DISTINCT salary
	FROM employees
	WHERE job_id = 'IT_PROG'

) AND job_id<>'IT_PROG';
```

或

```sql
SELECT last_name,employee_id,job_id,salary
FROM employees
WHERE salary<(
	SELECT MAX(salary)
	FROM employees
	WHERE job_id = 'IT_PROG'

) AND job_id<>'IT_PROG';
```



案例3：返回其它部门中比job_id为‘IT_PROG’部门所有工资都低的员工   的员工号、姓名、job_id 以及salary

```sql
SELECT last_name,employee_id,job_id,salary
FROM employees
WHERE salary<ALL(
	SELECT DISTINCT salary
	FROM employees
	WHERE job_id = 'IT_PROG'

) AND job_id<>'IT_PROG';
```

或

```sql
SELECT last_name,employee_id,job_id,salary
FROM employees
WHERE salary<(
	SELECT MIN( salary)
	FROM employees
	WHERE job_id = 'IT_PROG'

) AND job_id<>'IT_PROG';
```



#### （3）行子查询（结果集一行多列或多行多列）

案例：查询员工编号最小并且工资最高的员工信息

```sql
SELECT * 
FROM employees
WHERE (employee_id,salary)=(
	SELECT MIN(employee_id),MAX(salary)
	FROM employees
);
```



①查询最小的员工编号

```sql
SELECT MIN(employee_id)
FROM employees
```



②查询最高工资

```sql
SELECT MAX(salary)
FROM employees
```



③查询员工信息

```sql
SELECT *
FROM employees
WHERE employee_id=(
	SELECT MIN(employee_id)
	FROM employees


)AND salary=(
	SELECT MAX(salary)
	FROM employees

);
```




###2.select后面
/*
仅仅支持标量子查询
*/

案例：查询每个部门的员工个数

```sql
SELECT d.*,(

	SELECT COUNT(*)
	FROM employees e
	WHERE e.department_id = d.`department_id`

 ) 个数
 FROM departments d;
```



案例2：查询员工号=102的部门名

```sql
SELECT (
	SELECT department_name,e.department_id
	FROM departments d
	INNER JOIN employees e
	ON d.department_id=e.department_id
	WHERE e.employee_id=102
	
) 部门名;
```



###3.from后面
/*
将子查询结果充当一张表，要求必须起别名
*/

案例：查询每个部门的平均工资的工资等级

①查询每个部门的平均工资

```sql
SELECT AVG(salary),department_id
FROM employees
GROUP BY department_id


SELECT * FROM job_grades;
```




②连接①的结果集和job_grades表，筛选条件平均工资 between lowest_sal and highest_sal

```sql
SELECT  ag_dep.*,g.`grade_level`
FROM (
	SELECT AVG(salary) ag,department_id
	FROM employees
	GROUP BY department_id
) ag_dep
INNER JOIN job_grades g
ON ag_dep.ag BETWEEN lowest_sal AND highest_sal;
```



###4.exists后面（相关子查询）

/*
语法：
exists(完整的查询语句)
结果：
1或0

*/

```sql
SELECT EXISTS(SELECT employee_id FROM employees WHERE salary=300000);
```



案例1：查询有员工的部门名

**in**

```sql
SELECT department_name
FROM departments d
WHERE d.`department_id` IN(
	SELECT department_id
	FROM employees
)
```



**exists**

```sql
SELECT department_name
FROM departments d
WHERE EXISTS(
	SELECT *
	FROM employees e
	WHERE d.`department_id`=e.`department_id`
);
```




案例2：查询没有女朋友的男神信息

**in**

```sql
SELECT bo.*
FROM boys bo
WHERE bo.id NOT IN(
	SELECT boyfriend_id
	FROM beauty
)
```



**exists**

```sql
SELECT bo.*
FROM boys bo
WHERE NOT EXISTS(
	SELECT boyfriend_id
	FROM beauty b
	WHERE bo.`id`=b.`boyfriend_id`

);
```

## 六.分页查询 ★

/*

应用场景：当要显示的数据，一页显示不全，需要分页提交sql请求
语法：
	select 查询列表
	from 表
	【join type join 表2
	on 连接条件
	where 筛选条件
	group by 分组字段
	having 分组后的筛选
	order by 排序的字段】
	limit 【offset,】size;
	

offset要显示条目的起始索引（起始索引从0开始）
size 要显示的条目个数

特点：
	①limit语句放在查询语句的最后
	②公式
	要显示的页数 page，每页的条目数size
	
```sql
select 查询列表
from 表
limit (page-1)*size,size;

size=10
page  
1	0
2  	10
3	20
```

*/

案例1：查询前五条员工信息

```sql
SELECT * FROM  employees LIMIT 0,5;
SELECT * FROM  employees LIMIT 5;
```




案例2：查询第11条——第25条

```sql
SELECT * FROM  employees LIMIT 10,15;
```



案例3：有奖金的员工信息，并且工资较高的前10名显示出来

```sql
SELECT 

   * FROM
         employees 
     WHERE commission_pct IS NOT NULL 
     ORDER BY salary DESC 
     LIMIT 10 ;
```

## 七.联合查询

/*
union 联合 合并：将多条查询语句的结果合并成一个结果

语法：
查询语句1
union
查询语句2
union
...

应用场景：
要查询的结果来自于多个表，且多个表没有直接的连接关系，但查询的信息一致时

特点：★
1、要求多条查询语句的查询列数是一致的！
2、要求多条查询语句的查询的每一列的类型和顺序最好一致
3、union关键字默认去重，如果使用union all 可以包含重复项

*/

引入的案例：查询部门编号>90或邮箱包含a的员工信息

```sql
SELECT * FROM employees WHERE email LIKE '%a%' OR department_id>90;;

SELECT * FROM employees  WHERE email LIKE '%a%'
UNION
SELECT * FROM employees  WHERE department_id>90;
```

案例：查询中国用户中男性的信息以及外国用户中年男性的用户信息

```sql
SELECT id,cname FROM t_ca WHERE csex='男'
UNION ALL
SELECT t_id,tname FROM t_ua WHERE tGender='male';
```

# 三、DML语言

/*
数据操作语言：
插入：insert
修改：update
删除：delete

*/

##一.插入语句
### 方式一：经典的插入

语法：

```sql
insert into 表名(列名,...) values(值1,...);
SELECT * FROM beauty;
```



**1.插入的值的类型要与列的类型一致或兼容**

```sql
INSERT INTO beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id)
VALUES(13,'唐艺昕','女','1990-4-23','1898888888',NULL,2);
```



**2.不可以为null的列必须插入值。可以为null的列如何插入值？**

方式一：

```sql
INSERT INTO beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id)
VALUES(13,'唐艺昕','女','1990-4-23','1898888888',NULL,2);
```



方式二：

```sql
INSERT INTO beauty(id,NAME,sex,phone)
VALUES(15,'娜扎','女','1388888888');
```



**3.列的顺序是否可以调换**

```sql
INSERT INTO beauty(NAME,sex,id,phone)
VALUES('蒋欣','女',16,'110');
```

**4.列数和值的个数必须一致**

```sql
INSERT INTO beauty(NAME,sex,id,phone)
VALUES('关晓彤','女',17,'110');
```

**5.可以省略列名，默认所有列，而且列的顺序和表中列的顺序一致**

```sql
INSERT INTO beauty
VALUES(18,'张飞','男',NULL,'119',NULL,NULL);
```



### 方式二：

语法：

```sql
insert into 表名
set 列名=值,列名=值,...


INSERT INTO beauty
SET id=19,NAME='刘涛',phone='999';
```




###两种方式大pk ★

**1、方式一支持插入多行,方式二不支持**

```sql
INSERT INTO beauty
VALUES(23,'唐艺昕1','女','1990-4-23','1898888888',NULL,2)
,(24,'唐艺昕2','女','1990-4-23','1898888888',NULL,2)
,(25,'唐艺昕3','女','1990-4-23','1898888888',NULL,2);
```



**2、方式一支持子查询，方式二不支持**

```sql
INSERT INTO beauty(id,NAME,phone)
SELECT 26,'宋茜','11809866';

INSERT INTO beauty(id,NAME,phone)
SELECT id,boyname,'1234567'
FROM boys WHERE id<3;
```



##二.修改语句

/*

1.修改单表的记录★

语法：

```sql
update 表名
set 列=新值,列=新值,...
where 筛选条件;
```

2.修改多表的记录【补充】

语法：
sql92语法：

```sql
update 表1 别名,表2 别名
set 列=值,...
where 连接条件
and 筛选条件;
```

sql99语法：

```sql
update 表1 别名
inner|left|right join 表2 别名
on 连接条件
set 列=值,...
where 筛选条件;
```


*/

1.修改单表的记录

案例1：修改beauty表中姓唐的女神的电话为13899888899

```sql
UPDATE beauty SET phone = '13899888899'
WHERE NAME LIKE '唐%';
```

案例2：修改boys表中id好为2的名称为张飞，魅力值 10

```sql
UPDATE boys SET boyname='张飞',usercp=10
WHERE id=2;
```



2.修改多表的记录

案例 1：修改张无忌的女朋友的手机号为114

```sql
UPDATE boys bo
INNER JOIN beauty b ON bo.`id`=b.`boyfriend_id`
SET b.`phone`='119',bo.`userCP`=1000
WHERE bo.`boyName`='张无忌';
```



案例2：修改没有男朋友的女神的男朋友编号都为2号

```sql
UPDATE boys bo
RIGHT JOIN beauty b ON bo.`id`=b.`boyfriend_id`
SET b.`boyfriend_id`=2
WHERE bo.`id` IS NULL;

SELECT * FROM boys;
```




##三.删除语句
方式一：delete

语法：

1、单表的删除【★】
delete from 表名 where 筛选条件

2、多表的删除【补充】

sql92语法：
delete 表1的别名,表2的别名
from 表1 别名,表2 别名
where 连接条件
and 筛选条件;

sql99语法：

delete 表1的别名,表2的别名
from 表1 别名
inner|left|right join 表2 别名 on 连接条件
where 筛选条件;



方式二：truncate
语法：truncate table 表名;

*/

### 方式一：delete

1.单表的删除

案例：删除手机号以9结尾的女神信息

```sql
DELETE FROM beauty WHERE phone LIKE '%9';
SELECT * FROM beauty;
```



2.多表的删除

案例：删除张无忌的女朋友的信息

```sql
DELETE b
FROM beauty b
INNER JOIN boys bo ON b.`boyfriend_id` = bo.`id`
WHERE bo.`boyName`='张无忌';
```



案例：删除黄晓明的信息以及他女朋友的信息

```sql
DELETE b,bo
FROM beauty b
INNER JOIN boys bo ON b.`boyfriend_id`=bo.`id`
WHERE bo.`boyName`='黄晓明';
```



###方式二：truncate语句

案例：将魅力值>100的男神信息删除

```sql
TRUNCATE TABLE boys ;
```



###delete pk truncate【面试题★】

/*

1.delete 可以加where 条件，truncate不能加

2.truncate删除，效率高一丢丢
3.假如要删除的表中有自增长列，
如果用delete删除后，再插入数据，自增长列的值从断点开始，
而truncate删除后，再插入数据，自增长列的值从1开始。
4.truncate删除没有返回值，delete删除有返回值

5.truncate删除不能回滚，delete删除可以回滚.

*/

```sql
SELECT * FROM boys;

DELETE FROM boys;
TRUNCATE TABLE boys;
INSERT INTO boys (boyname,usercp)
VALUES('张飞',100),('刘备',100),('关云长',100);
```

# 四、DDL语言

/*

数据定义语言

库和表的管理

一、库的管理
创建、修改、删除
二、表的管理
创建、修改、删除

创建： create
修改： alter
删除： drop

*/

## 一.库的管理

### 1、库的创建

/*
语法：
create database  [if not exists]库名;
*/

案例：创建库Books

```sql
CREATE DATABASE IF NOT EXISTS books ;
```




###2、库的修改

RENAME DATABASE books TO 新库名;

更改库的字符集

```sql
ALTER DATABASE books CHARACTER SET gbk;
```



### 3、库的删除

```sql
DROP DATABASE IF EXISTS books;
```




##二.表的管理
###1.表的创建 ★

/*
语法：
create table 表名(
	列名 列的类型【(长度) 约束】,
	列名 列的类型【(长度) 约束】,
	列名 列的类型【(长度) 约束】,
	...
	列名 列的类型【(长度) 约束】


)

*/

案例：创建表Book

```sql
CREATE TABLE book(
	id INT,#编号
	bName VARCHAR(20),#图书名
	price DOUBLE,#价格
	authorId  INT,#作者编号
	publishDate DATETIME#出版日期
);
```


DESC book;

案例：创建表author

```sql
CREATE TABLE IF NOT EXISTS author(
	id INT,
	au_name VARCHAR(20),
	nation VARCHAR(10)

)
DESC author;
```




###2.表的修改

/*
语法

```sql
alter table 表名 add|drop|modify|change column 列名 【列类型 约束】;
```

*/

①修改列名

```sql
ALTER TABLE book CHANGE COLUMN publishdate pubDate DATETIME;
```



②修改列的类型或约束

```sql
ALTER TABLE book MODIFY COLUMN pubdate TIMESTAMP;
```



③添加新列

ALTER TABLE author ADD COLUMN annual DOUBLE; 

④删除列

```sql
ALTER TABLE book_author DROP COLUMN  annual;
```

⑤修改表名

```sql
ALTER TABLE author RENAME TO book_author;

DESC book;
```




###3.表的删除

```sql
DROP TABLE IF EXISTS book_author;

SHOW TABLES;
```



通用的写法：

DROP DATABASE IF EXISTS 旧库名;
CREATE DATABASE 新库名;


DROP TABLE IF EXISTS 旧表名;
CREATE TABLE  表名();



### 4.表的复制

```sql
INSERT INTO author VALUES
(1,'村上春树','日本'),
(2,'莫言','中国'),
(3,'冯唐','中国'),
(4,'金庸','中国');

SELECT * FROM Author;
SELECT * FROM copy2;
```


####(1)仅仅复制表的结构

```sql
CREATE TABLE copy LIKE author;
```

####(2)复制表的结构+数据
```
CREATE TABLE copy2 
SELECT * FROM author;
```



只复制部分数据

```
CREATE TABLE copy3
SELECT id,au_name
FROM author 
WHERE nation='中国';
```



仅仅复制某些字段

```
CREATE TABLE copy4 
SELECT id,au_name
FROM author
WHERE 0;
```

#五、常见的数据类型
/*
数值型：
	整型
	小数：
		定点数
		浮点数
字符型：
	较短的文本：char、varchar
	较长的文本：text、blob（较长的二进制数据）

日期型：
	


*/

##一.整型
/*
分类：
tinyint、smallint、mediumint、int/integer、bigint
1	 2		3	4		8

特点：
① 如果不设置无符号还是有符号，默认是有符号，如果想设置无符号，需要添加unsigned关键字
② 如果插入的数值超出了整型的范围,会报out of range异常，并且插入临界值
③ 如果不设置长度，会有默认的长度
④长度代表了显示的最大宽度，如果不够会用0在左边填充，但必须搭配zerofill使用！

*/

**如何设置无符号和有符号**

```sql
DROP TABLE IF EXISTS tab_int;
CREATE TABLE tab_int(
	t1 INT(7) int,
	t2 INT(7) int unsigned //无符号

);

DROP TABLE IF EXISTS tab_int;
CREATE TABLE tab_int(
	t1 INT(7) ZEROFILL//零填充
	t2 INT(7) ZEROFILL 

);

DESC tab_int;

INSERT INTO tab_int VALUES(-123456);
INSERT INTO tab_int VALUES(-123456,-123456);
INSERT INTO tab_int VALUES(2147483648,4294967296);

INSERT INTO tab_int VALUES(123,123);


SELECT * FROM tab_int;
```


##二.小数
/*
分类：
1.浮点型
float(M,D)
double(M,D)
2.定点型
dec(M，D)
decimal(M,D)

特点：

①
M：整数部位+小数部位
D：小数部位
如果超过范围，则插入临界值

②
M和D都可以省略
如果是decimal，则M默认为10，D默认为0
如果是float和double，则会根据插入的数值的精度来决定精度

③定点型的精确度较高，如果要求插入数值的精度较高如货币运算等则考虑使用


*/

**测试M和D**

```sql
DROP TABLE tab_float;
CREATE TABLE tab_float(
	f1 FLOAT,
	f2 DOUBLE,
	f3 DECIMAL
);
SELECT * FROM tab_float;
DESC tab_float;

INSERT INTO tab_float VALUES(123.4523,123.4523,123.4523);
INSERT INTO tab_float VALUES(123.456,123.456,123.456);
INSERT INTO tab_float VALUES(123.4,123.4,123.4);
INSERT INTO tab_float VALUES(1523.4,1523.4,1523.4);
```



**原则：**

/*
所选择的类型越简单越好，能保存数值的类型越小越好

*/

##三.字符型
/*
较短的文本：

char
varchar

其他：

binary和varbinary用于保存较短的二进制
enum用于保存枚举
set用于保存集合

较长的文本：
text
blob(较大的二进制)

特点：

|         | 写法       | M的意思                         | 特点           | 空间的耗费 | 效率 |
| ------- | ---------- | ------------------------------- | -------------- | ---------- | ---- |
| char    | char(M)    | 最大的字符数，可以省略，默认为1 | 固定长度的字符 | 比较耗费   | 高   |
| varchar | varchar(M) | 最大的字符数，不可以省略        | 可变长度的字符 | 比较节省   | 低   |

```sql
CREATE TABLE tab_char(
	c1 ENUM('a','b','c')


);


INSERT INTO tab_char VALUES('a');
INSERT INTO tab_char VALUES('b');
INSERT INTO tab_char VALUES('c');
INSERT INTO tab_char VALUES('m');
INSERT INTO tab_char VALUES('A');

SELECT * FROM tab_set;

CREATE TABLE tab_set(

​	s1 SET('a','b','c','d')

);
INSERT INTO tab_set VALUES('a');
INSERT INTO tab_set VALUES('A,B');
INSERT INTO tab_set VALUES('a,c,d');
```




##四.日期型

/*

分类：
date只保存日期
time 只保存时间
year只保存年

datetime保存日期+时间
timestamp保存日期+时间

特点：

|           | 字节 | 范围      | 时区等的影响 |
| --------- | ---- | --------- | ------------ |
| datetime  | 8    | 1000-9999 | 不受         |
| timestamp | 4    | 1970-2038 | 受           |

```sql
CREATE TABLE tab_date(
	t1 DATETIME,
	t2 TIMESTAMP

);

INSERT INTO tab_date VALUES(NOW(),NOW());

SELECT * FROM tab_date;


SHOW VARIABLES LIKE 'time_zone';

SET time_zone='+9:00';
```

# 六、常见约束

/*


含义：一种限制，用于限制表中的数据，为了保证表中的数据的准确和可靠性

分类：六大约束
	NOT NULL：非空，用于保证该字段的值不能为空
	比如姓名、学号等

​	DEFAULT:默认，用于保证该字段有默认值
​	比如性别

​	PRIMARY KEY:主键，用于保证该字段的值具有唯一性，并且非空
​	比如学号、员工编号等

​	UNIQUE:唯一，用于保证该字段的值具有唯一性，可以为空
​	比如座位号

​	CHECK:检查约束【mysql中不支持】
​	比如年龄、性别

​	FOREIGN KEY:外键，用于限制两个表的关系，用于保证该字段的值必须来自于主表的关联列的值

​	

​	在从表添加外键约束，用于引用主表中某列的值
​	比如学生表的专业编号，员工表的部门编号，员工表的工种编号
​	

添加约束的时机：
	1.创建表时
	2.修改表时
	

约束的添加分类：
	列级约束：
		六大约束语法上都支持，但外键约束没有效果
		

表级约束：	
	除了非空、默认，其他的都支持


主键和唯一的大对比：

|      | 保证唯一性 | 是否允许为空 | 一个表中可以有多少个 | 是否允许组合 |
| ---- | ---------- | ------------ | -------------------- | ------------ |
| 主键 | √          | ×            | 至多有1个            | √，但不推荐  |
| 唯一 | √          | √            | 可以有多个           | √，但不推荐  |



外键：
	1、要求在从表设置外键关系
	2、从表的外键列的类型和主表的关联列的类型要求一致或兼容，名称无要求
	3、主表的关联列必须是一个key（一般是主键或唯一）
	4、插入数据时，先插入主表，再插入从表
	删除数据时，先删除从表，再删除主表


*/

CREATE TABLE 表名(
	字段名 字段类型 列级约束,
	字段名 字段类型,
	表级约束

)
CREATE DATABASE students;



**注：①主键外键自动生成索引**

​		**②组合主键：只需其中一个属性不一样就行**

##一.创建表时添加约束

###1.添加列级约束
/*
语法：

直接在字段名和类型后面追加 约束类型即可。

只支持：默认、非空、主键、唯一



*/

```sql
USE students;
DROP TABLE stuinfo;
CREATE TABLE stuinfo(
	id INT PRIMARY KEY,#主键
	stuName VARCHAR(20) NOT NULL UNIQUE,#非空
	gender CHAR(1) CHECK(gender='男' OR gender ='女'),#检查
	seat INT UNIQUE,#唯一
	age INT DEFAULT  18,#默认约束
	majorId INT REFERENCES major(id)#外键

);


CREATE TABLE major(
	id INT PRIMARY KEY,
	majorName VARCHAR(20)
);
```




**查看stuinfo中的所有索引，包括主键、外键、唯一**

```sql
SHOW INDEX FROM stuinfo;
```




###2.添加表级约束
/*

语法：在各个字段的最下面
 【constraint 约束名】 约束类型(字段名) 
*/

~~~sql


```sql
DROP TABLE IF EXISTS stuinfo;
CREATE TABLE stuinfo(
	id INT,
	stuname VARCHAR(20),
	gender CHAR(1),
	seat INT,
	age INT,
	majorid INT,
	

	CONSTRAINT pk PRIMARY KEY(id),#主键
	CONSTRAINT uq UNIQUE(seat),#唯一键
	CONSTRAINT ck CHECK(gender ='男' OR gender  = '女'),#检查
	CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id)#外键

);
```





SHOW INDEX FROM stuinfo;
~~~



**通用的写法：★**

```sql
CREATE TABLE IF NOT EXISTS stuinfo(
	id INT PRIMARY KEY,
	stuname VARCHAR(20),
	sex CHAR(1),
	age INT DEFAULT 18,
	seat INT UNIQUE,
	majorid INT,
	CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id)

);
```



|          | 支持类型       | 可以起约束名       |
| -------- | -------------- | ------------------ |
| 列级约束 | 除了外键       | 不可以             |
| 表级约束 | 除了非空和默认 | 可以，但对主键无效 |



### 3.自增长列

auto_increment:自增长列，不用手动插入值，可以自动提供序列值，默认从1开始，步长为1

​	（1）若要更改起始值：手动插入值

​	（2）若要更改步长：更改系统变量

​				set auto_increment_increment=值

​	（3）一个表至多一个自增长列

​	（4）自增长列只能支持数值型

​	（5）自增长列必须为一个key（MySQL是这样）



##二.修改表时添加约束

/*
1、添加列级约束
alter table 表名 modify column 字段名 字段类型 新约束;

2、添加表级约束
alter table 表名 add 【constraint 约束名】 约束类型(字段名) 【外键的引用】;

*/

```sql
DROP TABLE IF EXISTS stuinfo;
CREATE TABLE stuinfo(
	id INT,
	stuname VARCHAR(20),
	gender CHAR(1),
	seat INT,
	age INT,
	majorid INT
)
DESC stuinfo;
```



###1.添加非空约束
```sql
ALTER TABLE stuinfo MODIFY COLUMN stuname VARCHAR(20)  NOT NULL;
```


###2.添加默认约束
```sql
ALTER TABLE stuinfo MODIFY COLUMN age INT DEFAULT 18;
```


###3.添加主键
①列级约束

```sql
ALTER TABLE stuinfo MODIFY COLUMN id INT PRIMARY KEY;
```



②表级约束

```sql
ALTER TABLE stuinfo ADD PRIMARY KEY(id);
```



###4.添加唯一

①列级约束

```sql
ALTER TABLE stuinfo MODIFY COLUMN seat INT UNIQUE;
```

②表级约束

```sql
ALTER TABLE stuinfo ADD UNIQUE(seat);
```




###5.添加外键
```sql
ALTER TABLE stuinfo ADD CONSTRAINT fk_stuinfo_major FOREIGN KEY(majorid) REFERENCES major(id); 
```



##三.修改表时删除约束

###1.删除非空约束
```sql
ALTER TABLE stuinfo MODIFY COLUMN stuname VARCHAR(20) NULL;
```



###2.删除默认约束
```sql
ALTER TABLE stuinfo MODIFY COLUMN age INT ;
```



###3.删除主键
```sql
ALTER TABLE stuinfo DROP PRIMARY KEY;
```



###4.删除唯一
```sql
ALTER TABLE stuinfo DROP INDEX seat;
```



###5.删除外键
```sql
ALTER TABLE stuinfo DROP FOREIGN KEY fk_stuinfo_major;

SHOW INDEX FROM stuinfo;
```



### **6.级联删除**

```sql
ALTER TABLE stuinfo ADD CONSTRAINT fk_stu_major FOREIGN KEY(majorid) REFERENCES major(id) ON DELETE CASCADE//当majod表的id删除时,表stuinfo majorid也删除
```



### 7.级联置空

```sql
ALTER TABLE stuinfo ADD CONSTRAINT fk_stu_major FOREIGN KEY(majorid) REFERENCES major(id) ON DELETE CASCADE//当majod表的id删除时,表stuinfo majorid置空
```



#七、TCL

/*
Transaction Control Language 事务控制语言

事务：
一个或一组sql语句组成一个执行单元，这个执行单元要么全部执行，要么全部不执行。

案例：转账

​	张三丰  1000
​	郭襄	1000

```sql
update 表 set 张三丰的余额=500 where name='张三丰'
意外
update 表 set 郭襄的余额=1500 where name='郭襄'
```



**事务的特性：**
ACID
①原子性：一个事务不可再分割，要么都执行要么都不执行
②一致性：一个事务执行会使数据从一个一致状态切换到另外一个一致状态
③隔离性：一个事务的执行不受其他事务的干扰
④持久性：一个事务一旦提交，则会永久的改变数据库的数据.



**事务的创建**
①隐式事务：事务没有明显的开启和结束的标记
比如insert、update、delete语句

delete from 表 where id =1;

②显式事务：事务具有明显的开启和结束的标记
前提：必须先设置自动提交功能为禁用

set autocommit=0;

步骤1：开启事务
set autocommit=0;
start transaction;可选的
步骤2：编写事务中的sql语句(select insert update delete)
语句1;
语句2;
...

步骤3：结束事务
commit;提交事务
rollback;回滚事务

savepoint 节点名;设置保存点



**事务的隔离级别：**

|                  | 脏读 | 不可重复读 | 幻读 |
| ---------------- | ---- | ---------- | ---- |
| read uncommitted | √    | √          | √    |
| read committed   | ×    | √          | √    |
| repeatable read  | ×    | ×          | √    |
| serializable     | ×    | ×          | ×    |

mysql中默认 第三个隔离级别 repeatable read
oracle中默认第二个隔离级别 read committed
**查看隔离级别**

```sql
select @@tx_isolation;
```

**设置隔离级别**

```sql
set session|global transaction isolation level 隔离级别;
```



开启事务的语句;

```sql
update 表 set 张三丰的余额=500 where name='张三丰'

update 表 set 郭襄的余额=1500 where name='郭襄' 
结束事务的语句;
```



*/

```sql
SHOW VARIABLES LIKE '%auto%';//显示事务规状态
SHOW ENGINES;
```



##一.演示事务的使用步骤

**开启事务**

```sql
SET autocommit=0;//取消事务自动开启
START TRANSACTION;
```

**编写一组事务的语句**

```sql
UPDATE account SET balance = 1000 WHERE username='张无忌';
UPDATE account SET balance = 1000 WHERE username='赵敏';
```



**结束事务**

```sql
ROLLBACK;//回滚



commit;//提交

SELECT * FROM account;
```




##二.演示事务对于delete和truncate的处理的区别

```sql
SET autocommit=0;
START TRANSACTION;
DELETE FROM account;(支持回滚)/TRUNCATE TABLE account;(不支持回滚)
ROLLBACK;
```



##三.演示savepoint 的使用
```sql
SET autocommit=0;
START TRANSACTION;
DELETE FROM account WHERE id=25;
SAVEPOINT a;#设置保存点
DELETE FROM account WHERE id=28;
ROLLBACK TO a;#回滚到保存点


SELECT * FROM account;
```



#八、视图
/*
含义：虚拟表，和普通表一样使用
mysql5.1版本出现的新特性，是通过表动态生成的数据

比如：舞蹈班和普通班级的对比

|      | 创建语法的关键字 | 是否实际占用物理空间 | 使用                         |
| ---- | ---------------- | -------------------- | ---------------------------- |
| 视图 | create view      | 只是保存了sql逻辑    | 增删改查，只是一般不能增删改 |
| 表   | create table     | 保存了数据           | 增删改查                     |

案例：查询姓张的学生名和专业名

```sql
SELECT stuname,majorname
FROM stuinfo s
INNER JOIN major m ON s.`majorid`= m.`id`
WHERE s.`stuname` LIKE '张%';

CREATE VIEW v1
AS
SELECT stuname,majorname
FROM stuinfo s
INNER JOIN major m ON s.`majorid`= m.`id`;

SELECT * FROM v1 WHERE stuname LIKE '张%';
```




##一.创建视图
/*
语法：
create view 视图名
as
查询语句;

*/
USE myemployees;

1.查询姓名中包含a字符的员工名、部门名和工种信息

①创建

```sql
CREATE VIEW myv1
AS

SELECT last_name,department_name,job_title
FROM employees e
JOIN departments d ON e.department_id  = d.department_id
JOIN jobs j ON j.job_id  = e.job_id;
```



②使用

```sql
SELECT * FROM myv1 WHERE last_name LIKE '%a%';
```





2.查询各部门的平均工资级别

①创建视图查看每个部门的平均工资

```sql
CREATE VIEW myv2
AS
SELECT AVG(salary) ag,department_id
FROM employees
GROUP BY department_id;
```

②使用

```sql
SELECT myv2.`ag`,g.grade_level
FROM myv2
JOIN job_grades g
ON myv2.`ag` BETWEEN g.`lowest_sal` AND g.`highest_sal`;
```



3.查询平均工资最低的部门信息

```sql
SELECT * FROM myv2 ORDER BY ag LIMIT 1;
```



4.查询平均工资最低的部门名和工资

```sql
CREATE VIEW myv3
AS
SELECT * FROM myv2 ORDER BY ag LIMIT 1;


SELECT d.*,m.ag
FROM myv3 m
JOIN departments d
ON m.`department_id`=d.`department_id`;
```




##二.视图的修改

方式一：

/*
create or replace view  视图名
as
查询语句;

*/

```sql
SELECT * FROM myv3 

CREATE OR REPLACE VIEW myv3
AS
SELECT AVG(salary),job_id
FROM employees
GROUP BY job_id;
```



方式二：

/*
语法：
alter view 视图名
as 
查询语句;

*/

```sql
ALTER VIEW myv3
AS
SELECT * FROM employees;
```



##三.删除视图

/*

语法：drop view 视图名,视图名,...;
*/

```sql
DROP VIEW emp_v1,emp_v2,myv3;
```




##四.查看视图

```sql
DESC myv3;

SHOW CREATE VIEW myv3;
```




##五.视图的更新

```sql
CREATE OR REPLACE VIEW myv1
AS
SELECT last_name,email,salary*12*(1+IFNULL(commission_pct,0)) "annual salary"
FROM employees;

CREATE OR REPLACE VIEW myv1
AS
SELECT last_name,email
FROM employees;

SELECT * FROM myv1;
SELECT * FROM employees;
```



1.插入

```sql
INSERT INTO myv1 VALUES('张飞','zf@qq.com');
```



2.修改

```sql
UPDATE myv1 SET last_name = '张无忌' WHERE last_name='张飞';
```

3.删除

```sql
DELETE FROM myv1 WHERE last_name = '张无忌';
```

具备以下特点的视图不允许更新

①包含以下关键字的sql语句：分组函数、distinct、group  by、having、union或者union all

```sql
CREATE OR REPLACE VIEW myv1
AS
SELECT MAX(salary) m,department_id
FROM employees
GROUP BY department_id;

SELECT * FROM myv1;
```



更新

```sql
UPDATE myv1 SET m=9000 WHERE department_id=10;
```



②常量视图

```sql
CREATE OR REPLACE VIEW myv2
AS

SELECT 'john' NAME;

SELECT * FROM myv2;
```



更新

```sql
UPDATE myv2 SET NAME='lucy';
```





③Select中包含子查询

```sql
CREATE OR REPLACE VIEW myv3
AS

SELECT department_id,(SELECT MAX(salary) FROM employees) 最高工资
FROM departments;
```



更新

```sql
SELECT * FROM myv3;
UPDATE myv3 SET 最高工资=100000;
```



④join

```sql
CREATE OR REPLACE VIEW myv4
AS

SELECT last_name,department_name
FROM employees e
JOIN departments d
ON e.department_id  = d.department_id;
```



更新

```sql
SELECT * FROM myv4;
UPDATE myv4 SET last_name  = '张飞' WHERE last_name='Whalen';
INSERT INTO myv4 VALUES('陈真','xxxx');
```



⑤from一个不能更新的视图

```sql
CREATE OR REPLACE VIEW myv5
AS

SELECT * FROM myv3;
```



更新

```sql
SELECT * FROM myv5;

UPDATE myv5 SET 最高工资=10000 WHERE department_id=60;
```



⑥where子句的子查询引用了from子句中的表

```sql
CREATE OR REPLACE VIEW myv6
AS

SELECT last_name,email,salary
FROM employees
WHERE employee_id IN(
	SELECT  manager_id
	FROM employees
	WHERE manager_id IS NOT NULL
);
```



更新

```sql
SELECT * FROM myv6;
UPDATE myv6 SET salary=10000 WHERE last_name = 'k_ing';
```



#九、变量
/*
系统变量：
	全局变量
	会话变量

自定义变量：
	用户变量
	局部变量

*/
##一.系统变量
/*
说明：变量由系统定义，不是用户定义，属于服务器层面
注意：全局变量需要添加global关键字，会话变量需要添加session关键字，如果不写，默认会话级别
使用步骤：
1、查看所有系统变量
show global|【session】variables;
2、查看满足条件的部分系统变量
show global|【session】 variables like '%char%';
3、查看指定的系统变量的值
select @@global|【session】系统变量名;
4、为某个系统变量赋值
方式一：
set global|【session】系统变量名=值;
方式二：
set @@global|【session】系统变量名=值;

*/
###1.全局变量
/*
作用域：针对于所有会话（连接）有效，但不能跨重启
*/

①查看所有全局变量

```
SHOW GLOBAL VARIABLES;
```

②查看满足条件的部分系统变量

```
SHOW GLOBAL VARIABLES LIKE '%char%';
```

③查看指定的系统变量的值

```
SELECT @@global.autocommit;
```

④为某个系统变量赋值

```
SET @@global.autocommit=0;
SET GLOBAL autocommit=0;
```



### 2.会话变量

/*
作用域：针对于当前会话（连接）有效
*/

①查看所有会话变量

```
SHOW SESSION VARIABLES;
```

②查看满足条件的部分会话变量

```
SHOW SESSION VARIABLES LIKE '%char%';
```

③查看指定的会话变量的值

```
SELECT @@autocommit;
SELECT @@session.tx_isolation;
```

④为某个会话变量赋值

```
SET @@session.tx_isolation='read-uncommitted';
SET SESSION tx_isolation='read-committed';
```



##二.自定义变量
/*
说明：变量由用户自定义，而不是系统提供的
使用步骤：
1、声明
2、赋值
3、使用（查看、比较、运算等）
*/

### 1.用户变量

/*
作用域：针对于当前会话（连接）有效，作用域同于会话变量
*/

赋值操作符：=或:=

①声明并初始化

SET @变量名=值;
SET @变量名:=值;
SELECT @变量名:=值;

②赋值（更新变量的值）

方式一：

	SET @变量名=值;
	SET @变量名:=值;
	SELECT @变量名:=值;
方式二：

	SELECT 字段 INTO @变量名
	FROM 表;
③使用（查看变量的值）

```
SELECT @变量名;
```




###2.局部变量
/*
作用域：仅仅在定义它的begin end块中有效
应用在 begin end中的第一句话
*/

①声明

```
DECLARE 变量名 类型;
DECLARE 变量名 类型 【DEFAULT 值】;
```

②赋值（更新变量的值）

方式一：

	SET 局部变量名=值;
	SET 局部变量名:=值;
	SELECT 局部变量名:=值;
方式二：

	SELECT 字段 INTO 具备变量名
	FROM 表;
③使用（查看变量的值）

```
SELECT 局部变量名;
```



案例：声明两个变量，求和并打印

用户变量

```
SET @m=1;
SET @n=1;
SET @sum=@m+@n;
SELECT @sum;
```

局部变量

```
DECLARE m INT DEFAULT 1;
DECLARE n INT DEFAULT 1;
DECLARE SUM INT;
SET SUM=m+n;
SELECT SUM;
```

用户变量和局部变量的对比

|          | 作用域              | 定义位置            | 语法                     |
| -------- | ------------------- | ------------------- | ------------------------ |
| 用户变量 | 当前会话            | 会话的任何地方      | 加@符号，不用指定类型    |
| 局部变量 | 定义它的BEGIN END中 | BEGIN END的第一句话 | 一般不用加@,需要指定类型 |

​						


#十、存储过程
存储过程和函数
/*
存储过程和函数：类似于java中的方法
好处：
1、提高代码的重用性
2、简化操作



*/

/*
含义：一组预先编译好的SQL语句的集合，理解成批处理语句
1、提高代码的重用性
2、简化操作
3、减少了编译次数并且减少了和数据库服务器的连接次数，提高了效率



*/

##一.创建语法

```
CREATE PROCEDURE 存储过程名(参数列表)
BEGIN

	存储过程体（一组合法的SQL语句）

END
```

**注意：**

/*
1、参数列表包含三部分
参数模式  参数名  参数类型
举例：
in stuname varchar(20)

参数模式：
in：该参数可以作为输入，也就是该参数需要调用方传入值
out：该参数可以作为输出，也就是该参数可以作为返回值
inout：该参数既可以作为输入又可以作为输出，也就是该参数既需要传入值，又可以返回值

2、如果存储过程体仅仅只有一句话，begin end可以省略
存储过程体中的每条sql语句的结尾要求必须加分号。
存储过程的结尾可以使用 delimiter 重新设置
语法：
delimiter 结束标记
案例：
delimiter $
*/


##二.调用语法

CALL 存储过程名(实参列表);

--------------------------------案例演示-----------------------------------

1.空参列表

案例：插入到admin表中五条记录

```
SELECT * FROM admin;

DELIMITER $
CREATE PROCEDURE myp1()
BEGIN
	INSERT INTO admin(username,`password`) 
	VALUES('john1','0000'),('lily','0000'),('rose','0000'),('jack','0000'),('tom','0000');
END $


#
```

调用

```
CALL myp1()$
```



2.创建带in模式参数的存储过程

案例1：创建存储过程实现 根据女神名，查询对应的男神信息

```
CREATE PROCEDURE myp2(IN beautyName VARCHAR(20))
BEGIN
	SELECT bo.*
	FROM boys bo
	RIGHT JOIN beauty b ON bo.id = b.boyfriend_id
	WHERE b.name=beautyName;
	

END $
```



调用

```
CALL myp2('柳岩')$
```



案例2 ：创建存储过程实现，用户是否登录成功

```
CREATE PROCEDURE myp4(IN username VARCHAR(20),IN PASSWORD VARCHAR(20))
BEGIN
	DECLARE result INT DEFAULT 0;#声明并初始化
	

	SELECT COUNT(*) INTO result#赋值
	FROM admin
	WHERE admin.username = username
	AND admin.password = PASSWORD;
	
	SELECT IF(result>0,'成功','失败');#使用

END $
```



调用

```
CALL myp3('张飞','8888')$
```



3.创建out 模式参数的存储过程

案例1：根据输入的女神名，返回对应的男神名

```
CREATE PROCEDURE myp6(IN beautyName VARCHAR(20),OUT boyName VARCHAR(20))
BEGIN
	SELECT bo.boyname INTO boyname
	FROM boys bo
	RIGHT JOIN
	beauty b ON b.boyfriend_id = bo.id
	WHERE b.name=beautyName ;
	
END $
```



案例2：根据输入的女神名，返回对应的男神名和魅力值

```
CREATE PROCEDURE myp7(IN beautyName VARCHAR(20),OUT boyName VARCHAR(20),OUT usercp INT) 
BEGIN
	SELECT boys.boyname ,boys.usercp INTO boyname,usercp
	FROM boys 
	RIGHT JOIN
	beauty b ON b.boyfriend_id = boys.id
	WHERE b.name=beautyName ;
	
END $
```



调用

```
CALL myp7('小昭',@name,@cp)$
SELECT @name,@cp$
```



4.创建带inout模式参数的存储过程

案例1：传入a和b两个值，最终a和b都翻倍并返回

```
CREATE PROCEDURE myp8(INOUT a INT ,INOUT b INT)
BEGIN
	SET a=a*2;
	SET b=b*2;
END $
```



```
调用

SET @m=10$
SET @n=20$
CALL myp8(@m,@n)$
SELECT @m,@n
```




##三.删除存储过程
语法：drop procedure 存储过程名

```
DROP PROCEDURE p1;
DROP PROCEDURE p2,p3;#×
```



##四、查看存储过程的信息
```
DESC myp2;×
SHOW CREATE PROCEDURE  myp2;
```



#十一、函数
/*
含义：一组预先编译好的SQL语句的集合，理解成批处理语句
1、提高代码的重用性
2、简化操作
3、减少了编译次数并且减少了和数据库服务器的连接次数，提高了效率

区别：

存储过程：可以有0个返回，也可以有多个返回，适合做批量插入、批量更新
函数：有且仅有1 个返回，适合做处理数据后返回一个结果

*/

##一.创建语法
CREATE FUNCTION 函数名(参数列表) RETURNS 返回类型
BEGIN
	函数体
END
/*

注意：
1.参数列表 包含两部分：
参数名 参数类型

2.函数体：肯定会有return语句，如果没有会报错
如果return语句没有放在函数体的最后也不报错，但不建议

return 值;
3.函数体中仅有一句话，则可以省略begin end
4.使用 delimiter语句设置结束标记

*/

##二、调用语法
SELECT 函数名(参数列表)

------------------------------案例演示----------------------------

1.无参有返回

案例：返回公司的员工个数

```
CREATE FUNCTION myf1() RETURNS INT
BEGIN

	DECLARE c INT DEFAULT 0;#定义局部变量
	SELECT COUNT(*) INTO c#赋值
	FROM employees;
	RETURN c;

END $

SELECT myf1()$
```



2.有参有返回

案例1：根据员工名，返回它的工资

```
CREATE FUNCTION myf2(empName VARCHAR(20)) RETURNS DOUBLE
BEGIN
	SET @sal=0;#定义用户变量 
	SELECT salary INTO @sal   #赋值
	FROM employees
	WHERE last_name = empName;
	

	RETURN @sal;

END $

SELECT myf2('k_ing') $
```



案例2：根据部门名，返回该部门的平均工资

```
CREATE FUNCTION myf3(deptName VARCHAR(20)) RETURNS DOUBLE
BEGIN
	DECLARE sal DOUBLE ;
	SELECT AVG(salary) INTO sal
	FROM employees e
	JOIN departments d ON e.department_id = d.department_id
	WHERE d.department_name=deptName;
	RETURN sal;
END $

SELECT myf3('IT')$
```



##三、查看函数

```
SHOW CREATE FUNCTION myf3;
```



##四、删除函数
```
DROP FUNCTION myf3;
```



案例

一、创建函数，实现传入两个float，返回二者之和

```
CREATE FUNCTION test_fun1(num1 FLOAT,num2 FLOAT) RETURNS FLOAT
BEGIN
	DECLARE SUM FLOAT DEFAULT 0;
	SET SUM=num1+num2;
	RETURN SUM;
END $

SELECT test_fun1(1,2)$
```



# 十二、流程控制结构

/*
顺序、分支、循环

*/

##一、分支结构
1.if函数

/*
语法：if(条件,值1，值2)
功能：实现双分支
应用在begin end中或外面

*/

2.case结构

/*
语法：
情况1：类似于switch
case 变量或表达式
when 值1 then 语句1;
when 值2 then 语句2;
...
else 语句n;
end 

情况2：
case 
when 条件1 then 语句1;
when 条件2 then 语句2;
...
else 语句n;
end 

应用在begin end 中或外面


*/

3.if结构

/*
语法：
if 条件1 then 语句1;
elseif 条件2 then 语句2;
....
else 语句n;
end if;
功能：类似于多重if

只能应用在begin end 中

*/

案例1：创建函数，实现传入成绩，如果成绩>90,返回A，如果成绩>80,返回B，如果成绩>60,返回C，否则返回D

```
CREATE FUNCTION test_if(score FLOAT) RETURNS CHAR
BEGIN
	DECLARE ch CHAR DEFAULT 'A';
	IF score>90 THEN SET ch='A';
	ELSEIF score>80 THEN SET ch='B';
	ELSEIF score>60 THEN SET ch='C';
	ELSE SET ch='D';
	END IF;
	RETURN ch;
	
	
END $

SELECT test_if(87)$
```



案例2：创建存储过程，如果工资<2000,则删除，如果5000>工资>2000,则涨工资1000，否则涨工资500

```
CREATE PROCEDURE test_if_pro(IN sal DOUBLE)
BEGIN
	IF sal<2000 THEN DELETE FROM employees WHERE employees.salary=sal;
	ELSEIF sal>=2000 AND sal<5000 THEN UPDATE employees SET salary=salary+1000 WHERE employees.`salary`=sal;
	ELSE UPDATE employees SET salary=salary+500 WHERE employees.`salary`=sal;
	END IF;
	
END $
```

CALL test_if_pro(2100)$

案例1：创建函数，实现传入成绩，如果成绩>90,返回A，如果成绩>80,返回B，如果成绩>60,返回C，否则返回D

```
CREATE FUNCTION test_case(score FLOAT) RETURNS CHAR
BEGIN 
	DECLARE ch CHAR DEFAULT 'A';
	

	CASE 
	WHEN score>90 THEN SET ch='A';
	WHEN score>80 THEN SET ch='B';
	WHEN score>60 THEN SET ch='C';
	ELSE SET ch='D';
	END CASE;
	
	RETURN ch;

END $

SELECT test_case(56)$
```



##二、循环结构
/*
分类：
while、loop、repeat

循环控制：

iterate类似于 continue，继续，结束本次循环，继续下一次
leave 类似于  break，跳出，结束当前所在的循环

*/

| 名称   | 语法                                                         | 特点             | 位置         |
| ------ | ------------------------------------------------------------ | ---------------- | ------------ |
| while  | Label:while loop_condition<br/>do <br/>     loop_list <br/>End while label; | 先判断后执行     | Beigin end中 |
| repeat | Label:repeat<br/>        loop_list<br/>Util end_condition <br/>end repeat label; | 先执行后判断     | Beigin end中 |
| loop   | Label:loop<br/>        loop_list <br/>End loop label;        | 没有条件的死循环 | Beigin end中 |



1.没有添加循环控制语句

案例：批量插入，根据次数插入到admin表中多条记录

```
DROP PROCEDURE pro_while1$
CREATE PROCEDURE pro_while1(IN insertCount INT)
BEGIN
	DECLARE i INT DEFAULT 1;
	WHILE i<=insertCount DO
		INSERT INTO admin(username,`password`) VALUES(CONCAT('Rose',i),'666');
		SET i=i+1;
	END WHILE;
	
END $

CALL pro_while1(100)$


/*

int i=1;
while(i<=insertcount){

//插入

i++;

}

*/
```



2.添加leave语句

案例：批量插入，根据次数插入到admin表中多条记录，如果次数>20则停止

```
TRUNCATE TABLE admin$
DROP PROCEDURE test_while1$
CREATE PROCEDURE test_while1(IN insertCount INT)
BEGIN
	DECLARE i INT DEFAULT 1;
	a:WHILE i<=insertCount DO
		INSERT INTO admin(username,`password`) VALUES(CONCAT('xiaohua',i),'0000');
		IF i>=20 THEN LEAVE a;
		END IF;
		SET i=i+1;
	END WHILE a;
END $


CALL test_while1(100)$
```




3.添加iterate语句

案例：批量插入，根据次数插入到admin表中多条记录，只插入偶数次

```
TRUNCATE TABLE admin$
DROP PROCEDURE test_while1$
CREATE PROCEDURE test_while1(IN insertCount INT)
BEGIN
	DECLARE i INT DEFAULT 0;
	a:WHILE i<=insertCount DO
		SET i=i+1;
		IF MOD(i,2)!=0 THEN ITERATE a;
		END IF;
		

		INSERT INTO admin(username,`password`) VALUES(CONCAT('xiaohua',i),'0000');
		
	END WHILE a;

END $


CALL test_while1(100)$

/*

int i=0;
while(i<=insertCount){
	i++;
	if(i%2==0){
		continue;
	}
	插入
	
}

*/
```





# 其他要点

## 一.定义顺序

select distinct 查询列表

from 表名 别名

join 表名 别名

on 连接条件

where 筛选条件

group by 分组列表

having 分组后筛选

order by 排序列表

limit 条目数；

## 二.执行顺序

from 子句

join 子句

on 子句

where 子句

group by 子句

having 子句

select 子句

order by 子句

limit 子句

## 三.数据库的三范式

**第一范式（** **1NF** **）：** 字段具有 原子性 , 不可再分 。所有关系型数据库系统都满足第一范式）数据库表中的字段都是单一属性的，不可再分。例如，姓名字段，其中的姓和名必须作为一个整体，无法区分哪部分是姓，哪部分是名，如果要区分出姓和名，必须设计成两个独立的字段。

**第二范式（** **2NF** **）：** 第二范式（ 2NF ）是在 第一范式（ 1NF ）的基础上 建立起来的，即满足第二范式（ 2NF ）必须先满足第一范式（ 1NF ）。要求 数据库表中的每个实例或行必须可以被惟一地区分 。通常需要为表加上一个列，以存储各个实例的惟一标识。这个惟一属性列被称为 主关键字或主键 。

   第二范式（ 2NF ）要求 实体的属性完全依赖于主关键字 。所谓完全依赖是指不能存在仅依赖主关键字一部分的属性，如果存在，那么这个属性和主关键字的这一部分应该分离出来形成一个新的实体，新实体与原实体之间是一对多的关系。为实现区分通常需要为表加上一个列，以存储各个实例的惟一标识。简而言之，第二范式就是非主属性非部分依赖于主关键字。

**第三范式的要求如下：** 满足第三范式（ 3NF ） 必须先满足第二范式（ 2NF ） 。简而言之，第三范式（ 3NF ）要求一个数据库表中 不包含 已在其它表中 已包含的非主关键字信息 。所以第三范式具有如下特征：

1 ，每一列只有一个值

2 ，每一行都能区分。

3 ，每一个表都 不包含其他表已经包含 的非主关键字信息。

例如，帖子表中只能出现发帖人的 id ，而不能出现发帖人的 id ，还同时出现发帖人姓名，否则，只要出现同一发帖人 id 的所有记录，它们中的姓名部分都必须严格保持一致，这就是 数据冗余 。

## 四.数据独立性

数据独立性分为两个方面：

1、物理独立性。用户的应用程序和存储在磁盘上数据库的数据是相互独立的，数据怎样在磁盘中存储是DBMS的管理的，用户不需要了解

​	即数据的物理存储改变了，应用程序不需要改变

2、逻辑独立性。用户的应用程序与数据库中数据的逻辑结构是相互独立的

​    即数据库中数据的逻辑结构改变时，应用程序不需要改变

逻辑独立性更难实现，因为程序对数据的逻辑结构依赖较大

## 五.转义字符

![](D:\Typora\images\zhuanyi.png)

# 错题解析

- **在关系模型中,每一个二维表称为一个？**

答案为关系

解析：关系--二维表

​			属性--字段

​			元组--行记录



- **在一个关系R中，若每个数据项都是不可再分割的，那么R一定属于（）**

答案为第一范式



- **数据库的物理独立性是指**

答案为用户的应用程序与存储在磁盘上数据库中的数据是相互独立的



- **在表或视图上执行除了（ 　）以外的语句都可以激活触发器。**

答案为create

解析：当数据库中表中的数据发生变化时，包括insert,update,delete任意操作，如果我们对该表写了对应的DML触发器，那么该触发器自动执行。



- **数据库系统的核心是**( )

答案为数据库管理系统



- **Access2010 数据库文件的扩展名为 **

答案为accdb



- **不借助第三方工具，怎样查看SQL的执行计划?**

答案为explain



- **数据库在磁盘上的基本组织形式是（ ）。**

答案为文件

解析：数据库中的逻辑组织关系由文件存储，并以文件形式表示

- **在关系模型中，起导航数据作用的是（ ）**

答案为关键码



- **某IT公司人事管理采用专门的人事管理系统来实现。后台数据库名为LF。新来的人事部张经理新官上任，第一件事是要对公司的员工做全面的了解。可是他在访问员工信息表EMPL里的工资和奖金字段的时被拒绝，只能查看该表其他字段。作为LF的开发者你将如何解决这一问题：（   ）**

```
A.废除张经理的数据库用户帐户对表EMPL里的工资列和奖金列的SELECT权限
B.添加张经理到db_datareader角色
C.添加张经理到db_accessadmin角色
D.授予张经理的数据库用户帐户对表EMPL里的工资列和奖金列的SELECT权限。
```

解析：

| db_accessadmin | 可以添加、删除用户的用户               |
| -------------- | -------------------------------------- |
| db_datareader  | 可以查看所有数据库中用户表内数据的用户 |

- **某打车公司将驾驶里程（drivedistanced）超过5000里的司机信息转移到一张称为seniordrivers 的表中,他们的详细情况被记录在表drivers 中，正确的sql为（）**

答案为select * into seniordrivers from drivers where drivedistanced >=5000

解析：

INSERT INTO 语句用于**向表格中插入新的行**。

```sql
INSERT INTO table_name VALUES (值1, 值2,....)
指定所要插入数据的列：
INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)
```

SELECT INTO 语句**从一个表中选取数据，然后把数据插入另一个表中**。常用于创n建表的备份复件或者用于对记录进行存档。

```sql
把所有的列插入新表 
SELECT *
INTO new_table_name [IN externaldatabase] 
FROM old_tablename
只把希望的列插入新表 
SELECT column_name(s)
INTO new_table_name [IN externaldatabase] 
FROM old_tablename
```



- **雇员表EMP 结构如下**
  **( 雇员编号 EMPNO ,  姓名 ENAME ,**
  **工作岗位 JOB , 管理员编号 MGR ,**
  **受雇时间 HIREDATE , 工资 SAL ,**
  **奖金 COMM , 部门编号 DEPTNO );**
  **下列操作语句正确的是：（   ）**

```
A.显示在10和30部门工作并且工资大于5500元的雇员的姓名和工资，列标题显示为Employee和Monthly Salary 语句：SELECT ENAME EMPLOYEE ,SAL &quot;MONTHLY SALARY&quot; FROM EMP WHERE DEPTNO IN(10,30)AND SAL&gt;5500;

B.显示受雇时间在2010年1月1日和2012年12月31日之间的雇员的姓名、工资、及受雇时间，并以受雇时间升序排列。 语句：SELECT ENAME,SAL,HIREDATE FROM EMP WHERE HIREDATE BETWEEN '2010-01-01' AND '2012-12-31' ORDER BY HIREDATE;

C.显示奖金比工资多10％以上的雇员的姓名、工资及奖金。 语句：SELECT ENAME,SAL ,COMM FROM EMP WHERE COMM&gt;SAL*1.1;

D.查询没有奖金且工资低于6500并工作岗位是经理、普通员工、销售员的所有员工信息。 语句：SELECT * FROM EMP WHERE SAL&lt;6500 AND COMM IS NULL AND JOB IN ('经理','普通员工','销售员');
```

解析：

B选项因为between...and后面加日期的话，短日期默认time为00:00:00 因此查询日期只能截止到2012-12-31 00：00：00 并没有当天的记录

C项错在没有加等号

1、《民法通则》第一百五十五条规定： 民法所称的“以上”、“以下”、“以内”、“届满”，包括本数；所称的“不满”、“以外”，不包括本数；
2、《中华人民共和国刑法》第九十九条：本法所称以上、以下、以内，包括本数。
3、对文中“以上”“以下”的语义范围进行总括说明，例如：本办法所称“不足”“不超过”“不满”均不含本级，“以上”均含本级。
4、《中华人民共和国刑法》第九十九条：本法所称以上、以下、以内，包括本数。比如“两年以上”中的“两年”即为本数。本数，就是所称之数，本位之数，参照之数，如：1000元以上，其中1000即是本位之数，所称之数，是参照之数。

D选项没有奖金不是说奖金是空值，可能为0可能为null



- **下面有关sql 语句中 delete truncate的说法正确的是？（）**

```
A.论清理表数据的速度，truncate一般比delete更快
B.truncate命令可以用来删除部分数据。
C.truncate只删除表的数据不删除表的结构
D.delete能够回收高水位
```

答案为AC

解析：

1、处理效率：drop>truscate>delete

2、drop删除整个表；truscate删除全部记录，但不删除表；delete删除部分记录

3、delete不影响所用extent，高水线保持原位置不动；trustcate会将高水线复位。



- **下面哪些字符可能会导致sql注入?**

答案为‘（单引号）

解析：SQL注入的关键是单引号的闭合