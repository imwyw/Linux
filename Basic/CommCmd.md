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
        - [windows2linux](#windows2linux)
    - [软件安装](#软件安装)
        - [.tar.gz文件和.rpm文件的区别](#targz文件和rpm文件的区别)
            - [*.rpm形式的二进制软件包](#rpm形式的二进制软件包)
            - [*.tar.gz/*.tgz、*.bz2形式的二进制软件包](#targztgzbz2形式的二进制软件包)
        - [rpm](#rpm)
        - [yum](#yum)
    - [chkconfig](#chkconfig)

<!-- /TOC -->

<a id="markdown-基本操作" name="基本操作"></a>
# 基本操作

<a id="markdown-启动级别" name="启动级别"></a>
## 启动级别
Linux下是使用0到6来表示不同的启动模式的，每个数字代表的含义如下：
```
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
```
#切换到图形
init 5
#切换到命令行
init 3
```

<a id="markdown-本机wmware连接方式" name="本机wmware连接方式"></a>
## 本机WMware连接方式

<a id="markdown-显示各类信息" name="显示各类信息"></a>
## 显示各类信息
```
# 显示简略信息
ls
# 显示权限
ls -l = ls -lrt = ll
# 显示权限包含隐藏文件
ls -la
# 显示路径
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
最大权限
chmod 777 filename #最大权限
chmod u+x cloudera-manager-installer.bin #权限为可执行
chmod -R u+x /var/www/html/cdh/cm5.2.0/package #递归处理文件和文件夹权限

<a id="markdown-linux文件的复制删除和移动命令" name="linux文件的复制删除和移动命令"></a>
## Linux文件的复制、删除和移动命令
<a id="markdown-cp复制" name="cp复制"></a>
### cp复制
cp [选项] 源文件或目录 目标文件或目录
<a id="markdown-mv移动" name="mv移动"></a>
### mv移动
mv [选项] 源文件或目录 目标文件或目录
mv cdh5.5.2/ cdh/ #文件夹【cdh5.5.2】更名为【cdh】
<a id="markdown-rm删除" name="rm删除"></a>
### rm删除
rm [选项] 文件…
如果没有使用 -r 选项，则rm不会删除目录。该命令的各选项含义如下：
　　-f 忽略不存在的文件，从不给出提示。
　　-r 指示rm将参数中列出的全部目录和子目录均递归地删除。
　　-i 进行交互式删除。
　　使用rm命令要格外小心。因为一旦一个文件被删除，它是不能被恢复的。例如，用户在输入cp，mv或其他命令时，不小心误输入了rm命令，当用户按了回车键并认识到自己的错误时，已经太晚了，文件已经没有了。为了防止此种情况的发生，可以使用rm命令中的 i选项来确认要删除的每个文件。如果用户输入y，文件将被删除。如果输入任何其他东西，文件将被保留。在下一个例子中，用户要删除文件test和example。然后会被要求对每个文件进行确认。用户最终决定删除example文件，保留test文件。
　　$ rm - ii test example Remove test ?n
　　Remove example ?y
rm -rf xxxx 无提示删除

<a id="markdown-文件拷贝" name="文件拷贝"></a>
## 文件拷贝
<a id="markdown-windows2linux" name="windows2linux"></a>
### windows2linux
* 利用putty相关工具
pscp.exe拷入C:\Windows\System32，可以在命令提示符(cmd)中执行pscp命令
C:\Users\Administrator>pscp D:\BaiduNetdiskDownload\jdk-7u9-linux-x64.rpm root@192.168.194.129:/usr/software

* secureCRT中使用命令rz/sz
send和received，站在服务端角度考虑
linux端需要安装lrzsz插件
yum -y install lrzsz

<a id="markdown-软件安装" name="软件安装"></a>
## 软件安装
<a id="markdown-targz文件和rpm文件的区别" name="targz文件和rpm文件的区别"></a>
### .tar.gz文件和.rpm文件的区别
Linux软件的二进制分发是指事先已经编译好二进制形式的软件包的发布形式，其优点是安装使用容易，缺点则是缺乏灵活性，如果该软件包是为特定的硬件/操作系统平台编译的，那它就不能在另外的平台或环境下正确执行。 

<a id="markdown-rpm形式的二进制软件包" name="rpm形式的二进制软件包"></a>
#### *.rpm形式的二进制软件包
安装：rpm -ivh *.rpm
卸载：rpm -e packgename
说明：RPM（RedHat Packge Manager）是RedHat公司出的软件包管理器，使用它可以很容易地对rpm形式的软件包进行安装、升级、卸载、验证、查询等操作，安装简单，而卸载时也可以将软件安装在多处目录中的文件删除干净，因此推荐初学者尽可能使用rpm形式的软件包。rpm的参数中-i是安装，-v是校验，-h是用散列符显示安装进度，*.rpm是软件包的文件名（这里的*.rpm特指*.src.rpm以外的以rpm为后缀的文件）；参数-e是删除软件包，packgename是软件包名，与软件包的文件名有所区别，它往往是文件名中位于版本号前面的字符串，例如apache-3.1.12-i386.rpm和apache-devel-3.1.12-i386.rpm是软件包文件名，它们的软件包名称分别是apache和apache-devel。更多的rpm参数请自行参看手册页：man rpm。 

<a id="markdown-targztgzbz2形式的二进制软件包" name="targztgzbz2形式的二进制软件包"></a>
#### *.tar.gz/*.tgz、*.bz2形式的二进制软件包
安装：tar zxvf *.tar.gz 或 tar yxvf *.bz2
卸载：手动删除
说明：*.tar.gz/*.bz2形式的二进制软件包是用tar工具来打包、用gzip/bzip2压缩的，安装时直接解包即可。对于解压后只有单一目录的软件，卸载时用命令“rm -rf 软件目录名”；如果解压后文件分散在多处目录中，则必须一一手动删除（稍麻烦），想知道解压时向系统中安装了哪些文件，可以用命令“tar ztvf *.tar.gz”/“tar ytvf *.bz2”获取清单。tar的参数z是调用gzip解压，x是解包，v是校验，f是显示结果，y是调用bzip2解压，t是列出包的文件清单。更多的参数请参看手册页：man tar。
如果你更喜欢图形界面的操作，可以在X-Window下使用KDE的ArK压缩档案管理工具。 
> http://www.cnblogs.com/ningvsban/archive/2012/12/18/2823100.html

<a id="markdown-rpm" name="rpm"></a>
### rpm
```
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
```
#搜索
yum search java
#列出所有可安装的软件包
yum list
#列出所有已安装的软件包
yum list installed
yum list installed |grep java #是否安装java环境
#卸载
yum -y remove
yum -y remove java-1.7.0-openjdk* #卸载JDK相关文件
yum -y remove tzdata-java.noarch  #卸载tzdata-java
#修改yum源配置
vi /etc/yum.repos.d/Centos.repo
```
* 修改来源为国内
>http://xtgly.blog.51cto.com/3159418/1595977


<a id="markdown-chkconfig" name="chkconfig"></a>
## chkconfig
```
#自动启动mysql
chkconfig mysqld on
chkconfig mysqld off
#值得注意的是，如果这个服务尚未被添加到chkconfig列表中，则现需要使用–add参数将其添加进去：
chkconfig –add postfix
#如果要查询当前所有自动启动的服务
chkconfig --list 
```








