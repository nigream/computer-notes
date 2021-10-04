## 一、开发工具概述

>  集成开发环境IDE（Integrated Development Environment），是一种专门用来提高Java开发效率的软件。

- 免费的IDE当中：Eclipse
- 收费的IDE当中：IntelliJ IDEA
- 免费+收费所有的IDE当中：全世界用得最多的就是IntelliJ IDEA

## 二、软件安装

自行百度

## 三、软件使用

### 3.1 首次启用

① 选择【不导入配置】。

![1581674463620](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314011927.png)

② 个人配置页面，可以不设置，直接关闭设置页面。

![1581674648234](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314011930.png)

③ 点击【创建新项目】。

![1581764099139](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012132.png)

④ 选择【空项目】。

![1581764164325](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314011934.png)

⑤ 填写【项目名】和【项目位置】。

![1581775699470](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314011938.png)

⑥ 选择【模块】，点击【添加】按钮，再点击【New Module】。

![1581777070827](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314011941.png)

⑦ 选择【Java模块】，并选择需要用到的JDK版本，然后点击【下一步】。

![1581777412550](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314011943.png)

⑧ 填写【模块名】，选择【内容根】和【模块文件位置】，最后点击【完成】。

![1581777766933](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314011945.png)

⑨ 最后点击【确定】

### 3.2 创建包和类

① 右击src文件夹，依次点击【新建】-->【包】。

![1581777991158](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314011948.png)

② 输入【包名】，点击【确认】，这里要求包名只用英文小写、数字和英文句点。这里一般使用公司域名把后缀提前，然后再加上一些有层级关系的包名。（注意：这里的英文句点是对包进行了层级划分，即com>nigream>test01层层深入）

![1581778590060](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314011953.png)

③ 右击包名，依次点击【新建】-->【Java类】。

![1581778900093](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314011956.png)

④ 填写【类名】，点击【确定】。（注意：最好使用驼峰原则，即每个单词首字母都大写）

![1581778953440](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012001.png)

⑤ 输入代码，idea不需要保存。

⑥ 右击程序空白处，点击【运行】即可运行。（class文件全都在项目根目录下的out目录下生成）

![1581779193971](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012003.png)

### 3.3 项目目录与结构

① 项目根目录下，`.idea` 目录和我们开发无关，是IDEA工具自己使用的；`demo01` 和 `demo02` 表示项目下的两个模块； `out` 目录储存所有模块下Java文件生成的 `.class` 文件。

![1581781122622](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012006.png)

② 模块目录下： `src` 目录存储我们编写的 `.java` 源文件， `.iml`  和我们开发无关，是IDEA工具自己使用的。

![1581781301713](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012008.png)

③ 如下是项目结构示意图：

![02-IDEA的项目结构](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012010.png)

### 3.4 基本配置

① 点击【文件】目录下的【设置】。

![](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012012.png)

② 选择【编辑器】目录下的【Font】，可以调整字体、大小等属性（如果有多项配置需要调整，记得每调整一项配置，就点击右下角的【应用】，防止之后忘了保存）。

![1581779547521](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012015.png)

③ 点击【快捷键】配置，点击上方的【小齿轮】，点击【重复】（这里可以复制一份快捷键配置，在这份配置上再进行修改，这样就不会破坏原有的快捷键配置）

![1581779883713](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012019.png)

④ 依次点击【主菜单】-->【代码】-->【补全】-->【基本】，双击【基本】这一行，点击【Add Keyboard Shortcut】，同时按住【Alt】和【/】来设置，以避免原来的【Ctrl+空格】与输入法切换的快捷键产生冲突。（该配置可以在按下快捷键时自动补全代码）

![1581780444119](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012024.png)

> 常用代码补全有：psvm-->生成main函数；sout-->生成输出语句；变量/常量/数组名称.fori-->生成for循环语句。

### 3.5 快捷键

| 快捷键                | 功能                                   |
| --------------------- | -------------------------------------- |
| `Alt+Enter`           | 导入包，自动修正代码                   |
| `Ctrl+Y`              | 删除光标所在行                         |
| `Ctrl+D`              | 复制光标所在行的内容，插入光标位置下面 |
| `Ctrl+Alt+L`          | 格式化代码                             |
| `Ctrl+/`              | 单行注释                               |
| `Ctrl+Shift+/`        | 选中代码注释，多行注释，再按取消注释   |
| `Alt+Ins`             | 自动生成代码，toString，get，set等方法 |
| `Alt+Shift+ 上下箭头` | 移动当前代码行                         |

### 3.6 关闭与打开项目

① 点击【文件】目录下的【关闭项目】，将项目关闭。

![1581782022537](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012029.png)

② 左侧是曾经打开过的项目，可以直接点击打开，也可点击【打开】按钮，选择相应的目录打开项目。

![1581782099405](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012034.png)

### 3.7 删除与导入项目

① 右击想要删除的模块，点击【Remove Module】（这样只是把该模块从项目中移除，并没有彻底删除该模块，在本地文件中仍然可以找到）。

![1581782228948](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012039.png)

② 点击【文件】目录下的【项目结构】。

![1581782417738](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012042.png)

③ 点击【添加】按钮，点击【Import Module】，选择外部模块地址，一路点【下一步】即可。

![1581782489113](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012044.png)

![1581778291466](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314012055.png)