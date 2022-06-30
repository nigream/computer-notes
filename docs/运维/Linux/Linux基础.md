# Linux基础

---

## 目录结构

### `/` 与 `~` 

`/` 表示Linux的根目录，根目录下一般有以下常用目录(可能不全) : `bin  boot  dev  etc  home  lib  opt  root  sbin  srv  sys  tmp  usr  var`

`~` 表示当前用户的 `家目录` ， `root` 用户的 `家目录` 是 `/root` ，普通用户 `a` 的 `家目录` 是 `/home/a` (每创建一个新的普通用户，都会在 `/home` 目录下创建一个以用户名命名的目录)



## 常用命令

### 清屏

```sh
# 快捷键 ctrl + L
```



### 系统相关

```shell
# 查看系统内核版本
[root@iZwz9havs2j3eja5yo3nrxZ ~]# uname -r
5.10.60-9.al8.x86_64
```

### `|` 命令

即 **管道命令** ，是指 `|` 左边的 **运行结果** ，是 `|` 右边的 **输入条件或者范围** 。

如: `history | grep date` ，指从 `history` 这条命令运行的结果中，显示包含有 `date` 字符串的结果。

### rpm

```sh
# 列出所有安装过的包
rpm -qa

# 列出所有名称包含 xxx 的安装过的包
rpm -qa | grep xxx
```

### grep 

用于查找文件里符合条件的字符串。

### cat / less/ more

- **cat 命令**
  - 用于显示 **整个文件的内容** ，单独使用 **没有翻页功能** 。
  - 它还可以将数个文件 **合并** 成一个文件。
- **more 命令**
  - 让画面在显示 **满一页时** 暂停，此时可按 **空格健** 继续显示下一个画面，或按 **q 键** 停止显示。
- **less 命令**
  - less 命令的用法与 more 命令类似，也可以用来浏览超过一页的文件。
  - 不同的是 less 命令除了可以按空格键向下显示文件外，还可以利用 **上下键** 来 **翻动文件** 。
  - 当要结束浏览时，只要在 less 命令的提示符 `:` 下按 **q 键** 即可。

### tail

```sh
# 展示文件末尾的xx行，-f表示若文件有更新时，则会在屏幕显示
tail -n 显示的行数 -f 文件路径
```



### 防火墙

```sh
# 开启防火墙
systemctl start firewalld
# 关闭防火墙
systemctl stop firewalld
# 设置开放访问的端口（需要重启防火墙）
firewall-cmd --zone=public --add-port=80/tcp --permanent
# 查看所有开启的端口
firewall-cmd --list-ports
```



### 进程相关

```sh
# 查询指定服务名称的进程信息
ps -ef | grep hbase

字段含义如下：
UID       PID       PPID      C     STIME    TTY       TIME              CMD

root      2649       1           0       Jun28      ?          03:53:28        java -jar  a.jar
root     19997     19712     0     10:24      pts/0    00:00:00       grep --color=auto jar

列序号    列含义    列含义说明
1    UID    用户标识ID
2    PID    进程ID
3    PPID    父进程ID
4    C    CPU占用率
5    STIME    进程开始时间
6    TTY    启动此进程的TTY（终端设备）
7    TIME    此进程运行的总时间
8    CMD    完整的命令名(带启动参数)
```

