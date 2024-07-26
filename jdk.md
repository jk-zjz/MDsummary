## JDK 下载安装
[官网](https://www.oracle.com/cn/java/technologies/)
```
下载
wget https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.tar.gz

解压
tar -zxvf jdk-21_linux-x64_bin.tar.gz

配置环境变量

/usr/bin/java/jdk-21.0.4

在末尾插入
export JAVA_HOME=/usr/bin/java/jdk-21.0.4
export PATH=$PATH:$JAVA_HOME/bin

刷新配置文件
source /etc/profile

```