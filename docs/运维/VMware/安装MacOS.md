安装MacOS

---

参考：

1. VM安装MacOS教程：https://blog.csdn.net/Simon_477/article/details/110252233
2. 

## 安装包准备

1. 



## 解锁VMware

**参考：**

1.  Auto-Unlocker使用教程：https://www.beiwangshan.com/1033.html
2.  Auto-Unlocker下载地址：https://github.com/paolo-projects/auto-unlocker/releases/

- 因为 `VMware` 默认是不支持 `MacOS` 的，所以我们需要解锁，让其支持我们安装 `MacOS` 。

- 打开 `VMware` --> 文件 --> 新建虚拟机 --> 选择“典型” --> 下一步 --> 选择“稍后安装操作系统” --> 下一步。这里可以看到目前支持安装的系统。

  ![image-20220328025254450](安装MacOS/image-20220328025254450.png)

- 从 github 上下载 Auto-Unlocker ，解压后，双击 `Unlocker.exe` ，程序自动识别 VMware 安装位置，我们点击 Patch 按钮即可，等待其执行完成。

  ![image-20220328025603507](安装MacOS/image-20220328025603507.png)

- 再按上述步骤 **新建虚拟机** 时，发现已经新增了 `MacOS` 。

  ![image-20220328031746936](安装MacOS/image-20220328031746936.png)

## 新建虚拟机

- 打开 `VMware` --> 文件 --> 新建虚拟机 --> 选择“典型” --> 下一步 --> 指定iso文件位置（此时有如下警告出现） --> 下一步。

  ![image-20220328034752647](安装MacOS/image-20220328034752647.png)

- 选择 Apple Mac OS 11.1 版本（这里最高只用 `11.1` ）--> 下一步。

  ![image-20220328040110420](安装MacOS/image-20220328040110420.png)

- 选择安装位置 --> 下一步。

  ![image-20220328040524442](安装MacOS/image-20220328040524442.png)

- 默认 --> 下一步。

  ![image-20220328040703924](安装MacOS/image-20220328040703924.png)

- 默认 --> 完成。

  ![image-20220328040746670](安装MacOS/image-20220328040746670.png)

- 编辑 `E:\vms\macos\macOS 11.2.3.vmx` ，在最后一行加上 `smc.version = 0` ，保存。

## 配置虚拟机

- 开启刚创建的虚拟机。

- 选择语言。

  ![image-20220328041448579](安装MacOS/image-20220328041448579.png)

- 选择 `磁盘工具` --> 点击 `继续` 。

  ![image-20220328042211732](安装MacOS/image-20220328042211732.png)

- 选择 `安装 macOS Big Sur` ，点击 `继续` 。

- 

  ![image-20220328041637005](安装MacOS/image-20220328041637005.png)

- 点击 `继续` --> 点击 `同意` 。