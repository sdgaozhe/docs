## 环境准备

> 在安装MySQL之前，我们需要安装一下MySQL的依赖。

### 更新yum源

- yum update

### 下载MySQL

- wget https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm

MySQL repo安装

- rpm -ivh mysql57-community-release-el7-9.noarch.rpm

## 正式安装

### 安装命令

- yum install mysql-server

### 启动命令

- systemctl start mysqld 

### 查看并修改初始密码

> MySQL安装后会生成一个初始密码，此时我们查看并将其记录下来，用来第一次登陆MySQL

- grep 'temporary password' /var/log/mysqld.log

![](https://oscimg.oschina.net/oscnet/up-0fb341495fbf5555ab272b2ef5071f243be.png)

### 登陆MySQL

> 登陆后依次执行下面的命令，进行初始设置

- mysql -u root -p
- set global validate_password_policy=LOW;
- set global validate_password_length=6;
- ALTER USER 'root'@'localhost' IDENTIFIED BY '416798';

## 权限配置

> MySQL安装成功后要做好相应的权限控制，如果需要远程访问，不要对MySQL配置任意IP段的访问，否则会带来一定的安全隐患

### 查看可以登陆MySQL的用户

![](https://oscimg.oschina.net/oscnet/up-7243e1654bb2ff255c3f91260667f6390c3.png)

### 指定IP连接MySQL

- 可以单独指定IP进行访问，并可以单独设置密码

  `grant all privileges on *.* to 'root'@'172.17.0.4' identified by '416798';`

- 对IP段进行鉴权

  `grant all privileges on *.* to 'root'@'172.17.0.*' identified by '416798';`

- 仅对test数据库进行访问

  `grant all privileges on test.* to 'root'@'172.17.0.*' identified by '416798';`

- 仅可访问test数据库的temp表

  `grant all privileges on test.temp to 'root'@'172.17.0.*' identified by '416798';`

- 鉴权删除

  `drop user 'root'@'172.17.0.4';`

## 其他常用设置

- systemctl stop mysqld  # 关闭MySQL
- systemctl restart mysqld # 重启MySQL
- systemctl status mysqld  # mysql运行状态
- systemctl enable mysqld  # mysql开机自启动
- select version(); # 查看MySQL版本

## 参考文档

- [MySQL 安装](https://www.runoob.com/mysql/mysql-install.html)