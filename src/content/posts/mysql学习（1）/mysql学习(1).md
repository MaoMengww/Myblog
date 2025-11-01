---
title: mysql学习1（数据库的操作和值的类型）
published: 2025-09-30
description: mysql学习（数据库的操作和值的类型））
image: "./cover.jpg"
tags: ["mysql"]
category: mysql
draft: false
---

### 一.什么是Mysql

mysql是一个开源的关系型数据库，广泛地被用以在各种场景各种规模的数据存储，具有高性能和可靠性，能够有效处理大量并发连接和用户请求

### 二.Mysql数据库的操作

mysql使用sql语句对数据库进行操作，sql是学习mysql数据库的必学

#### sql语句的分类

##### 1.DDL(Data Definition Language)数据定义语言。主要用途是操作数据库、表、列等。常用语句有CREATE、ALTER、DROP、TRUNCATE、COMMENT等

###### CREATE语句：用于创建新的数据库对象（比如数据库、表、视图、索引等）

###### ALTER语句: 用于修改已存在的数据库对象。例如，修改表的结构，添加、删除、修改列，添加约束等。

###### DROP语句：用于删除数据库对象，如表、视图、索引等。执行 `DROP` 语句后，数据和对象的定义都会被永久删除。

###### TRUNCATE语句：适用于删除表中的数据，但不会删除表中的结构，不逐行删除数据，执行效率高

###### RENAME语句：用于重命名数据库对象，通常用于修改表名或列名。

###### **COMMENT**语句：用于修改对象的名称，而不影响其数据和结构。

##### 2.DML(Data Manipulation Language)数据操作语言.主要用于操作数据表里的数据。常用的有INSERT、 UPDATE、 DELETE等

###### **INSERT**：用于向表中插入新的数据行。

###### **UPDATE**：用于修改表中已存在的数据。

######  **DELETE**：用于从表中删除数据行。

##### 3.DQL（Data Query Language）数据查询语言。专门用于从数据库表中检索数据，核心是SELECT

##### 4.DCL(Data Control Language) 数据控制语言。用于管理数据库的访问权限



### sql对数据库和表的操作

一个完整的sql操作以；结尾

```sql
# 数据库操作 #
CREATE DATABASE database_name; //创建数据库
SHOW DATABASE;   //显示数据库
USE database_name;   //选择数据库
DROP DATABASE database_name;  //删除数据库

# 表操作 #
CREATE TABLE table_name (
    column_name1 datatype constraints,
    column_name2 datatype constraints,
    ...
    column_nameN datatype constraints,
    table_constraints
);   //创建表.constraints列级约束；table_constraints表级约束
SHOW TABLES;  //查看表
DROP TABLE table_name;   //删除表
ALTER TABLE table_name1 RENAME TO table_name2;  //修改表名
ALTER TABLE table_name ADD column_name datatype; //增加列
ALTER TABLE table_name MODIFY column_name datatype; //修改列的数据类型
ALTER TABLE table_name DROP COLUMN column_name; //删除列
//略，添加约束等
```



### sql对数据的操作

```sql
# 插入数据 #
INSERT INTO table_name (column1, column2, column3, ...) VALUES (value1, value2, value3, ...);   //插入单行数据
INSERT INTO table_name (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)(value1, value2, value3, ...)...;   //插入多行数据
//每个字段与其值是严格一一对应的。也就是说：每个值、值的顺序、值的类型必须与对应的字段相匹配。

# 更新数据 #
UPDATE table_name SET column1 = value1, column2 = value2, ... [WHERE 条件表达式];

# 删除数据 #
DELETE FROM table_name [WHERE 条件表达式];
```



### 三.数据库的数据类型（图片源自网络）

![QQ20250930-145504](D:\image\typora文件图集\QQ20250930-145504.png)

![QQ20250930-145537](D:\image\typora文件图集\QQ20250930-145537.png)

![QQ20250930-145552](D:\image\typora文件图集\QQ20250930-145552.png)

![QQ20250930-145603](D:\image\typora文件图集\QQ20250930-145603.png)

![QQ20250930-145620](D:\image\typora文件图集\QQ20250930-145620.png)