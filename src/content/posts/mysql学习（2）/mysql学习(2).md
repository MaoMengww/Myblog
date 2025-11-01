---
title: mysql学习2（函数约束和多表查询）
published: 2025-10-27
description: mysql学习（函数约束和多表查询）
image: "./cover.jpg"
tags: ["mysql"]
category: mysql
draft: false
---

## 导言

最近十几天写了几个小demo,抄着康神写了一个不算很大的单体项目，虽然感觉经验增长了，但是知识方面却并没有增长。虽然自己能勉强看懂一些项目，但实际上基础方面还是松松垮垮的，有的也只是能用，而没有系统性的学习。因此准备把mysql还有redis之类的东西系统性的学的更深一点。其实学的过程中能够感觉到更基础的知识的重要性比如操作系统，计算机网络。可是总提不起勇气去学习，因此也就只能暂时先走一步看一步了。本文就是我照着黑马的视频重拾mysql,同时参考了越神的笔记，不要骂我抄袭（滑跪）。golang选手也要看黑马（哭）

## 函数

### 函数是什么：

函数是指一段可以直接被另一段程序调用的程序或代码

### 函数的分类：

#### 1.字符串函数：

| 函数                       | 功能                                                         |
| -------------------------- | ------------------------------------------------------------ |
| CONCAT(s1, s2, ..., sn)    | 字符串拼接，将s1, s2, ..., sn 拼接成一个字符串               |
| LOWER(str)                 | 将字符串全部转换为小写                                       |
| UPPER(str)                 | 将字符串全部转换为大写                                       |
| LPAD(str, n, pad)          | 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度    |
| RPAD(str, n, pad)          | 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度    |
| TRIM(str)                  | 去掉字符串头部和尾部的空格                                   |
| SUBSTRING(str, start, len) | 截取字符串，从start(索引)开始截取长度为len的字符串，**索引从1开始** |



#### 2.数值函数：

| 函数        | 作用                       |
| ----------- | -------------------------- |
| CEIL(x)     | 向上取整                   |
| FLOOR(x)    | 向下取整                   |
| MOD(x, y)   | 返回x/y的模                |
| RAND()      | 返回0~1内的随机数          |
| ROUND(x, y) | 对x的四舍五入，保留y位小数 |



```go
//案例：通过数据库函数，生成随机六位验证码
SELECT ROUND(RAND()*1000000,0)
```

#### 3.日期函数

| 函数                               | 作用                                                        |
| ---------------------------------- | ----------------------------------------------------------- |
| CURDATE()                          | 返回当前日期                                                |
| CURTIME()                          | 返回当前时间                                                |
| NOW()                              | 返回当前日期和时间                                          |
| YEAR(date)                         | 返回指定日期的年份                                          |
| MONTH(date)                        | 返回指定日期的月份                                          |
| DAY(date)                          | 返回指定日期的天数                                          |
| DATE_ADD(date, INTERVAL expr type) | 返回一个日期/时间值加上一个时间间隔expr后的时间间隔         |
| DATEDIFF(date1, date2)             | 返回起始时间dete2和结束时间date1之间的天数（date1 - date2） |

#### 4.流程函数

| 函数                                                     | 功能                                                   |
| -------------------------------------------------------- | ------------------------------------------------------ |
| IF(value, t, f)                                          | 如果value为true,返回t,否则返回f                        |
| IFNULL(Value1, Value2)                                   | 如果value不为空，返回Value1，否则返回Value2            |
| CASE WHEN [if1] THEN [res1]...ELSE [default] END         | 如果if1为true,返回res1, ...否则返回default默认值       |
| CASE [expr] WHEN [val1] THEN [res1]...ELSE [default] END | 如果expr的值为val1，返回res1, ...否则返回default默认值 |



## 约束

### 约束是什么：

约束时作用于表中字段上的规则，用于限制存储在表中的数据。目的是保证数据库中的数据的完整性，有效性和正确性

### 分类

| 约束条件 | 关键字         | 说明                                                       |
| -------- | -------------- | ---------------------------------------------------------- |
| 主键约束 | PRIMARY KEY    | 唯一标识表中的每一行，主键列的值不能重复且不能为空         |
| 自动增长 | AUTO_INCREMENT | 使字段在插入新行时自动增长（常用于主键）                   |
| 非空约束 | NOT NULL       | 限制该字段的数据不能为null                                 |
| 默认约束 | DEFAULT        | 保存数据时，如果未指定该字段的值，则采用默认值             |
| 唯一约束 | UNIQUE         | 保证该字段的所有数据都是唯一的，不重复的                   |
| 检查约束 | CHECK          | 保证字段满足某一个条件                                     |
| 外键约束 | FOREIGN KEY    | 用来让两张表的的数据之间建立联系，保证数据的一致性和完整性 |



tips:传统企业为了保证数据的一致性和完整性使用外键较多，但是大型互联网公司使用较少，原因是：

1.性能开销大

2.分布式/微服务架构的限制

3.数据库变更和分库分表的复杂性

### 外键的删除与更新行为

```sql
CREATE TABLE <child_table_name> (
    <column_name> <data_type>,
    <other_column_name> <data_type>,
    ...
    [CONSTRAINT <constraint_name>]  -- (可选) 约束的名字
    FOREIGN KEY (<child_column_name>)
    REFERENCES <parent_table_name> (<parent_column_name>)
    [ON DELETE <action>]  -- (可选)
    [ON UPDATE <action>]  -- (可选)
);
```



| 行为        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| NO ACTION   | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。（与REESTRICYT一致） |
| REESTRICYT  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。（与NO ACTION一致） |
| CASCADE     | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有，则也删除/更新外键在子表中的记录 |
| SET NULL    | 当在父表中删除对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（需要要求该外键允许取null） |
| SET DEFAULT | 父表有变更时，子表将外键列设置成一个默认的值（Innodb不支持） |

## 多表查询

### 多表关系

#### 1.一对多：

案例：部门与员工

关系:一个部门对应多个员工，一个员工对应一个部门

实现：**再多的一方建立外键，指向一的一方的主键**

#### 2.多对多

案例：学生与课程

关系：一个学生可以选修多个课程，一门课程也可以供多个学生选择

实现：**建立第三张中间表，中间表至少包含两个外键，分别关联两方主键**

![image-20251026183307710](D:\image\typora文件图集\image-20251026183307710.png)



#### 3.一对一

案例：用户与用户详情的关系

关系：一对一

实现：在任意一方加入外键，关联另外一方的主键，并且设置外键为唯一的（UNIQUE）

目的：1.性能优化（最主要的原因）；2.处理稀疏数据（优化 NULL）；3. 安全与权限控制；4.逻辑组织（概念清晰）



### 概述

概述：指从多张表中查询数据

笛卡尔积：两个集合的所有组合情况（在多表查询时，需要消除无效的笛卡尔积）

```sql
SELECT * FROM a, b WHERE [a.b_id = b.id]   //[]内内容目的为消除无效笛卡尔积 
```



![image-20251026191339726](D:\image\typora文件图集\image-20251026191339726.png)

**注意这张图在之后会用**



### 内连接

查询的是两张表交集的部分，即A∩B

##### 内连接查询语法

1.隐式内连接

```sql
SELECT column_name FROM table1, table2 WHERE 条件...;  //条件例：a.b_id = b.id
SELECT e.name, d.name FROM table1 AS e, table2 AS d WHERE 条件...;  
//e.id = d.id，e和d为别名，减少繁琐的名字输入，取别名后无法使用原名限定
```

2.显示内连接

```sql
SELECT column_name From table1 [INNER] JOIN table2 ON 连接条件;  //条件例：
```

tips:显示内连接可以减少字段的扫描，有更快的执行速度，在多表连接时优势明显

### 外连接

##### 外连接查询语法

1.左外连接

查询的是A,包含A∩B

```sql
SELECT column_name From table1 LEFT [OUTER] JOIN table2 ON 连接条件;
```

2.右外连接

查询的是B,包含A∩B

```sql
SELECT column_name From table1 RIGHT [OUTER] JOIN table2 ON 连接条件;
```

### 自连接

自己查询自己

可以是内连接也可以是外连接，就是要把自己和自己当成两张表

##### 自连接查询语法

```sql
SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件 ...;
```

示例：

1.查询员工及其所属领导的名字

```sql
SELECT a.name, b.name FROM employee AS a, employee AS b WHERE a.manager = b.id;
```

把employee和employee当成两张表

2.查询没有领导的员工

```sql
SELECT a.name, b.name FROM employee AS a LEFT JOIN employee AS b ON a.manager = b.id;
```



### 联合查询

把多次查询的结果合并起来，形成一个新的查询结果

```sql
SELECT 字段列表 FROM 表A
UNION [ALL]                     //不加all会去重，加all不会去重，会把两次查询的结果直接拼接
SELECT 字段列表 FROM 表B;
```



### 子查询

#### 概念

SQL语句中嵌套SELECT语句，称为**嵌套查询**，又称**子查询**

```sql
SELECT*FROM t1 WHERE column1 = (SELECT column1 FROM t2);
```

**子查询外部的语句可以是SELECT/INSERT/UPDATE/DELETE中的任意一个**

#### 分类

###### 1.标量子查询

​	子查询返回的结果是单个值（数字，字符串，日期等）最简单的形式，这种子查询称为**标量子查询**

常见的操作符：= <> > >= < <=

​	

```sql
//根据销售部门id，查询员工信息
SELECT * FROM emp where dept_id = (seclet id from dept where name = '销售部'); 
//查询指定入职日期之后入职的员工信息
SELECT * FROM rmp WHERE entrydate > '2006-02-12';
```

###### 2.列子查询

​	子查询返回的结果是一列（可以是多行），这种子查询称为**列子查询**

常用的操作符：IN, NOT IN, SOME, ALL



| 操作符 | 描述                                   |
| ------ | -------------------------------------- |
| IN     | 在指定的集合范围之内，多选一           |
| NOT IN | 不在指定的集合范围之内                 |
| ANY    | 子查询返回的列表中，有任意一个满足即可 |
| SOME   | 与ANY等同，使用SOME的地方都可以使用ANY |
| ALL    | 子查询返回列表的所有值都必须满足       |

```sql
SELECT * FROM  emp WHERE dept_id IN (SELECT id FROM dept WHERE name = '销售部' or name = '市场部');
```

###### 3.行子查询

​	子查询返回的结果是一行（可以是多列），这种子查询称为**行子查询**

常用的操作符： = , <>, IN, NOT IN

```sql
SELECT * FROM emp WHERE entrydate > (SELECT entrydate FROM empt WHERE name = 'cyjj');
```

###### 3.表子查询

​	子查询返回的结果是多行多列，这种子查询称为**表子查询**

常用的操作符：IN

```sql
SELECT * FROM emp WHERE (job, salary) in ( SELECT job, salary FROM emp WHERE name = 'kqgg' or name = 'rtjj');
```

## 结语

​	学计算机真是越学越觉得自己的渺小，越学感觉自己越菜，不过探索的过程是快乐的。马上就可以喝到wyhjj的奶茶了，hhh