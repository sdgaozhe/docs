## 开始之前

- 查看数据库: `show databases`
- 查看当前数据库：`select database()`
- 查看数据表：`show tables`
- 查看表结构：`desc tablename;`
- 建库utf8：`CREATE DATABASE blog DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;`
- 建库utf8mb4：`CREATE DATABASE blog DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;`
- 建表：`create table students id int not null auto_increment primary key,name varchar(30));`
- 优先级：小括号，not，比较运算符，逻辑运算符
- 优先级：and比or先运算，如果同时出现并希望先算or，需要结合()使用
- 有符号/无符号：有符号可以有负数（正负均等），无符号数大于等于零
- utf-8中文占三个字节

## 数据类型

### 数值类型

- 



## 条件查询

### 比较运算符

- 运算符：`>` `<` `>=` `<=` `!=` `=`

1. 查寻pid<3得数据项
`select * from product where pid < 3;`
2. 查询pid<3且只查看pname字段
`select pname from product where pid < 3;`

### 逻辑运算符

- 运算符：`or` `and` `not`

1. 查询pid<3且is_hot=1的数据项
`select * from product where pid<3 and is_hot=1;`

### 模糊查询

- 运算符：`like` `%` `_`

1. 查询pname开头为小的数据项
`select * from product where pname like "小%";`
2. 查询pname包含兴的数据项
`select * from product where pname like "%兴%";`

3. 查询姓黄并且名字是一个字的学生

` select * from product where pname like "小_";`

### 范围查询

- 运算符：`in` `between...and`
1. 查看market_price是649或1399的数据项
` select * from product where market_price in (649, 1399)`
2. 查看market_price在1000到2000的数据项
`select * from product where market_price between 1000 and 2000;`

### 空判断
- 注意：`null != ""`
- 运算符：`is null` `is not null` 
1. 查看punit为空的数据项
`select * from product where punit is null;`

## 聚合查询

- count (返回数据项数目)

`select count(*) from product;`

- max/min (返回字段的最大/小值)

`select max(market_price) from product;`

- sum （返回该字段的总和）

`select sum(market_price) from product;`

- avg (返回该字段的平均值)

`select avg(market_price) from product;`

- date(日期查询,查询当天的数据)

` select * from wyt2 where date(t1) = '2019-10-18';`

## 分组查询
### where/having区别

- where是对from后面指定的表进行数据筛选，属于对原始数据的筛选
- having是对group by的结果进行筛选

- 查询不同性别有多少人数

```
mysql> select sex as 性别, count(*) from user group by sex;
+--------+----------+
| 性别   | count(*) |
+--------+----------+
| NULL   |        6 |
| 男     |        5 |
+--------+----------+
```

- 查询男性有多少人

```
select sex as 性别, count(*) from user group by sex having sex="男";
+--------+----------+
| 性别   | count(*) |
+--------+----------+
| 男     |        5 |
+--------+----------+
```

```
mysql> select sex as 性别,count(*) from user where sex =  "男";
+--------+----------+
| 性别   | count(*) |
+--------+----------+
| 男     |        5 |
+--------+----------+
```

## 排序

- 默认升序
- asc从小到大排列，即升序
- desc从大到小排序，即降序
1. 按market_price升序查询is_hot=1的数据项

`select * from product where is_hot=1 order by market_price ;`

## 分页

- 每页显示m条数据，当前显示第n页

```
select * from students
where isdelete=0
limit (n-1)*m,m
```

## 数据操作（insert、update、delete）
### 插入数据

```
insert into students (sname) values ("Gage");
```

### 修改数据

```
update students set sname="Fovegage" where id = 1;
```

### 删除数据

```
delete from students where id =1;
```

## 字段操作(add、change、drop)

### 修改字段的长度

```
ALTER TABLE attence MODIFY COLUMN id INT(20)
```

### 新增字段

```
ALTER TABLE attence ADD COLUMN age VARCHAR(20) NOT NULL;
```

### 修改字段

```
ALTER TABLE attence CHANGE attence_name NAME  VARCHAR(20)
```
### 删除字段

```
ALTER TABLE attence DROP COLUMN age;
```

## 删除操作

### 表

 - 删除表：`DROP table tablename;`
 - 清空表：`delete from tablename;` # 会在二进制中记录
 - 清空表：`truncate table tablename;` # 直接删除，不会在二进制中记录
 - 删库：`drop database databasename;`