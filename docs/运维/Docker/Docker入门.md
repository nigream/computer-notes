# Docker 入门

---

参考：

```wiki
1. 尚硅谷官方学习笔记。 视频地址：https://www.bilibili.com/video/BV1Ls411n7mx?spm_id_from=333.999.0.0
```



## 相关概念

### Docker 的用途

- 在部署软件时，环境配置很麻烦，换一台机器，就要重来一次，费力费时。
- `Docker` 从根本上解决问题： **软件带环境安装** 。也就是说，安装的时候，把原始环境一模一样地复制过来（这样可以消除协作开发时 “只在我的机器上可正常工作” 的问题）。
- `Docker` 通过镜像 ( `images` )，将作业系统核心除外的运行应用程序所需要的系统环境，自下而上打包，达到应用程序跨平台间的无缝衔接运行。
- `Docker` 的理念： `Build，Ship and Run Any App , Anywhere` （在任何地方 构建、发布、和运行 任何应用）。
- `Docker` 是解决了 **运行环境和配置** 问题的 **软件容器** ，是有助于做 **持续集成** 和 **整体发布** 的 **容器虚拟化技术** 。

### 虚拟机与容器

#### 虚拟机

**virtual machine**

- 在一种操作系统里面运行另一种操作系统，比如在Windows 系统里面运行Linux 系统，应用程序对此毫无感知。
- 虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响。
- 这类虚拟机完美的运行了另一套系统，能够使应用程序，操作系统和硬件三者之间的逻辑不变。

**但是如果要部署另一套环境就需要重新装个虚拟机，也就是重新装另一个操作系统。所以有以下缺点：**

- 资源占用多
- 冗余步骤多
- 启动慢

#### Linux 容器

**Linux Containers，缩写为 LXC。**

- Linux 容器化技术不是模拟一个完整的操作系统，而是对进程进行隔离，将软件运行所需的所有资源打包到一个隔离的容器中。
- 容器与虚拟机不同，不需要捆绑一整套操作系统，只需要软件工作所需的 **库资源和设置** 。
- 系统因此而变得 **高效轻量** 并保证部署在 **任何环境** 中的软件都能 **始终如一地运行** 。

#### 二者不同之处

- **传统虚拟机技术** 是 **虚拟出一套硬件后** ，在其上运行一个 **完整操作系统** ，在该系统上再运行 **所需应用进程** ；
- 而 **容器** 内的 **应用进程** 直接运行于 **宿主系统的内核** ，容器内部没有 **系统内核** ，而且也 **没有进行硬件虚拟** ，因此容器要比传统虚拟机更为 **轻便** 。
- 每个容器之间 **互相隔离** ，每个容器有自己的 **文件系统** ，容器之间进程 **不会相互影响** ，能 **区分计算资源** 。

### Docker 的优势总结

- 更轻量：基于容器的虚拟化，仅包含业务运行所需的 `runtime` 环境，如 `CentOS/Ubuntu` 基础镜像仅 `170M` ; 一台宿主机可部署 `100 ~ 1000` 个容器。
- 更高效： **无操作系统虚拟化开销** 
  - 计算：轻量，无额外开销
  - 存储：系统盘 `aufs/dm/overlayfs` ; 数据盘 `volume`
  - 网络：宿主机网络，NS隔离
- 更敏捷、更灵活：
  - 分层的存储和包管理，`devops` 理念。
  - 支持多种 **网络配置** 。

### Docker 的三大要素

Docker 本身是一个 **容器运行载体** 或称之为 **管理引擎** 。

#### 镜像

- Docker 镜像（Image）就是一个只读的模板。
- 镜像可以用来创建 Docker 容器，一个镜像可以创建很多容器。

#### 容器

- 容器是用镜像创建的运行实例。
- 就像是Java中的类和实例对象一样，镜像是 **静态的定义** ，容器是镜像 **运行时的实体** 。
- 容器为镜像提供了一个 **标准的和隔离的运行环境** ，它可以被启动、开始、停止、删除。
- 容器 = 一个简易版的 Linux 环境（包括root用户权限、进程空间、用户空间和网络空间等）+ 运行在其中的 **应用程序** 。

#### 仓库

- 仓库（Repository）是集中 **存放镜像文件** 的场所。

## Docker 架构

### 整体架构

- Docker 是一个 Client-Server 结构的系统。
- Docker 客户机 对 Docker 守护进程（Docker daemon）发送命令，后者执行 **构建、运行和分发** Docker 容器的繁重工作。
- Docker 客户机和守护进程可以在同一个系统上运行，也可以将 Docker 客户机连接到远程 Docker 守护进程。
- Docker 客户机和守护进程使用 REST API、 UNIX 套接字 (sockets) 或网络接口进行通信。
- 还有一个 Docker 客户机是 Docker Compose，它可以处理由 **一组容器** 组成的应用程序。

![architecture](Docker入门/architecture.svg)

### Docker 运行流程

1. 用户是使用 Docker Client 与 Docker Daemon 建立通信，并发送请求给后者。
2. Docker Daemon 作为 Docker 架构中的主体部分，首先提供 Docker Server 的功能使其可以接受 Docker Client 的请求。
3. Docker Engine 执行 Docker 内部的一系列工作，每一项工作都是以一个 Job 的形式的存在。
4. Job 的运行过程中，当需要容器镜像时，则从 Docker Registry 中下载镜像，并通过镜像管理驱动 Graph Driver 将下载镜像以 Graph 的形式存储。
5. 当需要为 Docker 创建网络环境时，通过网络管理驱动 Network Driver 创建并配置 Docker 容器网络环境。
6. 当需要限制 Docker 容器运行资源或执行用户指令等操作时，则通过 Exec Driver 来完成。
7. Libcontainer 是一个 **独立的容器管理包** ，Network Driver 以及 Exec driver 都是通过 Libcontainer 来实现具体对容器进行的操作。

![image-20220317015245683](Docker入门/image-20220317015245683.png)

## 安装Docker

### 说明

Docker 官网：http://www.docker.com

- Docker 并非是一个通用的容器工具，它依赖于已存在并运行的 **Linux内核环境** 。
- Docker 实质上是在已经运行的 Linux 下制造了一个 **隔离的文件环境** ，因此它执行的效率 **几乎等同于** 所部署的Linux主机。
- 因此，Docker必须部署在含有 **Linux内核** 的系统上，如果其他系统想部署 Docker 就必须安装一个 **虚拟 Linux 环境** 。

### 在 Windows 安装 Docker (Hyper-V 方式)

参考：https://docs.docker.com/desktop/windows/install/

#### 此方法支持的系统版本

- Windows 11 64-bit: Pro version 21H2 or higher, or Enterprise or Education version 21H2 or higher.
- Windows 10 64-bit: Pro 2004 (build 19041) or higher, or Enterprise or Education 1909 (build 18363) or higher.
- 因为 Windows 10 and Windows 11 Home 无法开启 `Hyper-V` 功能（相当于虚拟机的功能）。

#### 安装步骤

1. 进入 `控制面板\程序` ，点击 `启用或关闭Windows功能` ，开启 `Hyper-V`。

### 在 Windows 安装 Docker (WSL 2 方式)

**Windows Subsystem for Linux (WSL)**

参考：https://docs.docker.com/desktop/windows/install/

#### 此方法支持的系统版本

- Windows 11 64-bit: Home or Pro version 21H2 or higher, or Enterprise or Education version 21H2 or higher.
- Windows 10 64-bit: Home or Pro 2004 (build 19041) or higher, or Enterprise or Education 1909 (build 18363) or higher.

#### 安装步骤

1. 进入 `控制面板\程序` ，点击 `启用或关闭Windows功能` ，开启 `适用于 Linux 的 Windows 子系统`。
1. 下载安装 Linux 内核：https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
1. 双击 `Docker Desktop Installer.exe` 安装 `Docker Desktop` 。
1. 重启系统。

### 在 CentOS 安装 Docker

Docker 只能在 **CentOS 7 或 CentOS 8** 的维护版本上安装。

- 



