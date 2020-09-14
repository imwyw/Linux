<!-- TOC -->

- [开发相关环境](#开发相关环境)
  - [JDK配置](#jdk配置)
  - [tomcat配置](#tomcat配置)

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


