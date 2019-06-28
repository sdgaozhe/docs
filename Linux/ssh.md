> ssh登录出现 Host key verification failed. 

- 第一种方法

```
cd ~/.ssh/
rm -rf known_hosts
```

- 第二种方法

```
cd ~/.ssh/
vi known_hosts
```