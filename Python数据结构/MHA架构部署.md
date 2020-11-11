### 环境准备

- 确保MySQL版本大于5.7
- 确保防火漆关闭或开启各服务器的3306端口

## MySQL配置

- 查看是否开启了GTID模式

![image-20201110163302003](https://oscimg.oschina.net/oscnet/up-861dc1fb7461ef434c797bb12be09413e0b.png)

- 若未开启在my.cnf文件最后添加，开启GTID（主）

  ```
  # must params
  server_id =  100
  enforce_gtid_consistency       = on
  gtid_mode                      = on
  
  # binlog
  log_bin = mysqlbin
  log_slave_updates = 1
  binlog_format = row
  
  # relay log
  skip_slave_start = 1
  ```

- 若未开启在my.cnf文件最后添加，开启GTID（从）

  > 需要注意的是，若配置多台MySQL从服务器，确保server_id唯一

  ```
  # must params
  server_id =  101
  enforce_gtid_consistency       = on
  gtid_mode                      = on
  
  # binlog
  log_bin = mysqlbin
  binlog_format = row
  master_info_repository = TABLE
  
  # relay log
  relay_log_info_repository = TABLE
  ```

- 重启数据库

  ````
  systemctl restart mysqld
  ````

- 再次确认一下是否开启成功

  > gtid_mode 被设置为on，即为开启成功

  ![image-20201110165848669](https://oscimg.oschina.net/oscnet/up-bdee82d26e2c3bc39ac9a858cca71ff62a1.png)

- 备份数据库

  > 若使用的是三个数据库，数据不一样，需要进行数据的备份，导入

  ```
  # 备份
  mysqldump --single-transaction --master-data=2 --triggers --routines --all-databases -uroot -p > all.sql
  # 导出
  mysqldump -u username -p dbname > dbname.sql
  # 导入
  mysqldump -u username -p dbname < dbname.sql
  ```

## 配置账号

- 主库创建账号

  > 创建用于复制的账号

  ```
  mysql> create user repl@'192.168.43.%' identified by '123456Gao!';
  Query OK, 0 rows affected (0.01 sec)
  
  mysql> grant replication slave on *.* to repl@'192.168.43.%';
  Query OK, 0 rows affected (0.06 sec)
  ```

- 从库进行连接

  > 进行数据库连接

  ```
  mysql> change master to master_host = '192.168.43.54',
      -> master_user = 'repl',
      -> master_password = '123456Gao!',
      -> master_auto_position = 1
      -> ;
  Query OK, 0 rows affected, 2 warnings (0.07 sec)
  ```

- 配置前

  ![image-20201110172707416](https://oscimg.oschina.net/oscnet/up-1e1344e9569c656dc41b3d59af807e0ac7b.png)

- 配置后

  ![image-20201110172725502](https://oscimg.oschina.net/oscnet/up-2d062a9b9cfcba4c75d9380ff02074a48dc.png)

- 开启从库复制链路

  ```
  start slave;
  ```

## 查看状态

- show slave status\G

  > 确保下图红框的两个内容状态为yes，

  ![](https://oscimg.oschina.net/oscnet/up-819dcc49c4bc74492635ffe3b294402ed1c.png)

- 测试

  > 在主库创建数据库，然后从库参看是否正确同步的该数据库

  ```
  # 主库创建
  create table test_gtid;
  # 从库查看
  show databases;
  ```

## 可能遇到的问题

- 测试链路

  ```
  mysql -urepl -p123456Gao! -h192.168.43.54
  ```

- [Mysql主从同步时Slave_IO_Running：Connecting ; Slave_SQL_Running：Yes的情况故障排除](https://blog.csdn.net/mbytes/article/details/86711508)