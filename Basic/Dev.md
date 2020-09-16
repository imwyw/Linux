<!-- TOC -->

- [开发相关环境](#开发相关环境)
  - [JDK配置](#jdk配置)
  - [tomcat配置](#tomcat配置)
  - [springboot部署](#springboot部署)
  - [内核升级](#内核升级)
  - [docker安装](#docker安装)

<!-- /TOC -->

<a id="markdown-开发相关环境" name="开发相关环境"></a>
# 开发相关环境

<a id="markdown-jdk配置" name="jdk配置"></a>
## JDK配置


```bash
# 创建目录，-p 表示如果没有上级目录的话一并创建
mkdir -p /usr/lib/jvm

# 解压
tar -zxvf jdk-8u202-linux-x64.tar.gz -C /usr/lib/jvm/

```

- 环境变量的设置

```bash
# 设置环境变量
vi /etc/profile
```

在【profile】文件开头添加内容：



```bash
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_202
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export  PATH=${JAVA_HOME}/bin:$PATH
```

执行【profile】，使之生效：

```bash
source /etc/profile

# 测试环境
java -version
```

<a id="markdown-tomcat配置" name="tomcat配置"></a>
## tomcat配置

> https://tomcat.apache.org/download-90.cgi 

下载安装包，拷贝至 【/usr/local/software】，解压至lib目录：

```bash
tar -zxvf apache-tomcat-9.0.37.tar.gz -C /usr/lib/
```

执行【startup.sh】

```bash
# 打开 bin 目录
cd /usr/lib/apache-tomcat-9.0.37/bin/

# 启动Tomcat
./startup.sh
```

> https://blog.csdn.net/lcyaiym/article/details/76696192


<a id="markdown-springboot部署" name="springboot部署"></a>
## springboot部署

区分 war 和 jar 部署方式区别：


> https://blog.csdn.net/weixin_39274753/article/details/81557501

<a id="markdown-内核升级" name="内核升级"></a>
## 内核升级

```bash
# 查看os版本
cat /etc/issue

# 查看内核版本
uname -r
```

由于 docker 对于 CentOS 的内核有要求，内核版本必须高于 3.8

先升级nss (Network Security Service, 网络安全服务)

```bash
yum update nss
```

升级内核需要使用elrepo的yum源，在安装yum源之前还需要我们导入elrepo的key，如下：

```bash
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org

yum install https://www.elrepo.org/elrepo-release-6.el6.elrepo.noarch.rpm
```

elrepo的key安装完毕后，我们下面开始正式升级内核。

在yum的elrepo源中有ml和lt两种内核，其中ml(mainline)为最新版本的内核，lt为长期支持的内核。

```bash
# 如果要安装ml内核，使用如下命令：
yum --enablerepo=elrepo-kernel -y install kernel-ml

# 如果要安装lt内核，使用如下命令：
yum --enablerepo=elrepo-kernel -y install kernel-lt
```

内核升级完毕后，不会立即生效，还需要我们修改grub.conf文件。

内核升级完毕后，需要我们修改内核的启动顺序，默认启动的顺序应该为1,升级以后内核是往前面插入为0，如下：

```bash
vi /etc/grub.conf
```

修改默认启动内核：`default=0`，重启后检查内核：

```bash
uname -r
```

参考：
>http://elrepo.org/tiki/tiki-index.php

>https://www.cnblogs.com/ilanni/p/6158519.html


<a id="markdown-docker安装" name="docker安装"></a>
## docker安装

yum安装docker No package docker available


yum没有找到docker包，更新epel第三方软件库，运行命令：

```bash
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm

yum install epel-release

yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

# 安装docker-ce社区版足够了
yum -y install docker-ce
```

之后就可以愉快地安装了。

