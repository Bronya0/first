# DQL查询（续）

```
SELECT * 字段列表 as别名 FROM表名 WHERE子句 GROUP BY子句 HAVING子句 ORDER BY子句 LIMIT子句;
```

## 排序查询

* **单列排序(仅仅对显示结果排序)**

```
select 字段名 from 表名 where 字段=值 order by 字段名 asc/desc
```

asc: 升序; desc: 降序

例句:  

select * from student order by age desc;

* **组合排序(仅仅对显示结果排序)**

同时对多个字段进行排序，如果第1个字段相等，则按第2个字段排序，依次类推。

```
select 字段名 from 表名 order by 字段名1 asc/desc, 字段名2 asc/desc ...
```

例句:

select * from student order by age desc, grade desc;

## 聚合函数

之前做的查询都是横向查询，它们都是根据条件一行一行的进行判断，而使用聚合函数查询是纵向查询，
它是对一列的值进行计算，然后返回一个结果值。聚合函数会忽略空值NULL。

* **五个聚合函数:**

max, min, avg, count, sum

```
select 聚合函数(列名) from 表名;
```

例句: 

--统计30岁以上总人数( 注意null值的处理 )

select count( ifnull( id,0 ) ) from student where age>30;

* **ifnull函数**

```
IFNULL(字段名,默认值)  如果字段为null则使用默认值替代
```

## 分组查询

一般分组会跟聚合函数一起使用

* **语法**

```
select 显示字段名1,2 from 表 group by 分组字段 having 条件;
```

例句:

select sex, count(*) from student3 group by sex;

select sex, avg(math) from student3 group by sex;

select sex, count(*) from student3 where age > 25 group by sex ;

--先筛选age, 剩下的再分组性别, 再对分组后的数据筛选统计数目大于2的.

```
SELECT sex, COUNT(*) FROM student3 WHERE age > 25 GROUP BY sex having COUNT(*) >2;
```

* **where和having区别**

where:

1)对查询结果进行分组前，将不符合where条件的行去掉，即在分组之前过滤数据，即先过滤再分组。
2) where后面不可以使用聚合函数

having:

1) having子句的作用是筛选满足条件的组，即在分组之后过滤数据，即先分组再过滤。
2) having后面可以使用聚合函数

## 限制查询

LIMIT的作用就是限制查询记录的条数。

```
LIMIT offset,length;
```

offset：起始行数，**从0开始计数**(也就是说第二行开始为则值1)，如果省略，默认就是0
length：返回的行数

例句:

select * from student3 limit 5;

# 数据库备份

可以在图形化工具里备份,也可以使用命令:

dos 未登录时备份:

```
mysqldump -u 用户名 -p 密码 数据库 > 文件的路径
```

登录后还原:

```
USE 数据库；
SOURCE 导入文件的路径;
```

例句:

-- 备份day21数据库中的数据到d:\day21.sql文件中
mysqldump -uroot -proot day21 > d:/day21.sql

# 数据库约束

对表中的数据进行限制，保证数据的正确性、有效性和完整性。一个表如果添加了约束，不正确的数据将无
法插入到表中。约束在创建表的时候添加比较合适

* **种类及关键字**:

主键: primary

唯一: unique

非空: not null

外键: foreign key

检查约束: check (mysql里没有)

## 主键

每条记录的唯一标识, 不用业务字段做主键, 另起id列. 仅仅给程序使用, 不要涉及业务.

主键特点: **非空**, **唯一性**

* **创建主键**

1.  若在创建表时添加主键:

```
字段名 字段类型 primary key,
```

2. 给已有的表添加主键

```
alter table 表名 add primary key(字段名);
```

* **删除主键**

```
alter table 表名 drop primary key;
```

* **主键自增**

希望在插入新记录时,系统自动增加主键的值,必须是int

```
create table 表名(
 id int primary key auto_increment,
 ...
 );
```

* **修改起始值**

  默认地AUTO_INCREMENT的开始值是1，如果希望修改起始值:

```
create table 表名(
 id int primary key auto_increment,
 ...
 )auto_increment=起始值;
```

或者对已经创建的表修改:

```
alter table 表名 auto_increment = 初始值;
```

* delete与truncate对主键自增长影响

delete删除对主键的值没有影响, 原来是几删除后还是几, 没有自动重新生成

truncate后,主键自增长重新开始

## 唯一约束

创建表时添加

```
字段名 字段类型 unique,
```

给已经建好的表添加:

```
alter table 表名 add unique(字段名);
```

注意: null不是数据, 可以重复存在

## 非空约束

某一列不能为null

创建表时:

```
字段名 字段类型 not null(字段名),
```

后添加:

```
alter table 表名 not null(字段名);
```

## 默认值

--创建表时使用默认值

```
create table st9 (
id int,
name varchar(20),
address varchar(20) default '广州'
)
```

-- 添加一条记录,使用默认地址
insert into st9 values (1, '李四', default);

## 外键约束

**从表中对应每个主键的一列作为外键, 被主表的主键所约束. 主表中主键列对应从表中外键列.**

创建从表时:

```
constraint 外键约束名 foreign key(从表外键名) references 主表(主键名)
```

后添加:

```
alter table 从表名 add CONSTRAINT 外键约束名 foreign key(外键名) references 主表(主键名);
```

其中,constraint 自定义约束名 这部分可以省略

* **删除外键**

```
alter table 表名 drop foreign key 外键名;
```

* **外键的级联操作**

修改或删除主表主键时，同时更新和删除附表外键的操作。

语法：

只能在创建表时建立级联关系：

```
on update cascade
```

级联删除:

```
on delete cascade
```

例如:

```
create table employee(
id int primary key auto_increment,
name varchar(20),
age int,
dep_id int, -- 外键对应主表的主键
-- 创建外键约束
constraint emp_depid_fk foreign key (dep_id) references
department(id) on update cascade on delete cascade
)
```

# 表之间的关系

**注意不是指表的数量的关系,而是表的字段的包含关系.**

* **一对多**

例如班级和学生，部门和员工，客户和订单，总类别和个体。

建表原则：

多方（从表）创建字段作外键 指向 单方（主表）的主键。

* **多对多**

老师和学生，学生和课本

建表原则：

创建中间表，该表有多个字段（id1, id2...）分别作为外键指向各自对应的表的主键(id1,id2...)

* **一对一**

一对一关系可以创成一张表, 多个表的话:

从表只能有一个主键, 该主键同时作为外键指向主表主键.

# 数据库设计

## 范式

目前关系数据库有六种范式：第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式(4NF）和第五范式（5NF，又称完美范式）. 

并非并列,而是递进关系. 满足更多设计要求则是更高层次范式.

* **第一范式（1NF）**

原子性:  每列都是不可再分割的数据项, 不能是集合或数组等非原子数据项.

* **第二范式（2NF）**

第二范式就是在第一范式的基础上每一列全部都依赖于主键列。(一表一主键)

每张表应该只描述一件事. 不产生局部依赖.

反例: 如果有两个不同的主键列在同一张表, 则每一列不完全依赖主键列.

* **第三范式（3NF）**

在满足第二范式的前提下，表中的每一列都**直接**依赖于主键，而不是通过其它的列来间接依赖于主键。

不产生传递依赖.