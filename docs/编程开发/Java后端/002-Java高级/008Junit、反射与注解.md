## 一、Junit单元测试

### 1.1 黑盒与白盒

- 黑盒测试：不需要写代码，给输入值，看程序是否能够输出期望的值。
- 白盒测试：需要写代码的。关注程序具体的执行流程。

### 1.2 junit使用步骤

① 定义一个测试类(测试用例)
* 测试类名：被测试的类名Test		CalculatorTest
* 包名：xxx.xxx.xx.test		cn.itcast.test

② 定义测试方法：可以独立运行
* 方法名：test测试的方法名		testAdd()  
* 返回值：void
* 参数列表：空参

③ 给方法加@Test

④ 导入junit依赖环境

* 结果为红色：失败
* 结果为绿色：成功

一般我们会使用断言操作来处理结果：Assert.assertEquals(期望的结果,运算的结果);

### 1.3 Before和After

@Before：修饰的方法会在测试方法之前被自动执行

@After：修饰的方法会在测试方法执行之后自动被执行

```java
public class CalculatorTest {
    /**
     * 初始化方法：
     *  用于资源申请，所有测试方法在执行之前都会先执行该方法
     */
    @Before
    public void init(){
        System.out.println("init...");
    }

    /**
     * 释放资源方法：
     *  在所有测试方法执行完后，都会自动执行该方法
     */
    @After
    public void close(){
        System.out.println("close...");
    }


    /**
     * 测试add方法
     */
    @Test
    public void testAdd(){
       // System.out.println("我被执行了");
        //1.创建计算器对象
        System.out.println("testAdd...");
        Calculator c  = new Calculator();
        //2.调用add方法
        int result = c.add(1, 2);
        //System.out.println(result);

        //3.断言  我断言这个结果是3
        Assert.assertEquals(3,result);

    }

    @Test
    public void testSub(){
        //1.创建计算器对象
        Calculator c  = new Calculator();
        int result = c.sub(1, 2);
        System.out.println("testSub....");
        Assert.assertEquals(-1,result);
    }
}
```

## 二、反射

> 框架设计的灵魂

### 2.1 概念

- 框架：半成品软件。可以在框架的基础上进行软件开发，简化编码。

- 反射：将类的各个组成部分封装为其他对象，这就是反射机制

- 好处：

  ① 可以在程序运行过程中，操作这些对象。

  ② 可以解耦，提高程序的可扩展性。

![image-20210314021355736](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/进阶/20210314021358.png)

### 2.2 获取Class对象

```java
1. Class.forName("全类名")：将字节码文件加载进内存，返回Class对象
   - 多用于配置文件，将类名定义在配置文件中。读取文件，加载类
2. 类名.class：通过类名的属性class获取
   - 多用于参数的传递
3. 对象.getClass()：getClass()方法在Object类中定义着。
   - 多用于对象的获取字节码的方式
```

```java
public class ReflectDemo1 {
    public static void main(String[] args) throws Exception {

        //1.Class.forName("全类名")
        Class cls1 = Class.forName("cn.itcast.domain.Person");
        System.out.println(cls1);
        //2.类名.class
        Class cls2 = Person.class;
        System.out.println(cls2);
        //3.对象.getClass()
        Person p = new Person();
        Class cls3 = p.getClass();
        System.out.println(cls3);

        //== 比较三个对象
        System.out.println(cls1 == cls2);//true
        System.out.println(cls1 == cls3);//true

        Class c = Student.class;
        System.out.println(c == cls1);//false
    }
}
```

**结论：** 同一个字节码文件(*.class)在一次程序运行过程中，只会被加载一次，不论通过哪一种方式获取的Class对象都是同一个。

### 2.3 Class对象获取功能

#### 2.3.1 获取成员变量们

```java
Field[] getFields()           获取所有public修饰的成员变量
Field getField(String name)   获取指定名称的 public修饰的成员变量
Field[] getDeclaredFields()   获取所有的成员变量，不考虑修饰符
Field getDeclaredField(String name)  获取指定名称的变量，不考虑修饰符
```

**Field类：成员变量封装成的类**

```java
1. 设置值
   - void set(Object obj, Object value)  
2. 获取值
   - get(Object obj) 
3. 忽略访问权限修饰符的安全检查：通过Field对象的get方法获取非public成员变量会报错，所以需要使用这个语句来忽略安全检查。
   - setAccessible(true)：暴力反射
```

```java
public class ReflectDemo2 {
    public static void main(String[] args) throws Exception {

        //0.获取Person的Class对象
        Class personClass = Person.class;
        //1.Field[] getFields()获取所有public修饰的成员变量
        Field[] fields = personClass.getFields();
        for (Field field : fields) {
            System.out.println(field);
        }

        System.out.println("------------");
        //2.Field getField(String name)
        Field a = personClass.getField("a");
        //获取成员变量a 的值
        Person p = new Person();
        Object value = a.get(p);
        System.out.println(value);
        //设置a的值
        a.set(p,"张三");
        System.out.println(p);

        System.out.println("===================");

        //Field[] getDeclaredFields()：获取所有的成员变量，不考虑修饰符
        Field[] declaredFields = personClass.getDeclaredFields();
        for (Field declaredField : declaredFields) {
            System.out.println(declaredField);
        }
        //Field getDeclaredField(String name)
        Field d = personClass.getDeclaredField("d");
        //忽略访问权限修饰符的安全检查
        d.setAccessible(true);//暴力反射
        Object value2 = d.get(p);
        System.out.println(value2);
    }
}
```

#### 2.3.2 获取构造方法们

```java
Constructor<?>[] getConstructors()  
Constructor<T> getConstructor(类<?>... parameterTypes)  
Constructor<T> getDeclaredConstructor(类<?>... parameterTypes)  
Constructor<?>[] getDeclaredConstructors()  
```

**Constructor类：构造方法封装成的类**

```java
1. 创建对象：
	- T newInstance(Object... initargs)  
 	- 如果使用空参数构造方法创建对象，操作可以简化：Class对象的newInstance方法
2. 访问非public构造方法也可以使用暴力反射
 	- setAccessible(true)
```

```java
public class ReflectDemo3 {
    public static void main(String[] args) throws Exception {

        //0.获取Person的Class对象
        Class personClass = Person.class;
        //Constructor<T> getConstructor(类<?>... parameterTypes)
        Constructor constructor = personClass.getConstructor(String.class, int.class);
        System.out.println(constructor);
        //创建对象
        Object person = constructor.newInstance("张三", 23);
        System.out.println(person);
        System.out.println("----------");
        Constructor constructor1 = personClass.getConstructor();
        System.out.println(constructor1);
        //创建对象
        Object person1 = constructor1.newInstance();
        System.out.println(person1);

        Object o = personClass.newInstance();
        System.out.println(o);
        //constructor1.setAccessible(true);
    }
}
```

#### 2.3.3 获取成员方法们

```java
Method[] getMethods()  
Method getMethod(String name, 类<?>... parameterTypes)  
Method[] getDeclaredMethods()  
Method getDeclaredMethod(String name, 类<?>... parameterTypes)  
```

**Method类：方法对象封装成的类**

```java
1. 执行方法
	- Object invoke(Object obj, Object... args)     
2. 获取方法名称
	- String getName:获取方法名
3. 暴力反射
	- setAccessible(true)
```

```java
public class ReflectDemo4 {
    public static void main(String[] args) throws Exception {
        //0.获取Person的Class对象
        Class personClass = Person.class;
        //获取指定名称的方法
        Method eat_method = personClass.getMethod("eat");
        Person p = new Person();
        //执行方法
        eat_method.invoke(p);

        Method eat_method2 = personClass.getMethod("eat", String.class);
        //执行方法
        eat_method2.invoke(p,"饭");

        System.out.println("-----------------");

        //获取所有public修饰的方法
        Method[] methods = personClass.getMethods();
        for (Method method : methods) {
            System.out.println(method);
            String name = method.getName();
            System.out.println(name);
            //method.setAccessible(true);
        }
    }
}
```

#### 2.3.4 获取全类名	

```java
String getName()  
```

```java
//获取类名
String className = personClass.getName();
System.out.println(className);//cn.itcast.domain.Person
```

### 2.4 小案例

**需求：** 写一个"框架"，不能改变该类的任何代码的前提下，可以帮我们创建任意类的对象，并且执行其中任意方法。（使用 **配置文件** 与 **反射** ）

**① 步骤**

```java
1. 将需要创建的对象的全类名和需要执行的方法定义在配置文件中
2. 在程序中加载读取配置文件
3. 使用反射技术来加载类文件进内存
4. 创建对象
5. 执行方法
```

**② 代码**

```java
className = cn.itcast.domain.Student
methodName = sleep
```

```java
public class ReflectTest {
    public static void main(String[] args) throws Exception {
        //可以创建任意类的对象，可以执行任意方法
        //1.加载配置文件
        //1.1创建Properties对象
        Properties pro = new Properties();
        //1.2加载配置文件，转换为一个集合
        //1.2.1获取class目录下的配置文件
        ClassLoader classLoader = ReflectTest.class.getClassLoader();
        InputStream is = classLoader.getResourceAsStream("pro.properties");
        pro.load(is);

        //2.获取配置文件中定义的数据
        String className = pro.getProperty("className");
        String methodName = pro.getProperty("methodName");

        //3.加载该类进内存
        Class cls = Class.forName(className);
        //4.创建对象
        Object obj = cls.newInstance();
        //5.获取方法对象
        Method method = cls.getMethod(methodName);
        //6.执行方法
        method.invoke(obj);
    }
}
```

## 三、注解

### 3.1 概念

* 定义：注解（Annotation），也叫元数据。一种代码级别的说明。它是JDK1.5及以后版本引入的一个特性，与类、接口、枚举是在同一个层次。它可以声明在包、类、字段、方法、局部变量、方法参数等的前面，用来对这些元素进行说明，注释。

```java
- JDK1.5之后的新特性
- 说明程序的
- 使用注解：@注解名称
```

### 3.2  作用分类

```java
1. 编写文档：通过代码里标识的注解生成文档【生成文档doc文档】
2. 代码分析：通过代码里标识的注解对代码进行分析【使用反射】
3. 编译检查：通过代码里标识的注解让编译器能够实现基本的编译检查【Override】
```

### 3.3 内置注解

```java
* @Override	：检测被该注解标注的方法是否是继承自父类(接口)的
* @Deprecated：该注解标注的内容，表示已过时
* @SuppressWarnings：压制警告(一般传递参数all  @SuppressWarnings("all"))
    - 写在类上，压制类的警告
    - 写在方法上，压制方法的警告
```

### 3.4 自定义注解

#### 3.4.1 格式

```java
元注解
public @interface 注解名称{
	属性列表;
}
```

本质：注解本质上就是一个接口，该接口默认继承Annotation接口

```java
public interface MyAnno extends java.lang.annotation.Annotation {}
```

#### 3.4.2 属性

 属性：接口中的抽象方法（接口能定义什么，注解就能定义什么）
  * 属性的返回值类型只有下列取值：

```java
- 基本数据类型
- String
- 枚举
- 注解
- 以上类型的数组
```

```java
public @interface MyAnno {
    int value();
    Person per();//这个是枚举类型
    MyAnno2 anno2();
    String[] strs();
    String show2();
}
```

```java
public enum Person {
    P1,P2;
}
```

- 定义了属性，在使用时需要给属性赋值

① 如果定义属性时，使用default关键字给属性默认初始化值，则使用注解时，可以不进行属性的赋值。

② 如果只有一个属性需要赋值，并且属性的名称是value，则value可以省略，直接定义值即可。

```java
public @interface MyAnno {
    int value();
    String name() default "张三";
}
```

③ 数组赋值时，值使用{}包裹。如果数组中只有一个值，则{}可以省略

```java
@MyAnno(value=12,per = Person.P1,anno2 = @MyAnno2,strs="bbb")
public class Worker {
}
```

#### 3.4.3 元注解

> **元注解：** 用于描述注解的注解。

```java
@Target：描述注解能够作用的位置
- 属性枚举类ElementType的取值：（可以有多个取值，用大括号）
  - TYPE：可以作用于类上
  - METHOD：可以作用于方法上
  - FIELD：可以作用于成员变量上
  
@Retention：描述注解被保留的阶段
- 属性枚举类RetentionPolicy的取值：
  - SOURCE：不会保留到class字节码文件中
  - CLASS：当前被描述的注解，会保留到class字节码文件中，不会被JVM读取到
  - RUNTIME：当前被描述的注解，会保留到class字节码文件中，并被JVM读取到（自定义注解通常用这个值）
  
@Documented：描述注解是否被抽取到api文档中（不要赋值）

@Inherited：描述注解是否被子类继承（不要赋值）
```

### 3.5 解析注解

>  获取注解中定义的属性值

**步骤：**

① 获取注解定义的位置的对象（Class,Method,Field）

② 获取指定的注解

③ 调用注解中的抽象方法获取配置的属性值

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Pro {

    String className();
    String methodName();
}
```

```java
@Pro(className = "cn.itcast.annotation.Demo1",methodName = "show")
public class ReflectTest {
    public static void main(String[] args) throws Exception {
        //1.解析注解
        //1.1获取该类的字节码文件对象
        Class<ReflectTest> reflectTestClass = ReflectTest.class;
        //2.获取上边的注解对象
        Pro an = reflectTestClass.getAnnotation(Pro.class);
        /*如下所示，上面这行代码相当于在内存中生成了一个该注解接口的子类实现对象
            public class ProImpl implements Pro{
                public String className(){
                    return "cn.itcast.annotation.Demo1";
                }
                public String methodName(){
                    return "show";
                }

            }
		*/
        //3.调用注解对象中定义的抽象方法，获取返回值
        String className = an.className();
        String methodName = an.methodName();
        System.out.println(className);
        System.out.println(methodName);
        //3.加载该类进内存
        Class cls = Class.forName(className);
        //4.创建对象
        Object obj = cls.newInstance();
        //5.获取方法对象
        Method method = cls.getMethod(methodName);
        //6.执行方法
        method.invoke(obj);
    }
}
```

### 3.6 案例

**简单的测试框架：**  当主方法执行后，会自动运行被检测的所有方法(加了Check注解的方法)，判断方法是否有异常，记录到文件中

```java
public class Calculator {
    //加法
    @Check
    public void add(){
        String str = null;
        str.toString();
        System.out.println("1 + 0 =" + (1 + 0));
    }
    //减法
    @Check
    public void sub(){
        System.out.println("1 - 0 =" + (1 - 0));
    }
    //乘法
    @Check
    public void mul(){
        System.out.println("1 * 0 =" + (1 * 0));
    }
    //除法
    @Check
    public void div(){
        System.out.println("1 / 0 =" + (1 / 0));
    }


    public void show(){
        System.out.println("永无bug...");
    }
}
```

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Check {
}
```

```java
public class TestCheck {
    public static void main(String[] args) throws IOException {
        //1.创建计算器对象
        Calculator c = new Calculator();
        //2.获取字节码文件对象
        Class cls = c.getClass();
        //3.获取所有方法
        Method[] methods = cls.getMethods();

        int number = 0;//出现异常的次数
        BufferedWriter bw = new BufferedWriter(new FileWriter("bug.txt"));


        for (Method method : methods) {
            //4.判断方法上是否有Check注解
            if(method.isAnnotationPresent(Check.class)){
                //5.有，执行
                try {
                    method.invoke(c);
                } catch (Exception e) {
                    //6.捕获异常

                    //记录到文件中
                    number ++;

                    bw.write(method.getName()+ " 方法出异常了");
                    bw.newLine();
                    bw.write("异常的名称:" + e.getCause().getClass().getSimpleName());
                    bw.newLine();
                    bw.write("异常的原因:"+e.getCause().getMessage());
                    bw.newLine();
                    bw.write("--------------------------");
                    bw.newLine();

                }
            }
        }

        bw.write("本次测试一共出现 "+number+" 次异常");

        bw.flush();
        bw.close();
    }
}
```

- 以后大多数时候，我们会使用注解，而不是自定义注解
- 注解给 **编译器** 和 **解析程序**  （如上面的TestCheck）使用
- 注解不是程序的一部分，可以理解为注解就是一个标签





