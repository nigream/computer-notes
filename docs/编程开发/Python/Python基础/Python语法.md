# Python语法

---

## 运算符

### / 与 //

参考：https://docs.python.org/3/glossary.html#term-floor-division

- `/` 表示 **数学除法** 。 `-11 / 4 = -2.75 `
- `//` 表示 **向下取整** 。`-11 // 4 = -3 `

### **

- `**` 表示 **乘方** 。 `5 ** 2 = 25`

## 字符串

### 单引号与双引号

- **相同：** 特殊字符如 `\n` 在二者之中意义一样，都表示换行符。
- **不同：** 在 `'...'` 中可以直接使用 `"` ，但使用 `'` 时，必须用 `\'` ；在 `"..."` 中可以直接使用 `'` ，但使用 `"` 时，必须用 `\"`  。

### 原始字符串

- 在字符串前加字母 r，比如 `r'C:\some\name'` ，则表示这个字符串为原始字符串，里面的内容不会被认为是特殊字符。

### 三引号

- 使用 `"""....."""` / `'''.....'''` 可以输入多行字符串。
- 在每一行末尾会默认加一个换行符 `\n`  。
- 第一行 `"""` / `'''` 后面跟着的换行符，可以通过加 `\` 符号去掉。

```python
print("""\
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
""")
```

### 乘法与加法

```python
>>> 3 * 'un' + 'ium'
'unununium'
```

### 自动拼接

- 相邻的两个字符串会自动拼接。
- 仅对 **字面值** 有效，在 **变量与变量** 之间、 **变量与字面值** 之间无效，这两种情况使用 `+` 

```python
>>> 'Py' 'thon'
'Python'
```

```python
>>> text = ('Put several strings within parentheses '
...         'to have them joined together.')
>>> text
'Put several strings within parentheses to have them joined together.'
```

### 索引

- 字符串支持 **索引** 。
- 索引支持 **负数** ，从右边开始计数， -1 表示最后一个字符（因为 -0 和 0 是一样的）。

```python
>>> word = 'Python'
>>> word[0]  # character in position 0
'P'
>>> word[5]  # character in position 5
'n'
>>> word[-1]  # last character
'n'
>>> word[-2]  # second-last character
'o'
>>> word[-6]
'P'
```

### 切片(slice)

- **左闭右开** 区间。

```python
>>> word = 'Python'
>>> word[0:2]  # characters from position 0 (included) to 2 (excluded)
'Py'
>>> word[2:5]  # characters from position 2 (included) to 5 (excluded)
'tho'
```

- 切片 左边的参数默认为 0 ，右边的参数默认为字符串的 size 。

```python
>>> word[:2]   # character from the beginning to position 2 (excluded)
'Py'
>>> word[4:]   # characters from position 4 (included) to the end
'on'
>>> word[-2:]  # characters from the second-last (included) to the end
'on'
```







---

**参考：**

1. https://docs.python.org/3/tutorial/index.html