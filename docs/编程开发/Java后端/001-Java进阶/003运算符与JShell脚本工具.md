## 一、运算符

### 1.1 定义

- **运算符：** 进行特定操作的符号，例如：`+` 。
- **表达式：** 用运算符连接起来的式子叫做表达式。例如：`20 + 5` 。
- 常量之间、变量之间、常量与变量之间都可以进行运算。

### 1.2 算数运算符

#### 1.2.1 分类

| 算数运算符 | 可以进行的运算                                     |
| ---------- | -------------------------------------------------- |
| `+`        | 加法运算，字符串连接运算                           |
| `-`        | 减法运算                                           |
| `*`        | 乘法运算                                           |
| `/`        | 除法运算（不管计算结果有没有余数，结果只取**商**） |
| `%`        | 取模运算（两个数字相除取余数）                     |
| `++`、`--` | 自增自减运算                                       |

> **注意：** Java中取模运算可以用于小数，但是计算得到的值不再是精确值，且这种计算没有多大意义。

- 一旦运算当中有不同类型的数据，那么结果将会是数据类型范围大的那种。

```java
public class Demo04Operator {
	public static void main(String[] args) {
		// 两个常量之间可以进行数学运算
		System.out.println(20 + 30);
		
		// 两个变量之间也可以进行数学运算
		int a = 20;
		int b = 30;
		System.out.println(a - b); // -10
		
		// 变量和常量之间可以混合使用
		System.out.println(a * 10); // 200
		
		int x = 10;
		int y = 3;
		
		int result1 = x / y;
		System.out.println(result1); // 3
		
		int result2 = x % y;
		System.out.println(result2); // 余数，模，1
		
		// int + double --> double + double --> double
		double result3 = x + 2.5;
		System.out.println(result3); // 12.5
	}
}
```

#### 1.2.2 加法的多种用法

① 对于数值来说，那就是加法。

② 对于字符char类型来说，在计算之前，char会被提升成为int（参照编码表），然后再计算。

③ 对于字符串来说，加号代表字符串连接操作（任何数据类型和字符串进行连接的时候，结果都会变成字符串）。

```java
public class Demo05Plus {
	public static void main(String[] args) {
		// 字符串类型的变量基本使用
		// 数据类型 变量名称 = 数据值;
		String str1 = "Hello";
		System.out.println(str1); // Hello
		
		System.out.println("Hello" + "World"); // HelloWorld
		
		String str2 = "Java";
		// String + int --> String
		System.out.println(str2 + 20); // Java20
		
		// 优先级问题
		// String + int + int
		// String		+ int
		// String
		System.out.println(str2 + 20 + 30); // Java2030
		
		System.out.println(str2 + (20 + 30)); // Java50
	}
}
```

#### 1.2.3 自增自减运算符

- **基本含义：** 让一个变量加1或减1。
- **使用格式：** 写在变量名称之前，或者写在变量名称之后，例如 `++num` ，也可以`num++`。
- **使用方式：**

```
单独使用：不和其他任何操作混合，自己独立成为一个步骤。
混合使用：和其他操作混合，例如与赋值混合，或者与打印操作混合等。
```

- **使用区别：**

```
1. 在单独使用的时候，前++和后++没有任何区别。也就是：++num;和num++;是完全一样的。
2. 在混合的时候，有【重大区别】
    A. 如果是【前++】，那么变量【立刻马上+1】，然后拿着结果进行使用。	【先加后用】
    B. 如果是【后++】，那么首先使用变量本来的数值，【然后再让变量+1】。	【先用后加】
```

```java
public class Demo06Operator {
	public static void main(String[] args) {
		int num1 = 10;
		System.out.println(num1); // 10
		++num1; // 单独使用，前++
		System.out.println(num1); // 11
		num1++; // 单独使用，后++
		System.out.println(num1); // 12
		System.out.println("=================");
		
		// 与打印操作混合的时候
		int num2 = 20;
		// 混合使用，先++，变量立刻马上变成21，然后打印结果21
		System.out.println(++num2); // 21
		System.out.println(num2); // 21
		System.out.println("=================");
		
		int num3 = 30;
		// 混合使用，后++，首先使用变量本来的30，然后再让变量+1得到31
		System.out.println(num3++); // 30
		System.out.println(num3); // 31
		System.out.println("=================");
		
		int num4 = 40;
		// 和赋值操作混合
		int result1 = --num4; // 混合使用，前--，变量立刻马上-1变成39，然后将结果39交给result1变量
		System.out.println(result1); // 39
		System.out.println(num4); // 39
		System.out.println("=================");
		
		int num5 = 50;
		// 混合使用，后--，首先把本来的数字50交给result2，然后我自己再-1变成49
		int result2 = num5--;
		System.out.println(result2); // 50
		System.out.println(num5); // 49
		System.out.println("=================");
		
		int x = 10;
		int y = 20;
		// 11 + 20 = 31
		int result3 = ++x + y--;
		System.out.println(result3); // 31
		System.out.println(x); // 11
		System.out.println(y); // 19
		
		// 30++; // 错误写法！常量不可以使用++或者--
	}
}
```

> **注意：** 只有变量才能使用自增、自减运算符。常量不可发生改变，所以不能用。

### 1.3 赋值运算符

> 赋值运算符分为`基本赋值运算符`和`复合赋值运算符`

① **基本赋值运算符：** 就是一个等号“=”，代表将右侧的数据交给左侧的变量。例如：

```java
int a = 30;
```

② **复合赋值运算符**

| 复合赋值运算符 | 含义   | 例子   | 相当于    |
| -------------- | ------ | ------ | --------- |
| `+=`           | 加等于 | a += 3 | a = a + 3 |
| `-=`           | 减等于 | b -= 4 | b = b - 4 |
| `*=`           | 乘等于 | c *= 5 | c = c * 5 |
| `/=`           | 除等于 | d /= 6 | d = d / 6 |
| `%=`           | 取模等 | e %= 7 | e = e % 7 |

- **注意事项**

  ① 只有变量才能使用赋值运算符，常量不能进行赋值。

  ② 复合赋值运算符其中隐含了一个强制类型转换（不管变量右端计算得到的值是否超出左边数据类型的范围，都会进行强制转换）。

```java
public class Demo07Operator {
	public static void main(String[] args) {
		int a = 10;
		// 按照公式进行翻译：a = a + 5
		// a = 10 + 5;
		// a = 15;
		// a本来是10，现在重新赋值得到15
		a += 5; 
		System.out.println(a); // 15
		
		int x = 10;
		// x = x % 3;
		// x = 10 % 3;
		// x = 1;
		// x本来是10，现在重新赋值得到1
		x %= 3;
		System.out.println(x); // 1
		
		// 50 = 30; // 常量不能进行赋值，不能写在赋值运算符的左边。错误写法！
		
		byte num = 30;
		// num = num + 5;
		// num = byte + int
		// num = int + int
		// num = int
		// num = (byte) int
		num += 5;
		System.out.println(num); // 35
	}
}
```

### 1.4 比较运算符

| 比较运算符 | 含义                                                         |
| ---------- | ------------------------------------------------------------ |
| `==`       | 比较符号两边数据是否相等，相等结果是true。                   |
| `<`        | 比较符号左边的数据是否小于右边的数据，如果小于结果是true。   |
| `<`        | 比较符号左边的数据是否大于右边的数据，如果大于结果是true。   |
| `<=`       | 比较符号左边的数据是否小于或者等于右边的数据，如果小于结果是true。 |
| `>=`       | 比较符号左边的数据是否大于或者等于右边的数据，如果小于结果是true。 |
| `!=`       | 不等于符号 ，如果符号两边的数据不相等，结果是true。          |

- **注意事项**

  ① 比较运算符的结果一定是一个boolean值，成立就是true，不成立就是false。

  ② 如果进行多次判断，不能连着写。数学当中的写法：1 < x < 3；程序当中【不允许】这种写法。

```java
public class Demo08Operator {
	public static void main(String[] args) {
		System.out.println(10 > 5); // true
		int num1 = 10;
		int num2 = 12;
		System.out.println(num1 < num2); // true
		System.out.println(num2 >= 100); // false
		System.out.println(num2 <= 100); // true
		System.out.println(num2 <= 12); // true
		System.out.println("===============");
		
		System.out.println(10 == 10); // true
		System.out.println(20 != 25); // true
		System.out.println(20 != 20); // false
		
		int x = 2;
		// System.out.println(1 < x < 3); // 错误写法！编译报错！不能连着写。
	}
}
```

### 1.5 逻辑运算符

| 逻辑运算符      | 含义                                                         |
| --------------- | ------------------------------------------------------------ |
| `&&` 与（并且） | 全都是true，才是true；否则就是false<br/>短路特点：符号左边是false，右边不再运算 |
| `||` 或（或者） | 至少一个是true，就是true；全都是false，才是false<br/>短路特点： 符号左边是true，右边不再运算 |
| `!` 非（取反）  | 本来是true，变成false；本来是false，变成true                 |

- **注意事项**

  1. 逻辑运算符只能用于boolean值。
  2. 与、或需要左右各自有一个boolean值，但是取反只要有唯一的一个boolean值即可。
  3. 与、或两种运算符，如果有多个条件，可以连续写。

  > 两个条件：条件A && 条件B
  > 多个条件：条件A && 条件B && 条件C

> 对于1 < x < 3的情况，应该拆成两个部分，然后使用与运算符连接起来：`int x = 2;`
> `1 < x && x < 3`

```java
public class Demo09Logic {
	public static void main(String[] args) {
		System.out.println(true && false); // false
		// true && true --> true
		System.out.println(3 < 4 && 10 > 5); // true
		System.out.println("============");
		
		System.out.println(true || false); // true
		System.out.println(true || true); // true
		System.out.println(false || false); // false
		System.out.println("============");
		
		System.out.println(true); // true
		System.out.println(!true); // false	
		System.out.println("============");
		
		int a = 10;
		// false && ...
		System.out.println(3 > 4 && ++a < 100); // false
		System.out.println(a); // 10
		System.out.println("============");
		
		int b = 20;
		// true || ...
		System.out.println(3 < 4 || ++b < 100); // true
		System.out.println(b); // 20
	}
}
```

### 1.6 三元运算符

#### 1.6.1 运算符分类

**① 一元运算符：** 只需要一个数据就可以进行操作的运算符。例如：取反!、自增++、自减--

**② 二元运算符：** 需要两个数据才可以进行操作的运算符。例如：加法+、赋值=

**③ 三元运算符：** 需要三个数据才可以进行操作的运算符。

#### 1.6.2 三元运算符定义

- **格式**

```java
数据类型 变量名称 = 条件判断 ? 表达式A : 表达式B;
```

- **流程**

```java
首先判断条件是否成立：
	如果成立为true，那么将表达式A的值赋值给左侧的变量；
	如果不成立为false，那么将表达式B的值赋值给左侧的变量；
二者选其一。
```

- **注意事项**

```java
1. 必须同时保证表达式A和表达式B都符合左侧数据类型的要求（否则报错）。
2. 三元运算符的结果必须被使用（用于运算、赋值或打印）。
```

```java
public class Demo10Operator {
	public static void main(String[] args) {
		int a = 10;
		int b = 20;
		
		// 数据类型 变量名称 = 条件判断 ? 表达式A : 表达式B;
		// 判断a > b是否成立，如果成立将a的值赋值给max；如果不成立将b的值赋值给max。二者选其一
		int max = a > b ? a : b; // 最大值的变量
		System.out.println("最大值：" + max); // 20
		
		// int result = 3 > 4 ? 2.5 : 10; // 错误写法！
		
		System.out.println(a > b ? a : b); // 正确写法！
		
		// a > b ? a : b; // 错误写法！
	}
}
```

### 1.7 编译器对于运算的优化

① 对于byte/short/char三种类型来说，如果右侧赋值的数值没有超过范围，那么javac编译器将会自动隐含地为我们补上一个(byte)(short)(char)。

> 1. 如果没有超过左侧的范围，编译器补上强转。
> 2. 如果右侧超过了左侧范围，那么直接编译器报错。

```java
public class Demo12Notice {
	public static void main(String[] args) {
		// 右侧确实是一个int数字，但是没有超过左侧的范围，就是正确的。
		// int --> byte，不是自动类型转换
		byte num1 = /*(byte)*/ 30; // 右侧没有超过左侧的范围
		System.out.println(num1); // 30
		
		// byte num2 = 128; // 右侧超过了左侧的范围
		
		// int --> char，没有超过范围
		// 编译器将会自动补上一个隐含的(char)
		char zifu = /*(char)*/ 65;
		System.out.println(zifu); // A
	}
}
```

② 在给变量进行赋值的时候，如果右侧的表达式当中全都是常量，没有任何变量，那么编译器javac将会直接将若干个常量表达式计算得到结果。（即**编译器的常量优化**）

```java
short result = 5 + 8; // 等号右边全都是常量，没有任何变量参与运算编译之后，得到的.class字节码文件当中相当于【直接就是】：short result = 13;右侧的常量结果数值，没有超过左侧范围，所以正确。
```

> **注意：** 一旦表达式当中有变量参与，那么就不能进行这种优化了。

```java
public class Demo13Notice {
	public static void main(String[] args) {
		short num1 = 10; // 正确写法，右侧没有超过左侧的范围，
		
		short a = 5;
		short b = 8;
		// short + short --> int + int --> int
		// short result = a + b; // 错误写法！左侧需要是int类型
		
		// 右侧不用变量，而是采用常量，而且只有两个常量，没有别人
		short result = 5 + 8;
		System.out.println(result);
		
		//short result2 = 5 + a + 8; // 错误写法
	}
}
```



## 二、JShell脚本工具

当我们编写的代码非常少的时候，而又不愿意编写类，main方法，也不愿意去编译和运行，这个时候可以使用JShell工具。

- **启动JShell工具：** 在DOS命令行直接输入JShell命令（该exe文件在jdk9及以上版本的bin目录下）

![1580976827184](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/入门/20210314011340.png)

- **退出JShell：** 输入`/exit`，回车。

