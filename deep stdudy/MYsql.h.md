````
/ / Mysq1操作句柄初始化

```
MYSQL *mysq1_init(MYSQL *mysq1);
```

//参数为空则动态申请句柄空间进行初始化//失败返回NULL

//连接mysq1服务器
MYSQL*mysql_real_connect(MYsQL*mysq1，const char *host，const char *user,
						const char *passwd ,const char *db，unsigned int port,
						const char *unix_socket，unsigned 1ong client_f1ag);
/ /mysql--初始化完成的句柄
/ /host---连接的mysq1服务器的地址
/ /user---连接的服务器的用户名
/ /passwd-连接的服务器的密码
/ /db ----默认选择的数据库名称

/ /unix_socket---通信管道文件或者socket文件，通常置NULL
/ /client_flag---客户端标志位，通常置
````

