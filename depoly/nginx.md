> Nginx: [error] open() ＂/usr/local/Nginx/logs/Nginx.pid" failed（2:No such file or directory）

```
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf

```

> nginx: [emerg] unknown directive "ssl"

- 在Nginx安装目录下执行

```
0、请检查是否安装了OpenSSL，若为安装执行 yum -y install openssl openssl-devel
1、./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module # 加载
2、make # 编译
3、cp /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.bak  # 备份
4、cp objs/nginx /usr/local/nginx/sbin/nginx # 拷贝新编译的nginx
5、/usr/local/nginx/sbin/nginx -t  # 测试 nginx.conf 是否配置正确
6、/usr/local/nginx/sbin/nginx -s reload # 重启服务
```