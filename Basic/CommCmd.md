<!-- TOC -->

- [基本操作](#基本操作)
  - [启动级别](#启动级别)
  - [本机WMware连接方式](#本机wmware连接方式)
  - [显示各类信息](#显示各类信息)
  - [chmod](#chmod)
  - [Linux文件的复制、删除和移动命令](#linux文件的复制删除和移动命令)
    - [cp复制](#cp复制)
    - [mv移动](#mv移动)
    - [rm删除](#rm删除)
  - [文件拷贝](#文件拷贝)
    - [利用putty相关工具](#利用putty相关工具)
    - [secureCRT或者xshell中使用命令rz/sz](#securecrt或者xshell中使用命令rzsz)
    - [FTP](#ftp)
  - [软件安装](#软件安装)
    - [rpm](#rpm)
    - [yum](#yum)
    - [修改yum源为阿里镜像](#修改yum源为阿里镜像)
  - [chkconfig系统服务](#chkconfig系统服务)
  - [iptables防火墙](#iptables防火墙)

<!-- /TOC -->

<a id="markdown-基本操作" name="基本操作"></a>
# 基本操作

<a id="markdown-启动级别" name="启动级别"></a>
## 启动级别
Linux下是使用0到6来表示不同的启动模式的，每个数字代表的含义如下：

```shell
0 ：系统正常启动然后关机 
1 ：单用户模式 
2 ：多用户模式，没有NFS 
3 ：多用户模式，有NFS 
4 ：系统为使用，留个用户
5 ：图形界面模式 
6 ：系统正常启动然后重新启动 
```

1.root用户登录；
2.打开/etc/inittab 文件
`vi /etc/inittab`
3.在默认的 run level 设置中,可以看到第一行书写如:id:5:initdefault:(默认的 run level 等级为 5,即图形界面)，将5修改为3重启即可；

* 运行时

```shell
#切换到图形
init 5
#切换到命令行
init 3
```

<a id="markdown-本机wmware连接方式" name="本机wmware连接方式"></a>
## 本机WMware连接方式

设置NAT方式连接网络，配置虚拟机网络ip和物理机虚拟网卡ip在同网段可通信。

<a id="markdown-显示各类信息" name="显示各类信息"></a>
## 显示各类信息

```shell
#显示简略信息
ls

#显示权限
ls -l = ls -lrt = ll

#显示权限包含隐藏文件
ls -la

#显示路径
pwd

#CentOS常用到的查看系统命令
uname -a

# 查看内核/操作系统/CPU信息
head -n 1 /etc/issue   # 查看操作系统版本
cat /proc/cpuinfo      # 查看CPU信息
hostname               # 查看计算机名
lspci -tv              # 列出所有PCI设备
lsusb -tv              # 列出所有USB设备
lsmod                  # 列出加载的内核模块
env                    # 查看环境变量 

#资源
free -m                # 查看内存使用量和交换区使用量
df -h                  # 查看各分区使用情况
du -sh <目录名>        # 查看指定目录的大小
grep MemTotal /proc/meminfo   # 查看内存总量
grep MemFree /proc/meminfo    # 查看空闲内存量
uptime                 # 查看系统运行时间、用户数、负载
cat /proc/loadavg      # 查看系统负载
 
#磁盘和分区
mount | column -t      # 查看挂接的分区状态
fdisk -l               # 查看所有分区
swapon -s              # 查看所有交换分区
hdparm -i /dev/hda     # 查看磁盘参数(仅适用于IDE设备)
dmesg | grep IDE       # 查看启动时IDE设备检测状况
 
#网络
ifconfig               # 查看所有网络接口的属性
iptables -L            # 查看防火墙设置
route -n               # 查看路由表
netstat -lntp          # 查看所有监听端口
netstat -antp          # 查看所有已经建立的连接
netstat -s             # 查看网络统计信息 
ip addr show           # 显示网卡地址
 
#进程
ps -ef                 # 查看所有进程
top                    # 实时显示进程状态
 
#用户
w                      # 查看活动用户
id <用户名>            # 查看指定用户信息
last                   # 查看用户登录日志
cut -d: -f1 /etc/passwd   # 查看系统所有用户
cut -d: -f1 /etc/group    # 查看系统所有组
crontab -l             # 查看当前用户的计划任务

#服务
chkconfig --list       # 列出所有系统服务
chkconfig --list | grep on    # 列出所有启动的系统服务

#程序
rpm -qa                # 查看所有安装的软件包

#主机名
vi /etc/sysconfig/network #修改主机名
vi /etc/hosts #网络配置
```

<a id="markdown-chmod" name="chmod"></a>
## chmod

```shell
最大权限
chmod 777 filename #最大权限
chmod u+x cloudera-manager-installer.bin #权限为可执行
chmod -R u+x /var/www/html/cdh/cm5.2.0/package #递归处理文件和文件夹权限
```

<a id="markdown-linux文件的复制删除和移动命令" name="linux文件的复制删除和移动命令"></a>
## Linux文件的复制、删除和移动命令

<a id="markdown-cp复制" name="cp复制"></a>
### cp复制
cp [选项] 源文件或目录 目标文件或目录

<a id="markdown-mv移动" name="mv移动"></a>
### mv移动

```shell
mv [选项] 源文件或目录 目标文件或目录

mv cdh5.5.2/ cdh/ #文件夹【cdh5.5.2】更名为【cdh】
```

<a id="markdown-rm删除" name="rm删除"></a>
### rm删除

rm [选项] 文件…

如果没有使用 -r 选项，则rm不会删除目录。该命令的各选项含义如下：

　　-f 忽略不存在的文件，从不给出提示。

　　-r 指示rm将参数中列出的全部目录和子目录均递归地删除。

　　-i 进行交互式删除。

　　使用rm命令要格外小心。因为一旦一个文件被删除，它是不能被恢复的。

例如，用户在输入cp，mv或其他命令时，不小心误输入了 rm 命令，当用户按了回车键并认识到自己的错误时，已经太晚了，文件已经没有了。

为了防止此种情况的发生，可以使用 rm 命令中的 i选项来确认要删除的每个文件。

如果用户输入 y，文件将被删除。如果输入任何其他东西，文件将被保留。在下一个例子中，用户要删除文件test和example。

然后会被要求对每个文件进行确认。用户最终决定删除 example 文件，保留 test 文件。

```shell
$ rm - ii test example Remove test ?n
Remove example ?y
rm -rf xxxx 无提示删除
```

<a id="markdown-文件拷贝" name="文件拷贝"></a>
## 文件拷贝



<a id="markdown-利用putty相关工具" name="利用putty相关工具"></a>
### 利用putty相关工具

pscp.exe拷入C:\Windows\System32，可以在命令提示符(cmd)中执行pscp命令

```
C:\Users\Administrator>pscp D:\BaiduNetdiskDownload\jdk-7u9-linux-x64.rpm root@192.168.194.129:/usr/software
```

<a id="markdown-securecrt或者xshell中使用命令rzsz" name="securecrt或者xshell中使用命令rzsz"></a>
### secureCRT或者xshell中使用命令rz/sz
send和received，站在服务端角度考虑

linux端需要安装lrzsz插件
```shell
yum -y install lrzsz
```

rz即传文件至远端，sz即从远端接收文件。

<a id="markdown-ftp" name="ftp"></a>
### FTP

安装ftp的前置条件是关掉SElinux

```bash
# vi /etc/selinux/config
```

修改 `SELINUX="disabled"` ,重启服务器。

关闭防火墙，参考上一章节。

安装命令：

```bash
yum install -y vsftpd
```

安装完成后，会生成一个目录 `/etc/vsftpd/` 和 `/var/ftp/` 两个重要的目录


[/etc/vsftpd/] 目录下有四个文件

```
1. ftpusers 文件 指定哪些用户不能访问 ftp 服务，这些用户是指 Linux 系统用户还是包括虚拟用户？
2. user_list 文件 用户列表，当 vsftpd 里 userlist_deny=NO 时，只允许这里的用户访问 ftp 服务，注意此时同时也检测 ftpusers 文件；当 vsftpd 里 userlist_deny=YES（默认） 时，不允许这里的用户 访问 ftp 服务
3. vsftpd.conf 文件 是 vsftpd 的核心配置文件
4. vsftpd_conf_migrate.sh 文件 是 vsftpd 操作的一些变量和设置脚本
```

[/var/ftp/] 目录则是默认情况下，匿名用户的根目录，也就是说默认情况下，匿名用户上传的文件将会放在这个目录下。

核心配置文件有以下几个基础的配置项

```
anonymous_enable=YES 是否允许匿名登录

local_enable=YES 是否允许本地账号(系统账号)登录

write_enable=YES 是否运行上传操作，如果要运行上传那么就要开启这个配置

listen=NO ftp服务的运行模式，=NO时表示xinetd模式；=YES时表示standlone模式。

userlist_enable=YES 控制 user_list 的功能，在上面有提到其实际意义
```

操作FTP服务的命令

```
查看服务状态：service vsftpd status

启动vsftpd服务：service vsftpd start

停止vsftpd服务：service vsftpd stop

重启vsftpd服务：service vsftpd restart
```


<a id="markdown-软件安装" name="软件安装"></a>
## 软件安装

<a id="markdown-rpm" name="rpm"></a>
### rpm

```shell
#搜索
rpm -q....
rpm -qa|grep java
rpm -qa | grep tomcat4 查看 tomcat4 是否被安装；
rpm -qip example.rpm 查看 example.rpm 安装包的信息；
rpm -qif /bin/df 查看/bin/df 文件所在安装包的信息；
rpm -qlf /bin/df 查看/bin/df 文件所在安装包中的各个文件分别被安装到哪个目录下；
#安装
rpm -ivh *.rpm
#升级
rpm -U 需要升级的包
rpm -Uvh example.rpm 升级 example.rpm 软件包
#卸载
rpm -e 需要卸载的安装包
rpm -e --nodeps java-1.7.0-openjdk-1.7.0.121-2.6.8.1.el6_8.x86_64
```

<a id="markdown-yum" name="yum"></a>
### yum

```shell
#搜索
yum search java
#列出所有可安装的软件包
yum list
#列出所有已安装的软件包
yum list installed
yum list installed |grep java #是否安装java环境
# 更新
yum update java
#卸载
yum -y remove
yum -y remove java-1.7.0-openjdk* #卸载JDK相关文件
yum -y remove tzdata-java.noarch  #卸载tzdata-java
#修改yum源配置
vi /etc/yum.repos.d/Centos.repo
```

* 修改来源为国内
>http://xtgly.blog.51cto.com/3159418/1595977

<a id="markdown-修改yum源为阿里镜像" name="修改yum源为阿里镜像"></a>
### 修改yum源为阿里镜像

```bash
# 第一步：备份原镜像文件，以免出错后可以恢复。
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

# 第二步：下载新的CentOS-Base.repo 到/etc/yum.repos.d/
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo

# 第三步：运行yum makecache生成缓存
yum makecache
```

推荐几个非常棒的国内yum源，已经设置方法，网易163yum源、中科大的yum源、sohu的yum源、阿里云yum源、清华大学的 yum源、浙江大学的 yum源、中国科技大学yum源

```
网易163 yum源，安装方法查看：http://mirrors.163.com/.help/ 
中科大的 yum源，安装方法查看：https://lug.ustc.edu.cn/wiki/mirrors/help 
sohu的 yum源，安装方法查看: http://mirrors.sohu.com/help/ 
阿里云的 yum源，安装方法查看: https://opsx.alibaba.com/mirror 
清华大学的 yum源，安装方法查看: https://mirrors.tuna.tsinghua.edu.cn/ 
浙江大学的 yum源，安装方法查看: http://mirrors.zju.edu.cn/ 
中国科技大学yum源，安装方法查看: http://centos.ustc.edu.cn/
```


<a id="markdown-chkconfig系统服务" name="chkconfig系统服务"></a>
## chkconfig系统服务

```shell
#自动启动mysql
chkconfig mysqld on
chkconfig mysqld off
#值得注意的是，如果这个服务尚未被添加到chkconfig列表中，则现需要使用–add参数将其添加进去：
chkconfig –add postfix
#如果要查询当前所有自动启动的服务
chkconfig --list 
```

<a id="markdown-iptables防火墙" name="iptables防火墙"></a>
## iptables防火墙

```bash
# 查看防火墙状态
service iptables status

# 停止防火墙
service iptables stop

# 启动防火墙
service iptables start

# 重启防火墙
service iptables restart

# 永久关闭防火墙
chkconfig iptables off

# 永久关闭后重启
chkconfig iptables on
```


