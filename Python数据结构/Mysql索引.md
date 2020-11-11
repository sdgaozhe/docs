> `MySQL`在建表的时候会自动对表的主键和外键字段添加索引，我们可以使用 ` show index from tablename;`进行查看，`MySQL`索引类型分为普通索引、主键索引、联合索引、唯一索引、全文索引，本文我们主要讨论普通索引和联合索引的使用情况，已经各索引失效的情况。

### 建表

> 首先我们简单的建立一个表

```mysql
mysql> CREATE TABLE Student(
    -> id int not null auto_increment primary key,
    -> name varchar(20) not null comment '姓名',
    -> score float not null comment '成绩',
    -> index (name)
    -> );
Query OK, 0 rows affected (0.03 sec)
```

### 主键索引/普通(单列)索引

> 我们通过查看索引和建表过程可以看到，当我们指定了Student表的主键，MySQL会自动为我们建立索引，其中`PRIMARY KEY (id)`就是建立主键索引，`KEY name(name)` 就是建立普通索引。

- 通过` show index from tablename;`查看索引

![image-20201014153925316](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20201014153925316.png)

- 通过` show create table Student\G`查看建表过程

```mysql
mysql> show create table Student\G
*************************** 1. row ***************************
       Table: Student
Create Table: CREATE TABLE `student` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(20) COLLATE utf8mb4_general_ci NOT NULL COMMENT '姓名',
  `score` float NOT NULL COMMENT '成绩',
  PRIMARY KEY (`id`),
  KEY `name` (`name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci
1 row in set (0.01 sec)
```

### 联合(多列)索引

