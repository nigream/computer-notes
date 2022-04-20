# Windows

---

## 常用命令

### 文件相关

```sh
# 新建文件夹
mkdir xxxx 
或 
md xxxx


# 查看当前文件夹中文件
dir


# 删除文件夹
rmdir xxxx
或
rd xxxx
    # 参数
    不带参数，只能删除空文件夹，若文件夹下有子文件，则无法删除。
    /q   即quiet，静默删除而不提醒
    /s   删除文件夹及其子文件
    
    
# 移动文件夹
move xxx xxx
	# 用例
	move mtex-capability-positioning.tar C:\Users\nigre\Desktop
	
	

# 进入上一级文件夹
cd ..


# 退出到磁盘根目录
cd \


# 新建空文件
type nul>xxxx


# 新建带内容的文件
echo 内容>xxxx


# 删除文件
del xxxx
```

### winrar

```sh
# 解压文件，保留原有文件结构
winrar x xxxx.rar 
	# 参数
	不带参数就会被解压到当前目录
	# 待目录参数
	winrar x xxxx.rar xxx（末尾需要\）
		如：winrar x mtex-capability-positioning-1.3.0.zip .\mtex-capability-positioning-1.3.0\
```

### 注释

```bash
:: 这是注释
```







### 查看端口占用情况

```sh
# 查看占用端口的进程（可以得到进程的PID）
netstat -ano|findstr "8080"
# 根据PID查询具体的进程名称
tasklist|findstr "8592"
```

### 其他

清屏  `cls`
