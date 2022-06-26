# Conda入门

---

## 安装

### Windows

#### 安装Anaconda

1. 具体安装步骤参考Miniconda。安装后包括的软件如下：

   ![image-20220118004409656](Conda入门/image-20220118004409656.png)

2. 其中，Anaconda Navigator 是一个可视化面板，管理Anaconda已安装的软件。

   点击 Environments 可以管理 Python 的库。

   ![image-20220118004633499](Conda入门/image-20220118004633499.png)

3. Jupyter Notebook 是基于网页的用于交互计算的应用程序。其可被应用于全过程计算：开发、文档编写、运行代码（不止Python）和展示结果。

4. Spyder 是一个简单的集成开发环境。和其他的 Python 开发环境相比，它最大的优点就是模仿 MATLAB 的 “工作空间” 的功能，可以很方便地观察和修改数组的值。



#### 安装MiniConda

1. 到此地址下载 `MiniConda Installer` ：https://docs.conda.io/en/latest/miniconda.html 。

2. 双击安装，选择安装位置，到这里取消勾选两个复选框。

   ![image-20220416102916267](Conda入门/image-20220416102916267.png)

   这里如果勾选了这个两个复选框，则会在系统环境变量 `path` 中加入如下几项：

   ```sh
   E:\devtools\sdk\conda\miniconda4.11.0 # 此项是把conda自带的python加入环境变量
   E:\devtools\sdk\conda\miniconda4.11.0\Library\mingw-w64\bin
   E:\devtools\sdk\conda\miniconda4.11.0\Library\usr\bin
   E:\devtools\sdk\conda\miniconda4.11.0\Library\bin
   E:\devtools\sdk\conda\miniconda4.11.0\Scripts # 此项可以把conda加入环境变量
   ```

   - 如果不加此项：`E:\devtools\sdk\conda\miniconda4.11.0\Library\bin` ，则下载依赖包时，会报如下的错误。
   - 因为下载依赖包时，需要用到此目录中的 `libcrypto-1_1-x64.dll` 和 `libssl-1_1-x64.dll` 两个文件；如果不加此项，需要将这两个文件复制进 `\DLLs` 目录中。

   ```sh
   PS E:\codes\python\start\EIN-SELD> conda env create -f environment.yml
   Collecting package metadata (repodata.json): failed
   
   CondaHTTPError: HTTP 000 CONNECTION FAILED for url <https://conda.anaconda.org/pytorch/win-64/repodata.json>
   Elapsed: -
   
   An HTTP error occurred when trying to retrieve this URL.
   HTTP errors are often intermittent, and a simple retry will get you on your way.
   'https://conda.anaconda.org/pytorch/win-64'
   ```

3. 添加环境变量。

   ```properties
   # 新增 CONDA_HOME 
   CONDA_HOME = E:\devtools\sdk\conda\miniconda4.11.0
   
   # 在 Path 中新增如下几项：
   %CONDA_HOME%\Scripts;%CONDA_HOME%\Library\bin;%CONDA_HOME%\Library\mingw-w64\bin;%CONDA_HOME%\Library\usr\bin
   ```
   
4. 添加镜像：在 `%homepath%\.condarc` 中加入如下配置（如果没有则新建该文件）。

   - 配置后执行 `conda clean --i` 以清除索引缓存，保证用的是镜像站提供的索引。
   - 下载依赖时可能需要多试几次（可能要关闭代理）。
   - 下面配置参见清华镜像：https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/

   ```yaml
   channels:
     - defaults
   show_channel_urls: true
   default_channels:
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
   custom_channels:
     conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
   ```

5. 配置代理，在 `C:\Users\nigre\.condarc` 中加入如下配置：

   ```yaml
   proxy_servers: {http: http://localhost:7890, https: http://localhost:7890}
   ```

   若使用代理，而不使用镜像，则配置文件最终可以这样：
   
   ```yaml
   channels:
     - anaconda
     - conda-forge
     - msys2
     - bioconda
     - menpo
     - pytorch
     - simpleitk
     - defaults
   
   proxy_servers: {http: http://localhost:7890, https: http://localhost:7890}
   ```
   

## 疑难杂症

### 系统找不到指定的路径

#### 问题描述

- Windows下卸载 `Anaconda` 后又安装 `Miniconda` ，发现 `命令行窗口` 会出现 `系统找不到指定的路径。` 字样。
- 此问题甚至导致 `mvn package` 失败（真是天理难容啊）。

#### 解决办法

参考：https://blog.csdn.net/jindaxiaoooo/article/details/108436982

- 注册表进入 `计算机\HKEY_CURRENT_USER\Software\Microsoft\Command Processor` 路径。

  ![image-20220420213903682](Conda入门/image-20220420213903682.png)

- 将 `Autorun` 注册表项的值，改为 `Minconda` 的。

- 重启 `cmd` 恢复正常。

- `mvn package` 也恢复正常。 

### 命令行窗口显示 base 字样

#### 问题描述

- 在打开 `JetBrain` 家族、`VSCode` 等软件的终端窗口，会出现如下 `base` 字样。
- 注：本来是没问题的，但是强迫症表示不爽。

![image-20220420215028857](Conda入门/image-20220420215028857.png)

#### 解决方案

参考：https://www.freesion.com/article/74871333990/

- 在配置文件 `C:\Users\nigre\.condarc`  中加上 `changeps1: False` 配置，即可解决。
- 该配置用于设置是否展示当前 `conda` 环境名称：https://conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html#change-command-prompt。

### ERROR REPORT

#### 问题描述

```crystal
# >>>>>>>>>>>>>>>>>>>>>> ERROR REPORT <<<<<<<<<<<<<<<<<<<<<<

    Traceback (most recent call last):
      File "E:\devtools\sdk\conda\miniconda4.11.0\lib\site-packages\conda\cli\main.py", line 143, in main      
        return activator_main()
      File "E:\devtools\sdk\conda\miniconda4.11.0\lib\site-packages\conda\activate.py", line 1222, in main     
        print(activator.execute(), end='')
    UnicodeEncodeError: 'gbk' codec can't encode character '\ue1bb' in position 912: illegal multibyte sequence

`$ E:\devtools\sdk\conda\miniconda4.11.0\Scripts\conda-script.py shell.powershell activate base`

  environment variables:
                 CIO_TEST=<not set>
        CONDA_DEFAULT_ENV=ein
                CONDA_EXE=E:\devtools\sdk\conda\miniconda4.11.0\Scripts\conda.exe
               CONDA_HOME=E:\devtools\sdk\conda\miniconda4.11.0
             CONDA_PREFIX=E:\devtools\sdk\conda\miniconda4.11.0\envs\ein
               CONDA_ROOT=E:\devtools\sdk\conda\miniconda4.11.0
              CONDA_SHLVL=1
           CURL_CA_BUNDLE=<not set>
                   GOPATH=E:\devtools\repos\go\public
                 HOMEPATH=\Users\nigre
                NODE_PATH=E:\devtools\sdk\nodejs\node_global\node_modules
                     PATH=E:\devtools\sdk\conda\miniconda4.11.0;E:\devtools\sdk\conda\miniconda4
                          .11.0\Library\mingw-w64\bin;E:\devtools\sdk\conda\miniconda4.11.0\Libr
                          ary\usr\bin;E:\devtools\sdk\conda\miniconda4.11.0\Library\bin;E:\devto
                          ols\sdk\conda\miniconda4.11.0\Scripts;E:\devtools\sdk\conda\miniconda4
                          .11.0\bin;E:\devtools\sdk\conda\miniconda4.11.0\envs\ein;E:\devtools\s
                          dk\conda\miniconda4.11.0\envs\ein\Library\mingw-w64\bin;E:\devtools\sd
                          k\conda\miniconda4.11.0\envs\ein\Library\usr\bin;E:\devtools\sdk\conda
                          \miniconda4.11.0\envs\ein\Library\bin;E:\devtools\sdk\conda\miniconda4
                          .11.0\envs\ein\Scripts;E:\devtools\sdk\conda\miniconda4.11.0\envs\ein\
                          bin;E:\devtools\sdk\conda\miniconda4.11.0\condabin;E:\devtools\VMware\
                          bin;E:\devtools\SecureCRT;C:\Windows\system32;C:\Windows;C:\Windows\Sy
                          stem32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0;C:\Windows\Syst
                          em32\OpenSSH;E:\devtools\sdk\jdk\jdk-11.0.1\bin;E:\devtools\version\gi
                          t\cmd;E:\devtools\version\svn\bin;E:\devtools\version\TortoiseSVN\bin;
                          E:\devtools\sdk\nodejs;E:\devtools\sdk\nodejs\node_global;C:\Program
                          Files (x86)\National Instruments\Shared\LabVIEW
                          CLI;D:\software\calibre;C:\Program Files\Docker\Docker\resources\bin;C
                          :\ProgramData\DockerDesktop\version-bin;D:\software\winrar;E:\devtools
                          \ide\寰俊web寮€鍙戣€呭伐鍏穃dll;E:\devtools\sdk\go\go1_18\bin;E:\devtools\sdk\
                          conda\miniconda4.11.0\Scripts;E:\devtools\sdk\conda\miniconda4.11.0\Li
                          brary\bin;E:\devtools\sdk\conda\miniconda4.11.0\Library\mingw-w64\bin;
                          E:\devtools\sdk\conda\miniconda4.11.0\Library\usr\bin;E:\devtools\sdk\
                          conda\miniconda4.11.0;C:\Users\nigre\AppData\Local\Microsoft\WindowsAp
                          ps;C:\Users\nigre\AppData\Roaming\npm;C:\Users\nigre\AppData\Local\Byp
                          assRuntm;C:\Users\nigre\go\bin
             PSMODULEPATH=C:\Users\nigre\Documents\WindowsPowerShell\Modules;C:\Program Files\Wi
                          ndowsPowerShell\Modules;C:\Windows\system32\WindowsPowerShell\v1.0\Mod
                          ules;E:\devtools\version\svn\PowerShellModules
               PYTHONPATH=E:\devtools\sdk\conda\miniconda4.11.0
       REQUESTS_CA_BUNDLE=<not set>
            SSL_CERT_FILE=<not set>

     active environment : ein
    active env location : E:\devtools\sdk\conda\miniconda4.11.0\envs\ein
            shell level : 1
       user config file : C:\Users\nigre\.condarc
 populated config files : C:\Users\nigre\.condarc
          conda version : 4.12.0
    conda-build version : not installed
         python version : 3.9.7.final.0
       virtual packages : __cuda=11.5=0
                          __win=0=0
                          __archspec=1=x86_64
       base environment : E:\devtools\sdk\conda\miniconda4.11.0  (writable)
      conda av data dir : E:\devtools\sdk\conda\miniconda4.11.0\etc\conda
  conda av metadata url : None
           channel URLs : https://conda.anaconda.org/pytorch/win-64
                          https://conda.anaconda.org/pytorch/noarch
                          https://conda.anaconda.org/anaconda/win-64
                          https://conda.anaconda.org/anaconda/noarch
                          https://conda.anaconda.org/conda-forge/win-64
                          https://conda.anaconda.org/conda-forge/noarch
                          https://conda.anaconda.org/msys2/win-64
                          https://conda.anaconda.org/msys2/noarch
                          https://conda.anaconda.org/bioconda/win-64
                          https://conda.anaconda.org/bioconda/noarch
                          https://conda.anaconda.org/menpo/win-64
                          https://conda.anaconda.org/menpo/noarch
                          https://conda.anaconda.org/simpleitk/win-64
                          https://conda.anaconda.org/simpleitk/noarch
                          https://conda.anaconda.org/nogil/win-64
                          https://conda.anaconda.org/nogil/noarch
                          https://conda.anaconda.org/ehmoussi/win-64
                          https://conda.anaconda.org/ehmoussi/noarch
                          https://conda.anaconda.org/cctbx202112/win-64
                          https://conda.anaconda.org/cctbx202112/noarch
                          https://conda.anaconda.org/jasonb857/win-64
                          https://conda.anaconda.org/jasonb857/noarch
                          https://conda.anaconda.org/mirror-test/win-64
                          https://conda.anaconda.org/mirror-test/noarch
                          https://conda.anaconda.org/prometeia/win-64
                          https://conda.anaconda.org/prometeia/noarch
                          https://conda.anaconda.org/cdat-forge/win-64
                          https://conda.anaconda.org/cdat-forge/noarch
                          https://repo.anaconda.com/pkgs/main/win-64
                          https://repo.anaconda.com/pkgs/main/noarch
                          https://repo.anaconda.com/pkgs/r/win-64
                          https://repo.anaconda.com/pkgs/r/noarch
                          https://repo.anaconda.com/pkgs/msys2/win-64
                          https://repo.anaconda.com/pkgs/msys2/noarch
          package cache : E:\devtools\sdk\conda\miniconda4.11.0\pkgs
                          C:\Users\nigre\.conda\pkgs
                          C:\Users\nigre\AppData\Local\conda\conda\pkgs
       envs directories : E:\devtools\sdk\conda\miniconda4.11.0\envs
                          C:\Users\nigre\.conda\envs
                          C:\Users\nigre\AppData\Local\conda\conda\envs
               platform : win-64
             user-agent : conda/4.12.0 requests/2.27.1 CPython/3.9.7 Windows/10 Windows/10.0.19044
          administrator : False
             netrc file : None
           offline mode : False


An unexpected error has occurred. Conda has prepared the above report.

If submitted, this report will be used by core maintainers to improve
future releases of conda.
Would you like conda to send this report to the core maintainers?



No report sent. To permanently opt-out, use

    $ conda config --set report_errors false


Invoke-Expression : 所在位置 行:1 字符: 2
+ [y/N]:
+  ~
属性或类型文本末尾缺少 ]。
所在位置 行:1 字符: 4
+ [y/N]:
+    ~
必须在“/”运算符后面提供一个值表达式。
所在位置 行:1 字符: 4
+ [y/N]:
+    ~~~
表达式或语句中包含意外的标记“N]:”。
所在位置 E:\devtools\sdk\conda\miniconda4.11.0\shell\condabin\Conda.psm1:107 字符: 9
+         Invoke-Expression -Command $activateCommand;
+         ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ParserError: (:) [Invoke-Expression], ParseException
    + FullyQualifiedErrorId : EndSquareBracketExpectedAtEndOfAttribute,Microsoft.PowerShell.Commands.InvokeExpressionCommand

```

#### 解决方案

- 可能是因为没加这个几个环境变量。
- 为什么说可能，因为加上后没有解决问题，但是重启电脑后发现问题解决了。
- 也可能是要删除，注册表 `计算机\HKEY_CURRENT_USER\Software\Microsoft\Command Processor` 路径下的 `AutoRun`。

```yaml
%CONDA_HOME%\Library\bin
%CONDA_HOME%\Library\mingw-w64\bin
%CONDA_HOME%\Library\usr\bin
```

## Conda 官方包

https://anaconda.org/
