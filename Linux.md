# Linux 
1）Centos 优化

    //修改主机名
    hostnamectl set-hostname hh
    //修改显示框颜色
    vim /etc/bashrc
    修改  PS1
    PS1="\e[1;32m\u\e[m\e[1;33m@\e[m\e[1;35m\h\e[m:\w$"
    source /etc/bash

2）排查重要文件  
网卡文件
    
    Centos7  静态配置文件方式由路径下管理
    /etc/sysconfig/network-scripts/ 
        TYPE=Ethernet 
        PROXY_METHOD=none
        BROWSER_ONLY=no
        BOOTPROTO=dhcp #自动获取
        DEFROUTE=yes
        IPV4_FAILURE_FATAL=no
        IPV6INIT=yes
        IPV6_AUTOCONF=yes
        IPV6_DEFROUTE=yes
        IPV6_FAILURE_FATAL=no
        IPV6_ADDR_GEN_MODE=stable-privacy
        NAME=$'\751\605\615\747\675\656 1'
        UUID=8a9dd9cf-e0b0-4791-a1a0-65469a544a0f
        ONBOOT=yes #开机自启动

    Centos9 已经被 NetwrokManager 管理
    查询状态
    systemctl status NetworkManager
    查询所有NetworkManager 网络接口状态
    nmcli device status
    查看网卡信息
    nmcli device show 网卡名
    修改网卡信息
    nmcli connection edit 网卡名
    网卡文件地址
    /etc/NetworkManager/system-connection/

挂载文件

    /etc/fstab
    file system table 系统文件挂载表格

    /dev/mapper/centos-root /                       xfs     defaults        0 0
    UUID=98b94fc4-993c-4da3-8e83-576856cdbd94 /boot                   xfs     defaults        0 0
    /dev/mapper/centos-swap swap                    swap    defaults        0 0

    UUID=98b94fc4-993c-4da3-8e83-576856cdbd94：这是 /boot 分区的 UUID (Universally Unique Identifier)。UUID 用于唯一标识一个文件系统或分区。
    /boot：这是挂载点，即 /boot 目录。
    xfs：文件系统类型。
    defaults：挂载选项。
    0：不备份。
    0：不进行文件系统检查。
主机名文件
    
    更改主机名
    vim /etc/hostnaem

    命令更改
    hostnamectl set-hostname 修改的名称

    需重新连接
本地域名解析

    vim /etc/hosts
修改本地域名解析

     vim /etc/resolv.conf
全局环境变量

    vim /etc/profile
shell 

    //查看shell版本
    cat /etc/shells
    //全局bash的环境变量
    vim /etc/bashrc
查看CentOS 以及版本

    cat redhat-release
        CentOS Linux release 7.9.2009 (Core)
        CentOS Stream release 9
系统启动的命名和初始化

    cd  /etc/init.d/
    //运行级别
    vim /etc/inittab
权限以及用户密码存放地
    
    //root 权限组
    vim /etc/sudoers
    //密码加密存放地
    vim /etc/shadow
通过shall命令获取到
root:$6$nIf/MxOOfdbLF/8C$zLaB9vK7MKSx2/jR9S344FYPN8mpOntUyKy0ErdOLY.aKcFDY8CulZto0IKujNkUI39yAz8JN.y5J4U9VX8Hl.::0:99999:7:::  
root： 后面的值
    
    grep '^root:' /etc/shadow | cut -d: -f2
        ^root 和root 的区别在于^就会显示第一个root，而root会查找所有
        cut -d 是以 : 为分隔符
        -f2 是分割的第一个单词
            $6$nIf/MxOOfdbLF/8C$zLaB9vK7MKSx2/jR9S344FYPN8mpOntUyKy0ErdOLY.aKcFDY8CulZto0IKujNkUI39yAz8JN.y5J4U9VX8Hl.
区局环境与用户环境

    vim /etc/bashrc
            -------全局环境变量
    vim /etc/profile

    vim ~/.bashrc
            -------用户环境变量
    vim ~/.bash_profile
yum源位置  包管理

    cd /etc/yum.repos.d/
程序大多安装目录 & 源码

    正常程序文件
    cd /usr/local/
    cd /opt/
        相当于wids的 program files
    源码文件
    cd /usr/src/
内核配置文件

    vim /etc/sysctl.conf

    安全性和网络性能调优：通过sysctl，你可以开启或关闭某些安全特性，如IP转发、ICMP阻塞、SYN Cookies等。此外，你还可以调整网络性能相关的参数，比如TCP超时时间、连接数限制等。
    内存管理和文件系统优化：sysctl允许你设置虚拟内存、文件系统的缓存策略、页缓存等参数，以提升系统的性能和稳定性。
    系统调优：通过对系统调度程序、进程管理、网络协议等方面的优化，sysctl可以帮助你适应特定的应用需求。
用户信息&用户组信息

    //用户组信息
    vim /etc/group
    //用户信息
    vim /etc/passwd
系统日志文件

    cd /var/log/
        message 系统文件日志 程序的启动与运行 
        secure 安装日志文件  更改密码&ssh连接
        dmesg 硬件系统日志
            在CentOS9没有这个文件，可能在其他地方
登录信息的二进制文件

    //用户登录信息记录
    vim /var/log/wtmp
    //近期登录信息
    vim /var/log/lastlog
/porc Linux 系统中的一个重要文件，内核信息和进程方面的信息
    
    //upu信息
    /proc/cpuinfo
    
    //内存信息
    /proc/meminfo

    //平均载入文件
    /proc/loadavg

    挂载列表
    /proc/mounts
/sbin和/bin 的区别

    一般在带s的是指系统级的
    /sbin是系统命令
    /bin是用户命令
/boot 保存的就是一些启动项
/etc/下的cron* 通常是定时任务

    cron.d/
    这是一个目录，通常用于存放系统管理员添加的额外的cron作业配置文件。这些文件通常包含需要定时执行的命令。

    cron.daily/
    这是一个目录，包含每天需要执行的脚本或命令。cron守护进程会定期（通常是每天）检查并执行这个目录中的脚本。

    cron.deny
    这是一个文件，用于列出不允许使用cron的用户。但在许多现代系统上，这个文件可能不再使用，而是使用/etc/cron.allow来明确允许哪些用户使用cron。

    cron.hourly/
    这是一个目录，包含每小时需要执行的脚本或命令。cron守护进程会每小时检查并执行这个目录中的脚本。

    cron.monthly/
    这是一个目录，包含每月需要执行一次的脚本或命令。cron守护进程会每月检查并执行这个目录中的脚本。

    crontab
    这是一个文件，通常用于查看当前用户的cron作业。你可以通过crontab -e编辑当前用户的cron作业，这些作业会存储在这个文件中（尽管不是直接在这个文件中编辑，而是通过一个接口）。

    cron.weekly/
    这是一个目录，包含每周需要执行一次的脚本或命令。cron守护进程会每周检查并执行这个目录中的脚本。

全局文本的替换 red 命令

    # 在输出或打印中，替换字符串。并不改变原文件内容
    sed '作用范围s/替换查找目标/替换成为/替换目标option' 文件名

    # 替换字符串，并更改原文件内容
    # 在sed后面加 -i,即编辑文档“edit files in place”选项
    sed -i '作用范围s/替换查找目标/替换成为/替换目标option' 文件名

    # + -i 是编辑模式 不加是打印出来
    sed -i s/cat/log/g test.txt
    使用编辑模式吧cat全部替换为log替换全局的，tesr.txt文本中的内容
