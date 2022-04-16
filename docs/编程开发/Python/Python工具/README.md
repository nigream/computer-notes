# Python工具

---

## 名词解释

**参考：**

```wiki
1. https://www.cnblogs.com/moyui/articles/12556700.html
2. https://www.cnblogs.com/blackcat-meow/p/10504680.html
3. https://www.cnblogs.com/mishdong123rousi/p/9646705.html
4. https://mp.weixin.qq.com/s?__biz=Mzg2NzYyNjg2Nw==&mid=2247490284&idx=1&sn=34931b236e7775285a6c9eecbb6d00f2&source=41#wechat_redirect
5. https://zhuanlan.zhihu.com/p/81025311
```



### Python

指 `Python` 语言。

```crystal
其中 Python2.x 和 Python3.x 差别很大， Python2.x 编写的程序可能无法在 Python3.x 环境下运行。
```



### pip

pip 是安装 Python 自带的 **包管理工具** ，但是 pip 不会自动处理包之间的依赖关系。

```crystal
1、pip 和 pip3 版本不同，但都位于 Scripts\ 目录下。
2、如果系统中只安装了 Python2 ，那么就只能使用 pip 。
3、如果系统中只安装了 Python3 ，那么既可以使用 pip 也可以使用 pip3 ，二者是等价的。
4、如果系统中同时安装了 Python2 和 Python3 ，则 pip 默认给 Python2 用，pip3 指定给 Python3 用。
5、重要：虚拟环境中，若只存在一个 Python 版本，可以认为 pip 和 pip3 命令都是相同的

# 镜像
https://pypi.tuna.tsinghua.edu.cn/simple/
```



### virtualenv

virtualenv 是一个 **环境管理工具** ，使用 virtualenv 可以创建一个完全隔离的环境，但 virtualenv 只能创建基于 **本机已存在的 Python 版本** 的虚拟环境。



### pipenv

相当于 `pip` + `virtualenv` ，但是 `pipenv` 的 依赖管理能力强于 `pip` ，比 `pip` 更方便。



### Poetry

1. Poetry 和 Pipenv 类似，是一个 Python 虚拟环境和依赖管理工具。
2. 另外它还提供了包管理功能，比如打包和发布。
3. 可以把它看做是 Pipenv 等类似工具的超集。
4. Poetry 可以同时管理 Python 库和 Python 程序。



### conda

#### conda

https://docs.conda.io/en/latest/

1. 可以把 `conda` 看作是 `pip` + `virtualenv` + `PVM (Python Version Manager)` + 一些必要的底层库，也就是一个更完整也更大的集成管理工具。
2. conda 是一个 **包管理工具** ，可以安装多种语言（包括 Python ）的包，它具有完美的包依赖关系处理能力，不用过分地去手动处理包之间的依赖关系。
3. conda 也是一个 **环境管理工具** ，可以创建 **任意 Python 版本** 的虚拟隔离环境。



#### Anaconda

https://www.anaconda.com/blog/whats-in-a-name-clarifying-the-anaconda-metapackage

Anaconda有如下几种含义：

- Anaconda 公司。
- Anaconda Distribution = Anaconda Installer （Anaconda 公司的一个开源的 Python 发行版本，专注于数据科学，其包含了 conda、Python 等软件包，以及 numpy、pandas（数据分析）、scipy等科学计算包）
- anaconda metapackage（里面包含 Anaconda 所需的所有依赖）
- Anaconda Enterprise（企业版）
- 不相关的同名软件（一个免费开源的Linux系统下载器）



#### Miniconda

Miniconda 功能等同于 Anaconda ，但它只包含 Anaconda 最基本的内容—— python 与 conda ，以及必要的依赖项，所占空间小于 Anaconda 。



#### 简单区分

https://stackoverflow.com/a/58147674/11249244

```crystal
Miniconda installer = Python + `conda`

Anaconda installer = Python + `conda` + meta package `anaconda`

meta Python pkg `anaconda` = about 160 other Python packages for daily use in data science

Anaconda installer = Miniconda installer + `conda install anaconda`

# 类似maven中的聚合工程（本身没有代码，只是引用一些依赖）
Meta packages, are packages that do NOT contain actual softwares and simply depend on other packages to be installed. 
```

- `conda` 不是一个命令行工具，他是一个 `Python` 包，所以他不能下载直接使用，这里就需要用到 `Anaconda` 和 `Miniconda` 。
- `Miniconda` 执行 `conda install anaconda` 即可让 `Mniconda` 变成 `Anaconda` ，只是文件夹名称不同。



#### conda-forge

https://conda-forge.org/docs/user/introduction.html

- 为 conda 提供软件包的一个社区。
- conda 官方的打包的包通过 default channel 发放，conda-forge 打包的包通过  `conda-forge` channel 发放。



#### Miniforge

- 与 Miniconda 类似，不过在有些平台 (如 ARMv8 64-bit、aarch64) Miniconda 不提供适配，但是 Miniforge 提供。



### Pycharm

Python 软件的开发工具。
