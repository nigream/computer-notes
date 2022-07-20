# Python解释器

---

## 解释器的种类

- `CPython` : 官方的用 `C` 语言编写的版本。
- `Jython` : 用 `Java` 语言编写的可以运行在 `Java` 平台的版本。
- `IronPython` : 可以运行在 `.NET` 和 `Mono` 平台。
- `PyPy` : 用 `Python` 实现的，支持 `JIT` 即时编译。

## 使用解释器的方式

### 执行 python 源文件

- 直接编写文本文件，执行 Python 程序时，只需执行 `python xxx.py` (Python 2.x 及其以下版本) 或 `python3 xxx.py` (Python 3.x 版本)命令即可。

### Python Shell (交互模式)

即 直接在 `Python Shell` 中执行 Python 的代码。

- 命令行输入 `python` 即可进入 `Python Shell` 。
- 执行 `exit()` 或者 `ctrl + z` 即可退出 `Python  Shell`

### python -c command [arg] ...

- 执行 python 表达式，表达式建议用单引号引起来。

### python -m module [arg] ...

- 执行 模块 中的方法。

### IPython

## 交互模式

### _  字符

- 在交互模式中，使用 `_` 可以获取上一次输出的值。

  ```sh
  >>> tax = 12.5 / 100
  >>> price = 100.50
  >>> price * tax
  12.5625
  >>> price + _
  113.0625
  >>> round(_, 2)
  113.06
  ```

- 不要为 `_` 赋值，否则会创建新的局部变量，我们则无法访问原先作为内置变量的 `_` 。