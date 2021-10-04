## **一、Java**概述

### **1.1** 什么是**Java**语言

> Java语言是美国Sun公司（Stanford University Network），在1995年推出的高级的编程语言。

### **1.2** **Java**语言发展历史

```java
1995年Sun公司发布Java1.0版本
1997年发布Java 1.1版本
1998年发布Java 1.2版本
2000年发布Java 1.3版本
2002年发布Java 1.4版本
2004年发布Java 1.5版本
2006年发布Java 1.6版本
2009年Oracle甲骨文公司收购Sun公司，并于2011发布Java 1.7版本
2014年发布Java 1.8版本
2017年发布Java 9.0版本
```

### **1.3 Java**之父

> Java之父James Gosling

![](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/20210314001405.jpg)

## **二、**计算机基础知识

### **2.1 **二进制

> 二进制只包含0、1两个数，逢二进一，1+1=10。每一个0或者每一个1就是一个位，叫做一个bit（比特）。

**① 十进制数据转成二进制数据：**使用除以2获取余数的方式

![](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/20210314001830.png)

**② 二进制数据转成十进制数据：**使用8421编码的方式

![](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/20210314001848.png)

### **2.2 **字节

> 字节是我们常见的计算机中最小存储单元。计算机存储任何的数据，都是以字节的形式存储，右键点击文件属性， 我们可以查看文件的字节大小。8个bit（二进制位） 0000-0000表示为1个字节，写成**1 byte**或者**1 B**。

```java
8 bit = 1 B
1024 B = 1 KB
1024 KB = 1 MB
1024 MB = 1 GB
1024 GB = 1 TB
PB、EB、ZB、YB、BB、NB、DB
```

### **2.3 **常用**DOS**命令

> DOS是一个早期的操作系统，现在已经被Windows系统取代，对于我们开发人员，目前需要在DOS中完成一些事情，因此就需要掌握一些必要的命令。

**① 进入DOS操作窗口：**

- 按下Windows+R键盘，打开运行窗口，输入cmd回车，进入到DOS的操作窗口。、
- 打开DOS命令行后，看到一个路径 c:\\user 就表示我们现在操作的磁盘是c盘。

**② 常用命令：**

|       命令       |    操作符号     |
| :--------------: | :-------------: |
|     盘符切换     |    ``盘符:``    |
|  查看当前文件夹  |     ``dir``     |
|    进入文件夹    | ``cd 文件夹名`` |
|    退出文件夹    |    ``cd ..``    |
| 退出到磁盘根目录 |    ``cd \``     |
|       清屏       |     ``cls``     |

```java
public void main(String[] args) {
    int a = 2;
}
```

