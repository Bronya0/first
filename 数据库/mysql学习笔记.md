# 主流数据库介绍

## MySQL

MySQL开源、免费，多操作系统支持。其开发可追溯至1985年，而第一个内部发行版本诞生，已经是1995年。09年Oracle收购了Sun和MySQL。MySQL体积小、速度快、占用资源少，但是安全性、功能性一般。

## Oracle

Oracle开发，收费较高。性能、功能性、安全性较强，多操作系统支持。官方提供技术维护。操作难度较高。

## sqlServer

易用性较强，性价比较高。开放性、安全性一般。

# MySQL入门

## 安装

[安装教程](https://bengtian.club/archives/database01)

## 基础术语

- **数据库:** 数据库是一些关联表的集合。
- **数据表:** 表是数据的矩阵。在一个数据库中的表看起来像一个简单的电子表格。
- **列:** 一列(数据元素) 包含了相同类型的数据, 例如邮政编码的数据。
- **行：**一行（=元组，或记录）是一组相关的数据，例如一条用户订阅的数据。
- **冗余**：存储两倍数据，冗余降低了性能，但提高了数据的安全性。
- **主键**：主键是唯一的。一个数据表中只能包含一个主键。你可以使用主键来查询数据。
- **外键**：外键用于关联两个表。
- **复合键**：复合键（组合键）将多个列作为一个索引键，一般用于复合索引。
- **索引**：使用索引可快速访问数据库表中的特定信息。索引是对数据库表中一列或多列的值进行排序的一种结构。类似于书籍的目录。
- **参照完整性**: 参照的完整性要求关系中不允许引用不存在的实体。与实体完整性是关系模型必须满足的完整性约束条件，目的是保证数据的一致性。

- **表头(header)**: 每一列的名称;
- **列(col)**: 具有相同数据类型的数据的集合;
- **行(row)**: 每一行用来描述某条记录的具体信息;
- **值(value)**: 行的具体信息, 每个值必须与该列的数据类型相同;
- **键(key)**: 键的值在当前列中具有唯一性。

![图片来源于菜鸟教程王网](E:\Chrome\Download\0921_1.jpg)

## 登录

- cmd启动mysql服务：

  ```java
  net start mysql
  ```

* 登录:

  ```
  mysql -u用户名 -p密码  (u,p后不接空格)
  或者:
  mysql -hip地址 -u用户名 -p密码 (本机ip地址127.0.0.1)
  或者:
  mysql --host=ip地址 --user=用户名 --password=密码
  ```

* 退出

  ```
  quit 或者 exit
  ```

## SQL 语句分类

* Data Definition Language (DDL数据定义语言)如：建库，建表
* Data Manipulation Language(DML数据操纵语言)，如：对表中的记录操作增删改
* Data Query Language(DQL数据查询语言)，如：对表中的查询操作
* Data Control Language(DCL数据控制语言)，如：对用户权限的设置

# 语法

## DDL库/表结构操作

### 创建数据库

* 创建数据库

  ```
  CREATE DATABASE 数据库名;
  ```

* 判断数据库是否已经存在，不存在则创建数据库

  ```
  CREATE DATABASE IF NOT EXISTS 数据库名;
  ```

* 创建数据库并指定字符集

  ```
  CREATE DATABASE 数据库名CHARACTER SET 字符集;
  ```

### 查看数据库

```
 SHOW DATABASES;
```

* 查看某个数据库的定义信息

```
show create database 数据库名;
```

### 修改数据库

* 修改字符集

```
ALTER DATABASE 数据库名 DEFAULT CHARACTER SET 字符集;
```

### 删除数据库

```
DROP DATABASE 数据库名;
```

### 使用数据库

* 查看当前正在使用的数据库

```
SELECT DATABASE();  (单数,接括号)
```

* 使用/切换数据库

```
USE 数据库名;
```

### 创建表

```
CREATE TABLE 表名(
	字段名1 字段类型1,
	字段名2 字段类型2
);
注意:
每行结尾为逗号,最后一个字段类型不加逗号,最后一行是); 字符串类型要加(指定字节数)
```

* 数据类型(部分)

|      int      | 整型                                |
| :-----------: | ----------------------------------- |
| float, double | 浮点型                              |
| char, varchar | 字符串型,                           |
|     date      | 日期类型, 格式为年月日:  yyyy-MM-dd |

* 创建一个表结构相同的表

```
create table 新表 like 旧表;
```

### 查看表

```
USE 数据库名;
SHOW TABLES;
```

* 查看表结构

```
DESC  表名;
```

* 查看当时创建该表的语句(`是为了避免混淆)

```
show create table 表名;
```

### 删除表

```
drop table 表名;
```

* 判断表是否存在, 存在则删除

```
drop table if exists 表名;
```

### 修改表

* 添加新列

```
alter table 表名 add 字段名 字段类型;
```

* 修改列类型

```
alter table 表名 modify 列名 新类型;
```

* 同时修改列名和列类型

```
alter table 表名 change 旧列名 新列名 新列类型;
```

* 删除列

```
alter table 表名 drop 列名;
```

* 修改表名

```
rename table 表名 to 新表名;
```

* 修改表字符集

```
alter table 表名 character set 字符集;
```

## DML表数据增删改操作

### 添加记录

* 添加一行完整记录

```
insert into 表名 (字段名1,字段名2...字段名n) values (值1,值2...值n);
或者:
insert into 表名 values (值1,值2...值n);
```

* 插入部分记录

``` 
insert into 表名 (字段名1,字段名2) values (值1,值2);
未说明的记录会自动以null代替
```

注意:

值的类型, 大小范围应该符合要求, 字符和日期型数据应包含在单引号中。MySQL
中也可以使用双引号做为分隔符。

### DOS乱码

* 查看MySQL 内部设置的编码
  查看包含character开头的全局变量:

  ```
  show variables like 'character%';
  ```

* 修改client、connection、results的编码为GBK，保证和DOS命令行编码保持一致

* 同时设置三项

  ```
  set names gbk;
  ```

  注意：退出DOS 命令行就失效了，需要每次都配置

### 蠕虫复制

将一张表中的数据复制到另一张表中

* 将表2 所有数据复制到表1

```
insert into 表1 select * from 表1;
```

* 将表2 部分数据复制到表1 

```
insert into 表1 select 列名1,列名2... from 表1;
```

### 更新记录

* 无条件修改,修改所有行

```
update 表名 set 字段名=值;
```

* 指定位置修改

```
update 表名 set 字段名=值 where 字段名=值;
```

### 删除记录

* 无条件逐条删除数据

```
delete from 表名;
```

* 指定位置删除数据

```
delete from 表名 where 字段名=值;
```

* truncate删除全部数据

```
truncate table 表名;
```

truncate 和delete区别:

从效果上来看：truncate是删除整个表，然后重构整个表。delete只是删除逐条删除没一条数据。

从空间上来看：delete会产生碎片，并不会释放空间，而truncate不会产生碎片。

从事务的角度：truncate不可以回滚，delete可以回滚。

## DQL查询表数据

### 简单查询

* 查表所有列的数据

```
select * from 表名;
```

* 查指定列数据

```
select 列名1,列名2...列名n from 表名;
```

### 指定别名进行查询

以自己想要的别名显示,不会对真实的列名造成影响

* 指定列的别名查询

```
select 字段名1 as 别名1,字段名2 as 别名2... from 表名;
```

* 对列和表同时指定别名

```
select 字段名1 as 别名, 字段名2 as 别名... from 表名 as 表别名;
```

别名字符不用加引号.

### 清除重复值

* 查询指定列并要求结果不显示重复值

```
select distinct 字段名 from 表名;
```

### 查询结果参与运算

* 某列数据和固定值运算

```
SELECT 列名1 + 固定值 FROM 表名;
```

*  某列数据和其他列数据参与运算

```
SELECT 列名1 + 列名2 FROM 表名;
```

注意: 参与运算的必须是数值类型.

### 条件查询

```
select 字段名 from 表名 where 条件;
```

* 参与条件构成的运算符

| 、<、<=、>=、=、<> | <>在SQL 中表示不等于，在mysql 中也可以使用!=,  没有==        |
| :----------------: | ------------------------------------------------------------ |
|   BETWEEN...AND    | 在一个范围之内，如：between 100 and 200 相当于条件在100到200之间，包头又包尾 |
|      IN(集合)      | 集合表示多个值，使用逗号分隔                                 |
|     LIKE '张%'     | 模糊查询                                                     |
|      IS NULL       | 查询某一列为NULL 的值，注：不能写=NULL                       |

| and 或 &&  | 与,前者使用较多 |
| :--------: | --------------- |
| or 或 \|\| | 或              |
|  not 或 !  | 非              |

例如:

select * from student where math >120 and English > 110;



* in关键字

  in后括号作为省略多个or关键字的简略写法, 只要有 字段=某个值 的数据则被查询

```
select 字段名 from 表名 where 某字段 in(值1,值2...值n);
```

例如:

select * from student3 where id not in(1,3,5,7);



* 范围查询

where后是查询指定字段的条件

```
select 字段名 from 表名 where 某字段 between 值1 and 值2;
```



* 模糊查询 like

```
SELECT * FROM 表名 WHERE 字段名 LIKE '通配符字符串';
```

通配符字符串: 

% : 替代任意多个字符串(包括0个)

_   : 替代一个任意字符串

* 例如:

  查询姓名中包含'钟'字的学生
  select * from student where name like '%钟%';

  查询姓王，且姓名有两个字的学生
  select * from student where name like '王_';

