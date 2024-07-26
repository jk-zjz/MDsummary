## mysql 安装

### 检测mysql
```
rpm -qa|grep mysql
rpm -qa|grep mariadb

如果存在进行删除

rpm -e --nodeps mysql-libs-5.1.73-1.el6.x86_64
rpm -e --nodeps mariadb-libs-5.5.56-2.el7.x86_64
```


### mysql 8.0
安装环境 centos 7

下载包
```
wget https://repo.mysql.com/mysql80-community-release-el7.rpm
```
安装源
```
yum -y install mysql80-community-release-el7.rpm
```
安装mysql服务
```
yum -y install mysql-community-server  

sudo yum install mysql-devel
```
设置mysql 配置
```
vim /etc/my.cnf

不区分大小写
lower_case_table_names=1

字符集
character-set-server=utf8

```
启动与登录
```
启动mysql 自动初始化
sudo systemctl start mysql
或
systemctl start mysqld.service

获取初始密码 /val/log/mysqld.log
sudo grep 'temporary password' /var/log/mysqld.log

```
登录与更改密码配置外部访问
```
登录
mysql  -u  root  -p

修改初始密码
ALTER USER 'root'@'localhost' IDENTIFIED BY '486466132Zyr.';

设置外部访问
use  mysql

update user set host = '%' where user = 'root';

flush privileges;
```

### mysql 5.7
安装环境 centos 7

设置权限与获取依赖包
```
权限设置
chmod -R 777 /tmp

检测依赖
rpm -qa|grep libaio
rpm -qa|grep net-tools

不存在安装
yum -y install libaio net-tools
```

下载安装包与解压
```
下载安装包
wget http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.16-1.el7.x86_64.rpm-bundle.tar
解压
tar -xvf mysql-5.7.16-1.el7.x86_64.rpm-bundle.tar
```

安装与检测
```
rpm -ivh mysql-community-common-5.7.16-1.el7.x86_64.rpm 
rpm -ivh mysql-community-libs-5.7.16-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-5.7.16-1.el7.x86_64.rpm 
rpm -ivh mysql-community-server-5.7.16-1.el7.x86_64.rpm
检测
mysqladmin –version 
```


初始化与默认密码
```
初始化
mysqld --initialize --user=mysql
获取默认密码
cat /var/log/mysqld.log | tail -n 10
```

启动与改个密码
```
启动初始化
systemctl start mysqld.service
更改密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```

更改字符集
```
character_set_server=utf8
init_connect=’SET NAMES utf8’
```

### mysql 8.0
安装环境 centos 9

配置yum
```
wget https://repo.mysql.com/mysql80-community-release-el9-1.noarch.rpm
sudo rpm -ivh mysql80-community-release-el9-1.noarch.rpm
```
禁用本地库
```
sudo dnf config-manager --disable mysql57-community
sudo dnf config-manager --disable mysql56-community
sudo dnf config-manager --enable mysql80-community
```
下载与导入密钥
```
wget https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
sudo rpm --import RPM-GPG-KEY-mysql-2022
```
手动下载mysql与安装
```
wget https://repo.mysql.com/yum/mysql-8.0-community/el/9/x86_64/mysql-community-client-8.0.37-1.el9.x86_64.rpm
wget https://repo.mysql.com/yum/mysql-8.0-community/el/9/x86_64/mysql-community-client-plugins-8.0.37-1.el9.x86_64.rpm
wget https://repo.mysql.com/yum/mysql-8.0-community/el/9/x86_64/mysql-community-common-8.0.37-1.el9.x86_64.rpm
wget https://repo.mysql.com/yum/mysql-8.0-community/el/9/x86_64/mysql-community-icu-data-files-8.0.37-1.el9.x86_64.rpm
wget https://repo.mysql.com/yum/mysql-8.0-community/el/9/x86_64/mysql-community-libs-8.0.37-1.el9.x86_64.rpm
wget https://repo.mysql.com/yum/mysql-8.0-community/el/9/x86_64/mysql-community-server-8.0.37-1.el9.x86_64.rpm

sudo rpm -ivh mysql-community-client-*.rpm mysql-community-client-plugins-*.rpm mysql-community-common-*.rpm mysql-community-icu-data-files-*.rpm mysql-community-libs-*.rpm mysql-community-server-*.rpm

安装
sudo dnf install mysql-community-server --nogpgcheck

```
配置与自启动
```
sudo systemctl start mysqld
sudo systemctl enable mysqld
```
