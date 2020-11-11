## 后台运行

> python的输出有缓冲，导致日志文件并不能够马上看到输出

- nohup python3 -u xxxx.py > nohup.out & # -u参数，使得python不启用缓冲


## 批量移动/删除

> 可以解决移动文件数目过大的问题（Argument list too long）

- find test/ -name "*.jpg" -exec cp {} train \;
- find test/ -name "*.jpg" | xargs -i rm {}

## 查看端口

> 一般管道符grep和netstat一块使用

- netstat -ntlp # 简化
- netstat -lnp | grep 88 # 详细
- netstat -anp # 可以看到进程编号信息
- firewall-cmd --query-port=666/tcp  # 查看端口是否开启成功 成功返回yes

## firewalld的基本使用

> 真实的业务环境，必须开启防火墙

- 启动： systemctl start firewalld
- 关闭： systemctl stop firewalld
- 查看状态： systemctl status firewalld 
- 开机禁用  ： systemctl disable firewalld
- 开机启用  ： systemctl enable firewalld
- 查看一个服务：systemctl status  jenkins.service 

## 开启一个端口

> 线上环境仅对需要的端口开启，非需要的端口应关闭

- 添加：firewall-cmd --zone=public --add-port=80/tcp --permanent    （--permanent永久生效，没有此参数重启后失效）
- 重新载入：firewall-cmd --reload
- 查看：firewall-cmd --zone=public --query-port=80/tcp
- 删除：firewall-cmd --zone=public --remove-port=80/tcp --permanent

## 自启动

> 将必须的脚本进行自启动，避免过多的脚本自启动

- /etc/rc.d/init.d # 存放着自启动脚本 ，优先级比下面的高
- /lib/systemd/system/  # 存放着自启动脚本
- /etc/rc.local  # 直接加入即可

## 日志过滤

> 在定位log文件的时候cat和grep命令发挥着重要的作用

- cat -n test.log | grep "error" | more  #  more 分页查询

## crontab

> , 和数组类似，指定数字，/ 每过多少个数字，- 从X到Z，* 取值范围内的所有数字

- 每五分钟执行      */5 * * * *
- 每小时执行        0 * * * *
- 每天执行          0 0 * * *
- 每周执行          0 0 * * 0
- 每月执行          0 0 1 * *
- 每年执行          0 0 1 1 *


## curl

> curl命令具有很大的效力，我们可以直接对我们接口进行测试pi

- curl -i www.baidu.com  # 获取全部response信息
- curl -I www.baidu.com # 只返回response的头信息
- curl url -X POST -H "Content-Type:application/json" -d '{"parameterName1":"parameterValue1","parameterName2":"parameterValue2"}' # 向接口发送json数据
- curl url -X POST -d "parameterName1=parameterValue1&parameterName2=parameterValue2" # 向接口发送表单数据
- curl url -F "file=@/Users/local/imgs/my.png"  -v # 向接口发送文件


## vim insert VISUAL模式

- 普通模式下输入“:set mouse-=a”，修改为非鼠标模式

## 权限

> 相关权限我在下图作了说明

[![20180412134052927.png](https://i.loli.net/2019/06/17/5d076ca6de6d214993.png)]()

- `r`可读，`w`可写，`x`可执行
- `+` 表示增加权限，如u+x, u+r, u+w, g+w, g+r, o+r， a+r等
- `-` 表示取消权限，如u-x, u-r, u-w, g-w, g-r, o-r， a-r等
- `=` 表示赋予给定权限，并取消其他所有权限（如果有的话，如原来u是rwx，设置u=r，u就剩r）
- chown # 对文件夹或文件的所属权限变更
- chmod # 直接变更

## 远程拷贝

> scp命令有很重要的作用，应掌握

- scp local_file remote_username@remote_ip:remote_folder # 对文件进行拷贝
- scp -r local_folder remote_username@remote_ip:remote_folder # 对目录 进行拷贝

## 管道和数量

> 统计数量，可以统计管道过滤的数量和文件的行数

- wc -l  

## tail/head(语法一致)

> 会把文件里的最尾部的内容显示在屏幕上，并且不断刷新

- tail -f notes.log
- tail  -n  10  test.log

## 添加环境变量

> 有的时候我们需要向系统中手动的添加变量信息


- export PATH=$PATH:/usr/local/webserver/php/bin   # 临时添加
- 永久添加

```
vi /etc/profile  
PATH=$PATH:/usr/local/webserver/php/bin:/usr/local/webserver/mysql/bin
export PATH
source /etc/profile  # 编译一下
```

- 查看添加的变量

```
echo $PATH
```

## 后台运行

> 在真实业务中，我们常常会用到后台运行的相关命令

```
nohup commond & 表示Ctrl+C也不会使其中断
&  表示任务在后台执行，如要在后台运行redis-server,则有  redis-server &
&& 表示前一条命令执行成功时，才执行后一条命令 ，如 echo '1‘ && echo '2'    
| 表示管道，上一条命令的输出，作为下一条命令参数，如 echo 'yes' | wc -l
|| 表示上一条命令执行失败后，才执行下一条命令，如 cat nofile || echo "fail"
```
## 管理员

> 该命令在线上一般用不到，但是在我们平常使用虚拟机的时候会用到

```
su username # 切换用户
su  # 输入root账户的密码后切换到root身份，无时间限制
sudo su # 效果同su，只是不需要root的密码，而需要当前用户的密码
```

## 查看系统信息

> 在安装一些机器学习或其他软件的时候，我们需要确认操作系统的一些信息

```
arch  # 用于centos查看32位还是64位
dpkg  #用于查看 Debian/ Ubuntu 操作系统是 32 位还是 64 位
cat /etc/issue  # 查看系统架构   # centos/Debian/Ubuntu
getconf LONG_BIT # 直接返回操作系统是32还是64
file /lib/systemd/systemd # 输出详细的具体信息
uname -r # 查看操作系统内核
```

## 安装

> linux常用的两个分支，乌班图和centos的基础安装命令

```
apt install package   # 乌班图
yum install  package  # centos
```
## 下载

> wget远程下载文件

```
wget  # 直接下载
wget -b # 后台下载
```
## netstat命令

> 查看操作系统开放的端口和进程信息

```
netstat -ntlp # 仅显示端口
netstat -anp # 查看防火墙端口
```

## ps/kill

> 一般先用ps查看port信息，kill进行杀掉

```
ps -A # 显示进程信息
ps -u root # 显示root进程用户信息
kill -9 port # 中断进程进行退出
kill port # 强制退出
```

## 运行级别

> linux的7种运行级别，应掌握

```
# 0 - 停机（千万别把initdefault设置为0，否则系统永远无法启动）
# 1 - 单用户模式
# 2 - 多用户，没有 NFS
# 3 - 完全多用户模式(标准的运行级)
# 4 – 系统保留的
# 5 - X11 （x window)
# 6 - 重新启动 （千万不要把initdefault 设置为6，否则将一直在重启 ）
```

## 重定向

> 一个>表示覆盖写，两个>>表示追加写

- ls -l > list.txt  # ls -l 的结果保存在了list.txt文件中

## 开机/关机

> 如果是通过shutdown命令设置关机的话，可以用shutdown -c命令取消重启

### 重启命令

```
1、reboot  / init6
2、shutdown -r now 立刻重启(root用户使用)
3、shutdown -r 10 过10分钟自动重启(root用户使用) 
4、shutdown -r 20:35 在时间为20:35时候重启(root用户使用)
```
### 关机命令
```
1、halt / init 0 立刻关机  
2、poweroff  立刻关机
3、shutdown -h now 立刻关机(root用户使用)
4、shutdown -h 10 10分钟后自动关机

```

## 返回

> 需要注意的是linux只能进行 ../../两级返回

- ./  #指在当前目录
- ../ #指返回上一级目录

## 管道/查看进程

> 一般都是组合使用

- grep a *.txt  # 模糊查询
- ps -ef | grep mysql  # 组合使用-->查看进程

## 进入

> 注意 cd - 的使用，很优雅

- cd -  # 返回上次的工作目录
- cd ~ #进入当前用户
- cd / #进入根目录

## 磁盘

> 查看系统的磁盘占用信息

- df -kh  #查看磁盘大小
- free # 查看磁盘占用

## 目录

> 查看linux目录树信息，其中tree需要单独安装

```
pwd   #显示当前目录
dirs #显示当前目录
tree  # 目录树
.
├── jjjj
│   └── j.txt
└── kkk
```
## 查看

> linux操作系统 . 开头的文件夹默认是不显示的，使用ls -a可以查看

```
ls -a # 显示隐藏
ls -l  # 单列格式输出详细信息，简写ll
cat  # 查看文件内容的全部
tail/head # 查看指定的行
tail -f filename # 不断刷新读取新内容
```
## 软连接

> 在执行ln命令之前，目录/usr/liu中不存在a2.c文件。执行ln之后，在/usr/liu目录中才有a2.c这一项，表明m2.c和a2.c链接起来（注意，二者在物理上是同一文件），利用ls -l命令可以看到链接数的变化。

```
ln /mub1/m2.c /usr/liu/a2.c  #将目录/usr/mengqc/mub1下的文件m2.c链接到目录/usr/liu下的文件a2.c
ln -s Lte.V120 Lte  #  迭代版本在前
ln -snf # 修改软连接  
ln -s /usr/local/python3/bin/python3.6 /usr/bin/python3
```

## 复制

> 一般做备份的时候用的多

```
cp # 只能移动文件
cp -r #包括文件夹一块移动
```
## 移动/改名

> 注意通配符的应用

```
mv ex3 new1 #将文件ex3改名为new1
mv /lianxi/kkk/* /lianxi/jjjj/  #移动文件
```

## 解压/打包

> 一般在进行二进制安装软件的时候用的多

```
tar -zxvf XXX.tar.gz
tar -zcvf 包名 将要打包文件  #打包
tar -jxvf XXX.tar.bz2 
tar -ztvf 包名  #查看包中的文件
```

## 删除

> rm -rf 应慎用

```
rm -r #可以删除文件夹
rm -rf #强制删除
```
## 文件/文件夹

> 注意-p的参数的使用，可以大大提交效率

```
mkdir filefolder #建立空白文件夹
mkdir -p filefolder # 级联创建
rmdir #删除空白文件夹
touch  filename # 创建文件
```
## 搜索

> 对文件进行搜索应掌握，很有用

```
whereis #搜索程序名称
whereis -b #搜索二进制文件
whereis -m #搜索说明文件
whereis -s #搜索源代码

find . #列出当前目录及子目录下所有文件和文件夹
find  / -name "*.k"  # 在根目录下搜索后缀为.k的文件

which
which python
/usr/bin/python
```

## 释放swap

> 该命令一般用不到，知道有这个命令即可

```
swapon -s 查看到swap分区挂载在哪儿
swapoff  /dev/sda2  #停止/释放
swapon -a  #再次开启
```

## dos2unix

> 该命令做物联网开发的小伙伴可能会遇到

- dos2unix  windowsfile  # Windows格式文本转换为Unix&Linux格式文件

## 其他问题

- 出现^H，使用Ctrl+回车即可