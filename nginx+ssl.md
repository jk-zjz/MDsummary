## 安装 Nginx 与 配置 SSL证书

### 安装Nginx


Nginx 相关依赖
```
yum install -y gcc-c++

yum install -y pcre pcre-devel

yum install -y zlib zlib-devel

yum install -y openssl openssl-devel
```

下载Nginx 
```
nginx的下载网址
https://nginx.org/en/download.html

使用
tar -zxvf 解压压缩包


加入SSL模块
./configure --with-http_ssl_module

编译
make

如果你已经安装了nginx 
备份/usr/local/nginx/sbin/中的nginx
拷贝 objs中的nginx到  /usr/local/nginx/sbin/

安装

make install
```

```
申请到SSL 证书 下载nginx 到 /usr/local/nginx/ssl 目录
解压
一个  .pem 和 .key

修改配置文件 /usr/local/nginx/conf/nginx.cofig
```

```config

server {
    #配置 433 端口使用 ssl
    listen 443 ssl;
    server_name yourdomain.com; #需要将yourdomain.com替换成证书绑定的域名。
    root html;
    index index.html;
    ssl_certificate /usr/local/nginx/ssl/.pem;  #需要将.pem替换成已上传的证书文件的名称。
    ssl_certificate_key /usr/local/nginx/ssl.key; #需要将cert-file-name.key替换成已上传的证书密钥文件的名称。
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    #表示使用的加密套件的类型。
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #表示使用的TLS协议的类型。
    ssl_prefer_server_ciphers on;
    location / {
        root html;  #站点目录。
        index index.html index.htm;
    }
}



```
Nginx常用命令
```
./nginx 启动
./nginx -t 检测启动
./nginx -s stop 停止nginx
./nginx -s reload 重启nginx
./nginx -s quit  此方式是待nginx进程处理完任务后，再停止nginx。
./nginx -c /usr/local/nginx/conf/nginx.conf 指定配置文件启动
```


## http 跳转 https
### 方法一：rewrite
```
server {
        listen 80;	#监听ipv4
        listen [::]:80;	#监听ipv6
        server_name xxx.com www.xxx.com;	#虚拟主机域名
        rewrite ^(.*)$ https://$host$1 permanent;	#rewrite跳转
}

server {
        listen 443 ssl;
        listen [::]:443 ssl;
        
        server_name xxx.com www.xxx.com;
        root /var/www/xxx;

        index index.html index.htm index.nginx-debian.html;

        location / {
                try_files $uri $uri/ =404;
        }
}

```
### 方法二：return 301
```
server {
        listen 80;	#监听ipv4
        listen [::]:80;	#监听ipv6
        server_name xxx.com www.xxx.com;	#虚拟主机域名
        return 301 https://$server_name$request_uri;	#return 301
}

server {
        listen 443 ssl;
        listen [::]:443 ssl;
        
        server_name xxx.com www.xxx.com;
        root /var/www/xxx;

        index index.html index.htm index.nginx-debian.html;

        location / {
                try_files $uri $uri/ =404;
        }
}
```