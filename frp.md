# FRP(内网穿透工具)
## 下载网址
1）下载方式

    github
        https://github.com/fatedier/frp/releases
    Centos
        查看公网架构  arch  x86_64 对应frp_0.38.0_linux_amd64.tar.gz
        wget https://github.com/fatedier/frp/releases/download/v0.57.0/frp_0.57.0_linux_amd64.tar.gz

版本区别  3.7.X  && 5.7.X 区别在配置文件  .ini 和 .toml  
CPU区别  arm && amd

不建议用root安装

## Frpc(客户端  内网)
2）安装（Centos 9 && frp_0.57.0_linux_amd64 为例）

    //解压
    tar -zxvf frp_0.57.0_linux_amd64.tar.gz
    
    cd frp_0.57.0_linux_amd64/
        //解压内容
        frpc  frpc.toml  frps  frps.toml  LICENSE
    sudo vim frpc.toml
3）配置文件 frpc.toml

    # FRP服务端IP
    serverAddr = "103.xxx.xxx.135"
    # FRP服务端Port
    serverPort = 7000
    
    # 设置自定义404页面
    custom404Page = "路径/404.html"
    
    # 鉴权方式：token / oidc
    auth.method = "token"
    # TOKEN值 与服务器相同
    auth.token = "5a138c6aefdb12873e39a21a26e9a4e6"
    
    # 客户端Dashboard 配置
    webServer.addr = "0.0.0.0"
    webServer.port = 7001
    webServer.user = "admin"
    webServer.password = "admin"
    
    [[proxies]]
    name = "web1"
    type = "http"
    localPort = 80
    customDomains = ["test1.example.com"]
    #customDomains = ["test1.example.com","test1.example.cn"] 多个
    
    [[proxies]]
    name = "web2"
    type = "https"
    localPort = 443
    customDomains = ["test2.example.com"]

    [[proxies]]
    name = "ssh"
    type = "tcp"
    localIP = "127.0.0.1"
    localPort = 22
    remotePort = 6000
    #ssh 连接 103.xxx.xxx.135:6000
4）创建客户端frpc.service文件
        
    [Unit]
    Description=frpc service
    After=network.target
    
    [Service]
    Type=simple
    ExecStart=自己的frp解压目录/frpc -c 自己的frp解压目录/frpc.toml
    Restart=on-failure
    RestartSec=5s
    
    [Install]
    WantedBy=multi-user.target

5）启动

    前台启动
    ./frpc -c ./frpc.toml
    
    后台启动
    ./frpc -c ./frpc.toml &

    使用systemd 管理
        # 使用 yum 安装 systemd（CentOS/RHEL）
        yum install systemd 
        # 使用 apt 安装 systemd（Debian/Ubuntu）
        apt install systemd

    # 启动frp
    sudo systemctl start frpc
    # 停止frp
    sudo systemctl stop frpc
    # 重启frp
    sudo systemctl restart frpc
    # 查看frp状态
    sudo systemctl status frpc

### `注：  sudo systemctl start frpc 启动失败`

    检查SELinux状态：
    如果您的系统启用了SELinux，那么可能是SELinux策略阻止了frpc的执行。
    您可以临时将SELinux设置为宽容模式来测试是否是这个原因：

    setenforce 0


## Frps(服务器  外网)
2）安装（Centos 9 && frp_0.57.0_linux_amd64 为例）

    //解压
    tar -zxvf frp_0.57.0_linux_amd64.tar.gz
    
        cd frp_0.57.0_linux_amd64/
            //解压内容
            frpc  frpc.toml  frps  frps.toml  LICENSE
        sudo vim frpc.toml
3）配置文件 frps.toml

    # 服务端口
    bindPort = 7000
    
    # 鉴权方式：token / oidc
    auth.method = "token"
    # TOKEN值（与客户端配置一致即可）
    auth.token = "5a138c6aefdb12873e39a21a26e9a4e6"
    
    # HTTP 类型代理
    vhostHTTPPort = 80
    # HTTPS 类型代理
    vhostHTTPSPort = 443
    
    # Dashboard 配置
    webServer.addr = "0.0.0.0"
    webServer.port = 7001
    webServer.user = "admin"
    webServer.password = "admin"
4）创建客户端frps.service文件

    [Unit]
    # 服务名称，可自定义
    Description = frps server
    After = network.target syslog.target
    Wants = network.target
    
    [Service]
    Type = simple
    # 启动frps的命令，需修改为您的frps的安装路径
    ExecStart = 自己的frp解压目录/frps -c 自己的frp解压目录/frps.toml
    
    [Install]
    WantedBy = multi-user.target

5）启动

    前台启动
    ./frps -c ./frps.toml
    
    后台启动
    ./frps -c ./frps.toml &

    使用systemd 管理
        # 使用 yum 安装 systemd（CentOS/RHEL）
        yum install systemd 
        # 使用 apt 安装 systemd（Debian/Ubuntu）
        apt install systemd

    # 启动frp
    sudo systemctl start frps
    # 停止frp
    sudo systemctl stop frps
    # 重启frp
    sudo systemctl restart frps
    # 查看frp状态
    sudo systemctl status frps

## Web 页面服务 + nginx
1）下载与安装(以 nginx-1.20.2.tar.gz 为例)
    
    #nginx 下载地址
    https://nginx.org/en/download.html
    tar -zxvf nginx-1.20.2.tar.gz
    #安装nginx依赖
    sudo yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel

    cd nginx-1.20.2
    # 执行configure脚本，设置安装nginx的初始化配置（--with-http_ssl_module：启动 SSL 的支持），生成 Makefile 可编译文件
    ./configure --with-http_ssl_module
    make
    sudo make install
    #默认安装路径  /usr/local/nginx

2)配置 nginx.conf文件
    
    1.在服务端添加
        # HTTP 类型代理
        vhostHTTPPort = 80
        # HTTPS 类型代理
        vhostHTTPSPort = 443
    sudo systemctl restart frps
    
    2.在客户端添加
        [[proxies]]
        name = "web1"
        type = "http"
        localPort = 80
        customDomains = ["test1.example.com"]
        
        [[proxies]]
        name = "web2"
        type = "https"
        localPort = 443
        customDomains = ["test2.example.com"]
    sudo systemctl restart frpc
    
    3.新增 nginx 配置（HTTP配置）
        server {
            #HTTP 默认访问端口号为 80
            listen 80; 
            #请填写绑定证书的域名
            server_name test1.example.com; 
            
            location / {
                #root 是你需要访问页面的根目录
                root   /opt/test/web1;
                try_files $uri $uri/ /index.html;
                index  index.html index.htm;
            }
        }
    4.新增 nginx 配置（HTTPS配置
        server {
            #HTTPS默认访问端口号为 443
            listen 443 ssl; 
            #请填写绑定证书的域名
            server_name test2.example.com; 
            #请填写证书文件的相对路径或绝对路径
            ssl_certificate /etc/nginx/conf.d/cert/test2.example.com.crt; 
            #请填写私钥文件的相对路径或绝对路径
            ssl_certificate_key /etc/nginx/conf.d/cert/test2.example.com.key; 
            ssl_session_timeout 5m;
            #请按照以下协议配置
            ssl_protocols TLSv1.2 TLSv1.3; 
            #请按照以下套件配置，配置加密套件，写法遵循 openssl 标准。
            ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE; 
            ssl_prefer_server_ciphers on;
        
            location / {
                root   /opt/test/web2;
                try_files $uri $uri/ /index.html;
                index  index.html index.htm;
            }
        }

    5.校验与重启
        # 校验nginx配置
        sudo nginx -t
        # 重载nginx配置
        sudo nginx -s reload

    
    

    



