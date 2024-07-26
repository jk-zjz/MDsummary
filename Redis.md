## Redis 安装
环境 Conetos 7
### 下载redis && 解压
```
redis githup仓库
https://github.com/redis/redis/releases

下载
wget https://github.com/redis/redis/archive/refs/tags/7.0.13.tar.gz

tar -zxvf 压缩包

```
// 编译与安装
```
// 移动目录到local
mv /root/redis-5.0.7 /usr/local/redis

cd /usr/local/redis

// 编译
make
// 安装
make PREFIX=/usr/local/redis install

```


启动
```
	
./bin/redis-server& ./redis.conf

// 启动redis 服务端
redis-server --port 8888

// 启动redis客户端
redis-cli -p 8888

```


redis.conf 配置
```
daemonize 守护进程是否启动 Yes/No
注意：如果开启（yes），redis将以服务的形式存在，日志将不再打印到命名窗口中。

port 6… 设定当前端口号
注意：如果使用配置文件启动多个服务，这里一定要记得改端口。

logfile “xxx.log” 设定存放日志的文件。
注意：每个服务都必须对应一个日志文件，这样方便我们查阅。

dir 存放日志“目录的路径”
注意：日志要统一管理。

如果想实现在外部访问服务器中的Redis，除了需要设置 protected-mode no 之外，还需将redis.conf 文件中的 bind 127.0.0.1注释掉。
```

配置防火墙放行端口
```
# 1、开放redis的6379端口【假设redis端口为6379】
firewall-cmd --zone=public --add-port=6379/tcp --permanent

# 2、重启防火墙使得配置生效
systemctl restart firewalld

# 3、查看系统所有开放的端口
firewall-cmd --zone=public --list-ports
```

重启redis
```
# 1、查看redis进程是否存在
ps -ef | grep redis

# 2、关闭redis
# 找到自己redis服务中的redis-cli，
./opt/redis/bin/redis-cli shutdown

#3、启动redis 【加&表示以后台程序方式运行，不加也可以】
./opt/redis/bin/redis-server &
# 使用指定配置文件启动redis
./opt/redis/bin/redis-server /opt/redis/conf/redis.conf
```
测试
```
#1、进入redis服务
./opt/redis/bin/redis-cli -h IP地址 -p 端口
# 通过执行下面的命令，看看是不是都为no，如果不是，就用config set 配置名 属性 改为no。
config get daemonize
config get protected-mode 

```