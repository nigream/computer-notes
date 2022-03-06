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



