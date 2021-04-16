常用linux系统管理与网络操作相关命令

<!----more---->

[toc]

# ps 进程查看器

args:

```text
a 显示所有进程
-a 显示同一终端下的所有程序
-A 显示所有进程
c 显示进程的真实名称
-N 反向选择
-e 等于“-A”
e 显示环境变量
f 显示程序间的关系
-H 显示树状结构
r 显示当前终端的进程
T 显示当前终端的所有程序
u 指定用户的所有进程
-au 显示较详细的资讯
-aux 显示所有包含其他使用者的行程
-C<命令> 列出指定命令的状况
–lines<行数> 每页显示的行数
–width<字符数> 每页显示的字符数
–help 显示帮助信息
–version 显示版本显示
```

列含义：

- F 代表这个程序的旗标 (flag)， 4 代表使用者为 super user
- S 代表这个程序的状态 (STAT)，关于各 STAT 的意义将在内文介绍
- UID 程序被该 UID 所拥有
- PID 进程的ID
- PPID 则是其上级父程序的ID
- C CPU 使用的资源百分比
- PRI 这个是 Priority (优先执行序) 的缩写，详细后面介绍
- NI 这个是 Nice 值，在下一小节我们会持续介绍
- ADDR 这个是 kernel function，指出该程序在内存的那个部分。如果是个 running的程序，一般就是 “-“
- SZ 使用掉的内存大小
- WCHAN 目前这个程序是否正在运作当中，若为 - 表示正在运作
- TTY 登入者的终端机位置
- TIME 使用掉的 CPU 时间。
- CMD 所下达的指令为何

# top linux下的任务管理器

进程信息：

- PID：进程的ID
- USER：进程所有者
- PR：进程的优先级别，越小越优先被执行
- NInice：值
- VIRT：进程占用的虚拟内存
- RES：进程占用的物理内存
- SHR：进程使用的共享内存
- S：进程的状态。S表示休眠，R表示正在运行，Z表示僵死状态，N表示该进程优先值为负数
- %CPU：进程占用CPU的使用率
- %MEM：进程使用的物理内存和总内存的百分比
- TIME+：该进程启动后占用的总的CPU时间，即占用CPU使用时间的累加值。
- COMMAND：进程启动命令名称

命令：

```text
q：退出top命令
<Space>：立即刷新
s：设置刷新时间间隔
c：显示命令完全模式
t:：显示或隐藏进程和CPU状态信息
m：显示或隐藏内存状态信息
l：显示或隐藏uptime信息
f：增加或减少进程显示标志
S：累计模式，会把已完成或退出的子进程占用的CPU时间累计到父进程的MITE+
P：按%CPU使用率排行
T：按MITE+排行
M：按%MEM排行
u：指定显示用户进程
r：修改进程renice值
kkill：进程
i：只显示正在运行的进程
W：保存对top的设置到文件^/.toprc，下次启动将自动调用toprc文件的设置。
h：帮助命令。
q：退出
```

# 查询网络服务和端口

列出所有端口 (包括监听和未监听的):

```text
netstat -a
```

列出所有有监听的服务状态:

```text
netstat -l
```

查询7902端口现在运行什么程序

```text
 $lsof -i:7902
```

# linux后台执行命令：&与nohup

**jobs命令**

jobs命令可以查看当前有多少在后台运行。

```text
jobs -l
```

**nohup命令**

在命令的末尾加个&符号后，程序可以在后台运行，但是一旦当前终端关闭（即退出当前帐户），该程序就会停止运行。那假如说我们想要退出当前终端，但又想让程序在后台运行，该如何处理呢？

在这种情况下，我们就可以使用nohup命令。nohup就是不挂起的意思( no hang up)。该命令的一般形式为：

```text
nohup ./test &
```

运行halo博客到后台：

```text
nohup java -jar halo-1.4.8.jar >> blog.log 2>&1 &
```



# Linux Shell 输出重定向

|     **类 型**      |       **符 号**        |                          **作 用**                           |
| :----------------: | :--------------------: | :----------------------------------------------------------: |
|   标准输出重定向   |     command >file      | 以覆盖的方式，把 command 的正确输出结果输出到 file 文件中。  |
|                    |     command >>file     | 以追加的方式，把 command 的正确输出结果输出到 file 文件中。  |
| 标准错误输出重定向 |     command 2>file     |   以追加的方式，把 command 的错误信息输出到 file 文件中。    |
|                    |    command 2>>file     | 以覆盖的方式，把正确输出和错误信息同时保存到同一个文件（file）中。 |
|  正确错误同时保存  |   command >file 2>&1   | 以覆盖的方式，把正确输出和错误信息同时保存到同一个文件（file）中。 |
|                    |  command >>file 2>&1   |  追加，把正确输出和错误信息同时保存到同一个文件（file）中。  |
|                    | command >file1 2>file2 | 覆盖，把正确的输出结果输出到 file1 文件中，把错误信息输出到 file2 文件中。 |

# 将文件复制到服务器上

### 从本地服务器复制到远程服务器

复制文件：

```
$scp local_file remote_username@remote_ip:remote_folder
$scp local_file remote_username@remote_ip:remote_file
$scp local_file remote_ip:remote_folder
$scp local_file remote_ip:remote_file
```

复制目录：

```
$scp -r local_folder remote_username@remote_ip:remote_folder
$scp -r local_folder remote_ip:remote_folder
```

# Linux根据进程号查看进程位置

Linux的所有进程都保存在/proc/目录下，保存形式为：/proc/进程号。进入到进程号目录后，里面有一个cwd链接文件即指向的进程的的目录。

```
ls -l /proc/${pid}
```

# 参考

[Linux工具快速教程 — Linux Tools Quick Tutorial (linuxtools-rst.readthedocs.io)](https://linuxtools-rst.readthedocs.io/zh_CN/latest/index.html)

[linux后台执行命令：&与nohup的用法 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/59297350)