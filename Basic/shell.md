<!-- TOC -->

- [shell](#shell)
  - [传递参数](#传递参数)
    - [测试条件](#测试条件)

<!-- /TOC -->

<a id="markdown-shell" name="shell"></a>
# shell

Shell 既是一种命令语言，又是一种程序设计语言。

Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

```shell
#!/bin/bash
echo "Hello World !"
```

<a id="markdown-传递参数" name="传递参数"></a>
## 传递参数

我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：`$n`

n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……

```shell
#!/bin/bash

echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

```bash
# 设置权限
chmod +x test.sh
# 执行时，传递参数
$ ./test.sh 1 2 3
```

命令 | 描述
---|---
$$ | 本脚本pid
$0 | 本脚本文件名
$! | 后台运行的最后一个进程的pid
$? | 上一个命令的退出状态，1为失败，其他为成功（一般是0）
$n | 传递给脚本或函数的参数，n是一个数字，表示第几个参数
$@ | 传递给脚本或函数的所有参数，”$1 $2 … $n”
$* | 传递给脚本或函数的所有参数，”$1” “$2” … “$n”

<a id="markdown-测试条件" name="测试条件"></a>
### 测试条件

在有参数时，可以使用对参数进行校验的方式处理以减少错误发生：

```shell
if [ -n "$1" ]; then
    echo "包含第一个参数"
else
    echo "没有包含第一参数"
fi
```

中括号内可以添加文件测试：

命令 | 等同 | 英文 | 描述
---|----|----|---
-eq | == | Equal | 等于
-ne | != | Not Equal | 不等于
-lt | < | Less Than | 小于
-le | <= | Less than or Equal | 小于等于
-gt | > | Greater Than | 大于
-ge | >= | Greater than or Equal | 大于等于
-z |  | Zero | 字符串为空
-n |  | Not zero | 字符串不为空
-nt |  | Newer Than | 修改日期比…更新（注意：不是比较创建日期）
-d |  | is a Directory | 是文件夹
-f |  | is a File | 是文件
-r |  | is a Readable file | 是一个可读文件
-w |  | is a Writable file | 是一个可写文件
-x |  | is an eXecutable file | 是一个可执行文件
&& |  |  | 逻辑与，需同时满足“与”左右两个条件
|| |  |  | 逻辑或，只需要满足“或”的左边或右边的条件，其中，左边的条件先判断
