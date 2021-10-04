## 一、File类

### 1.1 概述

java.io.file 类是文件和目录路径名的抽象表示，可以用于创建、删除、获取、遍历文件和目录，并且可以判断文件和目录是否存在、获取文件的大小。

File类是一个与系统无关的类，任何操作系统都可以使用这个类中的方法。

> file、directory、path

### 1.2 静态成员变量

```java
static String pathSeparator 与系统有关的路径分隔符，为了方便，它被表示为一个字符串。
static char pathSeparatorChar 与系统有关的路径分隔符。

static String separator 与系统有关的默认名称分隔符，为了方便，它被表示为一个字符串。
static char separatorChar 与系统有关的默认名称分隔符。
```
pathSeparator：路径分割符（windows：分号      Linux：冒号）

separator：文件分割符（windows：反斜杠（\）   Linux：正斜杠（/））

pathSeparator和separator分别在pathSeparatorChar和separatorChar的返回值基础上加个``""``变成字符串。

```java
操作路径:路径不能写死了
C:\develop\a\a.txt  windows
C:/develop/a/a.txt  linux
"C:"+File.separator+"develop"+File.separator+"a"+File.separator+"a.txt"
```
### 1.3 绝对与相对路径

绝对路径：是一个完整的路径（以盘符开始的路径）

相对路径：是一个简化的路径（“相对”指的是相对于当前项目的根目录）

**注意：**

① 路径不区分大小写

② 路径中的文件名称分隔符windows使用反斜杠，反斜杠也是转义字符，两个反斜杠代表一个普通的反斜杠。

### 1.4 构造方法

```java
File(String pathname) 通过给定路径名字符串转换为抽象路径名来创建一个新的File实例。
String pathname:字符串的路径名称
- 路径可以是以文件结尾,也可以是以文件夹结尾
- 路径可以是相对路径,也可以是绝对路径
- 路径可以是存在,也可以是不存在
- 创建File对象,只是把字符串路径封装为File对象,不考虑路径的真假情况
```

```java
File(String parent, String child) 根据 parent 路径名字符串和 child 路径名字符串创建一个新 File 实例。
参数:把路径分成了两部分
String parent:父路径
String child:子路径
好处:
- 父路径和子路径,可以单独书写,使用起来非常灵活;父路径和子路径都可以变化
```

```java
File(File parent, String child) 根据 parent 抽象路径名和 child 路径名字符串创建一个新 File 实例。
参数:把路径分成了两部分
File parent:父路径
String child:子路径
好处:
- 父路径和子路径,可以单独书写,使用起来非常灵活;父路径和子路径都可以变化
- 父路径是File类型,可以使用File的方法对路径进行一些操作,再使用路径创建对象
```

### 1.5 常用方法

**(1) 获取方法**

```java
File类获取功能的方法
- public String getAbsolutePath()：返回此File的绝对路径名字符串。
    - 无论调用构造方法时使用的路径是绝对的还是相对的，它返回的都是绝对路径。
- public String getPath()：将此File转换为路径名字符串，即获取构造方法中传递的路径.
- public String getName()：返回由此File表示的文件或目录的名称，即结尾部分。
- public long length()：返回由此File表示的文件的长度，即构造方法发指定的文件的大小，以字节为单位。
    - 文件夹是没有大小概念的，不能获取文件夹的大小；如果构造方法中给出的路径不存在，那么length方法返回0。
```

**(2) 判断方法**

```java
File类判断功能的方法
- public boolean exists() ：此File表示的文件或目录是否实际存在，用于判断构造方法中的路径是否存在。
- public boolean isDirectory() ：此File表示的是否为目录，若路径不存在，则返回false。
- public boolean isFile() ：此File表示的是否为文件，若路径不存在，则返回false。
```

**(3) 创建和删除方法**

```java
public boolean createNewFile() ：当且仅当具有该名称的文件尚不存在时，创建一个新的空文件。
   
    public boolean createNewFile() throws IOException
    createNewFile声明抛出了IOException,我们调用这个方法,就必须的处理这个异常,要么throws,要么trycatch
```

创建文件的路径和名称在构造方法中给出 (构造方法的参数)

**返回值:布尔值**

- true:文件不存在,创建文件,返回true
- false:文件存在,不会创建,返回false

**注意:**

- 1.此方法只能创建文件,不能创建文件夹
- 2.创建文件的路径必须存在,否则会抛出异常

```java
public boolean mkdir() ：创建单级文件夹。
public boolean mkdirs() ：既可以创建单级文件夹，也可以创建多级文件夹。
```

创建文件的路径和名称在构造方法中给出。

- 文件夹不存在，创建文件夹，返回true；
- 文件夹存在，不会创建，返回false；
- 构造方法中给出的路径不存在，则返回false

**注意：**此方法只能创建文件夹，不能创建文件。

```java
public boolean delete() 删除由此File表示的文件或目录。
```

- true:文件/文件夹删除成功,返回true
- false:文件夹中有内容,不会删除返回false;构造方法中路径不存在false

注意：delete方法是直接在硬盘删除文件/文件夹,不走回收站,删除要谨慎

**(4) 目录遍历**

```java
public String list() 返回一个String数组，表示该File目录中的所有子文件和目录（包括隐藏文件和隐藏文件夹）。
public File[] listFiles() 返回一个File数组，表示该File目录中的所有的子文件或目录（包括隐藏文件和隐藏文件夹）。
```

list方法和listFiles方法遍历的是构造方法中给出的**目录**

- 如果构造方法中给出的目录的**路径不存在**,会抛出空指针异常
- 如果构造方法中给出的路径**不是一个目录**,也会抛出空指针异常

## 二、递归

### 2.1 概念

> 递归:方法自己调用自己

- **递归的分类:**
  - 直接递归：方法自身调用自己。
  - 间接递归：A方法调用B方法，B方法调用C方法，C方法调用A方法。
- **注意事项：**
  - 递归一定要有条件限定，保证递归能够停止下来，否则会发生栈内存溢出。
  - 在递归中虽然有限定条件，但是递归次数不能太多。否则也会发生栈内存溢出。
  - 构造方法,禁止递归

**使用前提：**当调用方法的时候，方法的主体不变，每次调用方法的参数不同，可以使用递归。

### 2.2 原理

![image-20210314021054100](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/进阶/20210314021057.png)

### 2.3 递归遍历文件

```java
public class Demo04Recurison {
    public static void main(String[] args) {
        File file = new File("c:\\abc");
        getAllFile(file);
    }

    /*
        定义一个方法,参数传递File类型的目录
        方法中对目录进行遍历
     */
    public static void getAllFile(File dir){
        System.out.println(dir);//打印被遍历的目录名称
        File[] files = dir.listFiles();
        for (File f : files) {
            //对遍历得到的File对象f进行判断,判断是否是文件夹
            if(f.isDirectory()){
                //f是一个文件夹,则继续遍历这个文件夹
                //我们发现getAllFile方法就是传递文件夹,遍历文件夹的方法
                //所以直接调用getAllFile方法即可:递归(自己调用自己)
                getAllFile(f);
            }else{
                //f是一个文件,直接打印即可
                System.out.println(f);
            }
        }
    }
}
```

```java
public class Demo05Recurison {
    public static void main(String[] args) {
        File file = new File("c:\\abc");
        getAllFile(file);
    }

    public static void getAllFile(File dir){
        File[] files = dir.listFiles();
        for (File f : files) {
            if(f.isDirectory()){
                getAllFile(f);
            }else{
                //f是一个文件,直接打印即可
                /*
                    c:\\abc\\abc.java
                    只要.java结尾的文件
                    1.把File对象f,转为字符串对象
                 */

                if(f.getName().toLowerCase().endsWith(".java")){
                    System.out.println(f);
                }
            }
        }
    }
}
```

## 三、文件过滤器

① ``File[] listFiles(FileFilter filter)`` 

java.io.FileFilter接口：用于抽象路径名（File对象）的过滤器。

- boolean accept(File pathname) 测试指定抽象路径名是否应该包含在某个路径名列表中。
- File pathname:使用ListFiles方法遍历目录,得到的每一个文件对象。

② ``File[] listFiles(FilenameFilter filter)`` 

java.io.FilenameFilter接口：实现此接口的类案例可用于过滤器文件名。

- boolean accept(File dir, String name) 测试指定文件是否应该包含在某一文件列表中。
- File dir:构造方法中传递的被遍历的目录。
- String name:使用ListFiles方法遍历目录,获取的每一个文件/文件夹的名称。

**注意：**两个过滤器接口是没有实现类的,需要我们自己写实现类,重写过滤的方法accept,在方法中自己定义过滤的规则

![image-20210314021120787](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/进阶/20210314021122.png)

```java
/*
    创建过滤器FileFilter的实现类,重写过滤方法accept,定义过滤规则
 */
public class FileFilterImpl implements FileFilter{
    @Override
    public boolean accept(File pathname) {
        /*
            过滤的规则:
            在accept方法中,判断File对象是否是以.java结尾
            是就返回true
            不是就返回false
         */
        //如果pathname是一个文件夹,返回true,继续遍历这个文件夹
        if(pathname.isDirectory()){
            return true;
        }

        return pathname.getName().toLowerCase().endsWith(".java");
    }
}
```

```java
public class Demo01Filter {
    public static void main(String[] args) {
        File file = new File("c:\\abc");
        getAllFile(file);
    }

    /*
        定义一个方法,参数传递File类型的目录
        方法中对目录进行遍历
     */
    public static void getAllFile(File dir){
        File[] files = dir.listFiles(new FileFilterImpl());//传递过滤器对象
        for (File f : files) {
            //对遍历得到的File对象f进行判断,判断是否是文件夹
            if(f.isDirectory()){
                //f是一个文件夹,则继续遍历这个文件夹
                //我们发现getAllFile方法就是传递文件夹,遍历文件夹的方法
                //所以直接调用getAllFile方法即可:递归(自己调用自己)
                getAllFile(f);
            }else{
                //f是一个文件,直接打印即可
                System.out.println(f);
            }
        }
    }
}
```

## 四、IO流

### 4.1 概念

![image-20210314021149406](E:\notes\java云图版\002进阶\005File类和IO流\img\image-20210314021149406.png)

### 4.2 读写原理

- java程序-->JVM(java虚拟机)-->OS(操作系统)-->OS调用写数据的方法-->把数据写入到文件中
- java程序-->JVM-->OS-->OS读取数据的方法-->读取文件

## 五、字节流

### 5.1 OutputStream

java.io.OutputStream：**字节输出流**，此抽象类是表示字节输出流的所有类的超类。定义了一些子类共性的成员方法：

```java
public void close() ：关闭此输出流并释放与此流相关联的任何系统资源。
public void flush() ：刷新此输出流并强制任何缓冲的输出字节被写出。
public void write(byte[] b)：将 b.length字节从指定的字节数组写入此输出流。
public void write(byte[] b, int off, int len) ：从指定的字节数组写入 len字节，从偏移量 off开始输出到此输出流。
public abstract void write(int b) ：将指定的字节输出流。
```

① ``public void write(byte[] b)`` 将 b.length字节从指定的字节数组写入此输出流。

一次写多个字节:

- 如果写的第一个字节是正数(0-127),那么显示的时候会查询ASCII表
- 如果写的第一个字节是负数,那第一个字节会和第二个字节,两个字节组成一个中文显示,查询系统默认码表(GBK)

② ``public void write (byte[] b,int off,int len) `` 从指定的字节数组写入 len 字节，从偏移量 off 开始输出到此输出流。

③ 利用String类自身的方法传入byte数组

- byte[] getBytes()  把字符串转换为字节数组

```java
public class Demo02OutputStream {
    public static void main(String[] args) throws IOException {
        //创建FileOutputStream对象,构造方法中绑定要写入数据的目的地
        FileOutputStream fos = new FileOutputStream(new File("09_IOAndProperties\\b.txt"));
        //调用FileOutputStream对象中的方法write,把数据写入到文件中
        //在文件中显示100,写个字节
        fos.write(49);
        fos.write(48);
        fos.write(48);

        byte[] bytes = {65,66,67,68,69};//ABCDE
        //byte[] bytes = {-65,-66,-67,68,69};//烤紻E
        fos.write(bytes);

        fos.write(bytes,1,2);//BC
        
        byte[] bytes2 = "你好".getBytes();
        System.out.println(Arrays.toString(bytes2));//[-28, -67, -96, -27, -91, -67]
        fos.write(bytes2);

        //释放资源
        fos.close();
    }
}
```

### 5.2 FileOutputStream

① java.io.FileOutputStream extends OutputStream

文件字节输出流：把内存中的数字写入到硬盘的文件中。

```java
FileOutputStream(String name) 创建一个向具有指定名称的文件中写入数据的输出文件流。
FileOutputStream(File file) 创建一个向指定 File 对象表示的文件中写入数据的文件输出流。
```
**构造方法的作用:**

- 1.创建一个FileOutputStream对象
- 2.会根据构造方法中传递的文件/文件路径,创建一个空的文件
- 3.会把FileOutputStream对象指向创建好的文件

**使用步骤：**

- 1.创建一个FileOutputStream对象,构造方法中传递写入数据的目的地
- 2.调用FileOutputStream对象中的方法write,把数据写入到文件中
- 3.释放资源(流使用会占用一定的内存,使用完毕要把内存清空,提供程序的效率)

② 记事本原理

![image-20210314021226202](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/进阶/20210314021228.png)

③ 追加写/续写:使用两个参数的构造方法

```java
FileOutputStream(String name, boolean append) 创建一个向具有指定 name 的文件中写入数据的输出文件流。
FileOutputStream(File file, boolean append) 创建一个向指定 File 对象表示的文件中写入数据的文件输出流。
```

**参数:**

- String name,File file:写入数据的目的地
- boolean append:追加写开关
  - true:创建对象不会覆盖源文件,继续在文件的末尾追加写数据
  - false:创建一个新文件,覆盖源文件    

**写换行:写换行符号**

- windows:\r\n
-  linux:/n
- mac:/r

```java
public class Demo03OutputStream {
    public static void main(String[] args) throws IOException {
        FileOutputStream fos = new FileOutputStream("09_IOAndProperties\\c.txt",true);
        for (int i = 1; i <=10 ; i++) {
            fos.write("你好".getBytes());
            fos.write("\r\n".getBytes());
        }

        fos.close();
    }
}
```

### 5.3 InputStream

java.io.InputStream：**字节输入流**，此抽象类是表示字节输入流的所有类的超类。定义了一些子类共性的成员方法：

```java
int read()从输入流中读取数据的下一个字节。
int read(byte[] b) 从输入流中读取一定数量的字节，并将其存储在缓冲区数组 b 中。
void close() 关闭此输入流并释放与该流关联的所有系统资源。
```
### 5.4 FileInputStream

① java.io.FileInputStream extends InputStream

把硬盘文件中的数据，读取到内存中使用。

**构造方法：**

```java
FileInputStream(String name)
FileInputStream(File file)
```
**参数:读取文件的数据源**

- String name:文件的路径
- File file:文件

**构造方法的作用:**

- 1.会创建一个FileInputStream对象
-  2.会把FileInputStream对象指定构造方法中要读取的文件

**使用步骤：**

- 1.创建FileInputStream对象,构造方法中绑定要读取的数据源
- 2.使用FileInputStream对象中的方法read,读取文件
- 3.释放资源

② 每次读取一个字节，并把指针向后移一个字节

```java
public class Demo01InputStream {
    public static void main(String[] args) throws IOException {
        //1.创建FileInputStream对象,构造方法中绑定要读取的数据源
        FileInputStream fis = new FileInputStream("09_IOAndProperties\\c.txt");
        //2.使用FileInputStream对象中的方法read,读取文件
        //int read()读取文件中的一个字节并返回,读取到文件的末尾返回-1
        int len = fis.read();
        System.out.println(len);//97 a

        len = fis.read();
        System.out.println(len);// 98 b

        len = fis.read();
        System.out.println(len);//99 c

        len = fis.read();
        System.out.println(len);//-1

        len = fis.read();
        System.out.println(len);//-1

        //3.释放资源
        fis.close();
    }
}
```

![image-20210314021242357](https://gitee.com/nigream/cloudimage/raw/master/java_notes_img/进阶/20210314021243.png)

⑤ 一次读取多个字节

```java
int read(byte[] b) 从输入流中读取一定数量的字节，并将其存储在缓冲区数组 b 中。
    1.方法的参数byte[]的作用?
        起到缓冲作用,存储每次读取到的多个字节
        数组的长度一般定义为1024(1kb)或者1024的整数倍
    2.方法的返回值int是什么?
        每次读取的有效字节个数
```

```java
String类的构造方法
String(byte[] bytes) :把字节数组转换为字符串
String(byte[] bytes, int offset, int length) 把字节数组的一部分转换为字符串 offset:数组的开始索引 length:转换的字节个数
```
```java
public class Demo02InputStream {
    public static void main(String[] args) throws IOException {
        //创建FileInputStream对象,构造方法中绑定要读取的数据源
        FileInputStream fis = new FileInputStream("09_IOAndProperties\\b.txt");
        //使用FileInputStream对象中的方法read读取文件
        //int read(byte[] b) 从输入流中读取一定数量的字节，并将其存储在缓冲区数组 b 中。
        byte[] bytes = new byte[2];
        int len = fis.read(bytes);
        System.out.println(len);//2
        //System.out.println(Arrays.toString(bytes));//[65, 66]
        System.out.println(new String(bytes));//AB

        len = fis.read(bytes);
        System.out.println(len);//2
        System.out.println(new String(bytes));//CD

        len = fis.read(bytes);
        System.out.println(len);//1
        System.out.println(new String(bytes));//ED

        len = fis.read(bytes);
        System.out.println(len);//-1
        System.out.println(new String(bytes));//ED

        /*
            发现以上读取是一个重复的过程,可以使用循环优化
            不知道文件中有多少字节,所以使用while循环
            while循环结束的条件,读取到-1结束
        */
        /*byte[] bytes = new byte[1024];//存储读取到的多个字节
        int len = 0; //记录每次读取的有效字节个数
        while((len = fis.read(bytes))!=-1){
            //String(byte[] bytes, int offset, int length) 把字节数组的一部分转换为字符串 offset:数组的开始索引 length:转换的字节个数
            System.out.println(new String(bytes,0,len));
        } */

        //释放资源
        fis.close();
    }
}
```

### 5.5 文件复制

```java
public class Demo01CopyFile {
    public static void main(String[] args) throws IOException {
        long s = System.currentTimeMillis();
        //1.创建一个字节输入流对象,构造方法中绑定要读取的数据源
        FileInputStream fis = new FileInputStream("c:\\1.jpg");
        //2.创建一个字节输出流对象,构造方法中绑定要写入的目的地
        FileOutputStream fos = new FileOutputStream("d:\\1.jpg");
        //一次读取一个字节写入一个字节的方式
        //3.使用字节输入流对象中的方法read读取文件----速度太慢
        /*int len = 0;
        while((len = fis.read())!=-1){
            //4.使用字节输出流中的方法write,把读取到的字节写入到目的地的文件中
            fos.write(len);
        }*/

        //使用数组缓冲读取多个字节,写入多个字节
        byte[] bytes = new byte[1024];
        //3.使用字节输入流对象中的方法read读取文件
        int len = 0;//每次读取的有效字节个数
        while((len = fis.read(bytes))!=-1){
            //4.使用字节输出流中的方法write,把读取到的字节写入到目的地的文件中
            fos.write(bytes,0,len);
        }

        //5.释放资源(先关写的,后关闭读的;如果写完了,肯定读取完毕了)
        fos.close();
        fis.close();
        long e = System.currentTimeMillis();
        System.out.println("复制文件共耗时:"+(e-s)+"毫秒");
    }
}
```

### 5.6 读取中文

使用字节流读取中文文件

**1个中文字符**

- GBK:占用两个字节
- UTF-8:占用3个字节

```java
public class Demo01InputStream {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("09_IOAndProperties\\c.txt");
        int len = 0;
        while((len = fis.read())!=-1){
            System.out.println((char)len);//出现乱码
        }
        fis.close();
    }
}
```

## 六、字符流

### 6.1 Reader

java.io.Reader 字符输入流的最顶层的父类，定义类一些共性的成员方法，是一个抽象类。

```java
int read() 读取单个字符并返回。
int read(char[] cbuf)一次读取多个字符,将字符读入数组。
void close() 关闭该流并释放与之关联的所有源。
```

### 6.2 FileReader

java.io.FileReader extends InputStreamReader extends Reader 把硬盘文件中的数据以字符的方式读取到内存中。

**构造方法:**

```java
FileReader(String fileName)
FileReader(File file)
参数:读取文件的数据源
    String fileName:文件的路径
    File file:一个文件
FileReader构造方法的作用:
    1.创建一个FileReader对象
    2.会把FileReader对象指向要读取的文件
```

使用步骤:

- 1.创建FileReader对象,构造方法中绑定要读取的数据源
- 2.使用FileReader对象中的方法read读取文件
- 3.释放资源

```java
public class Demo02Reader {
    public static void main(String[] args) throws IOException {
        //1.创建FileReader对象,构造方法中绑定要读取的数据源
        FileReader fr = new FileReader("09_IOAndProperties\\c.txt");
        //2.使用FileReader对象中的方法read读取文件
        //int read() 读取单个字符并返回。
        /*int len = 0;
        while((len = fr.read())!=-1){
            System.out.print((char)len);
        }*/

        //int read(char[] cbuf)一次读取多个字符,将字符读入数组。
        char[] cs = new char[1024];//存储读取到的多个字符
        int len = 0;//记录的是每次读取的有效字符个数
        while((len = fr.read(cs))!=-1){
            /*
                String类的构造方法
                String(char[] value) 把字符数组转换为字符串
                String(char[] value, int offset, int count) 把字符数组的一部分转换为字符串 offset数组的开始索引 count转换的个数
             */
            System.out.println(new String(cs,0,len));
        }

        //3.释放资源
        fr.close();
    }
}
```

### 6.3 Writer

java.io.Writer:字符输出流,是所有字符输出流的最顶层的父类,是一个抽象类

```java
- void write(int c) 写入单个字符。
- void write(char[] cbuf)写入字符数组。
- abstract  void write(char[] cbuf, int off, int len)写入字符数组的某一部分,off数组的开始索引,len写的字符个数。
- void write(String str)写入字符串。
- void write(String str, int off, int len) 写入字符串的某一部分,off字符串的开始索引,len写的字符个数。
- void flush()刷新该流的缓冲。
- void close() 关闭此流，但要先刷新它。
```

### 6.4 FileWriter

java.io.FileWriter extends OutputStreamWriter extends Writer 文件字符输出流，把内存中的字符数据写入到文件中

① 构造方法

```java
FileWriter(File file) 根据给定的 File 对象构造一个 FileWriter 对象。
FileWriter(String fileName) 根据给定的文件名构造一个 FileWriter 对象。
参数:写入数据的目的地
- String fileName: 文件的路径
- File file: 是一个文件
```

**构造方法的作用:**

- 1.会创建一个FileWriter对象
- 2.会根据构造方法中传递的文件/文件的路径,创建文件
- 3.会把FileWriter对象指向创建好的文件

② 使用步骤

- 1.创建FileWriter对象,构造方法中绑定要写入数据的目的地
- 2.使用FileWriter中的方法write,把数据写入到内存缓冲区中(字符转换为字节的过程)
- 3.使用FileWriter中的方法flush,把内存缓冲区中的数据,刷新到文件中
- 4.释放资源(会先把内存缓冲区中的数据刷新到文件中)

```java
public class Demo01Writer {
    public static void main(String[] args) throws IOException {
        //1.创建FileWriter对象,构造方法中绑定要写入数据的目的地
        FileWriter fw = new FileWriter("09_IOAndProperties\\d.txt");
        //2.使用FileWriter中的方法write,把数据写入到内存缓冲区中(字符转换为字节的过程)
        //void write(int c) 写入单个字符。
        fw.write(97);
        //3.使用FileWriter中的方法flush,把内存缓冲区中的数据,刷新到文件中
        //fw.flush();
        //4.释放资源(会先把内存缓冲区中的数据刷新到文件中)
        fw.close();
    }
}
```

③ close 和 flush方法的区别

 flush：刷新缓冲区，该对象可以继续使用

close：先刷新缓冲区，然后通知系统释放资源。流对象不可以再继续使用了。

④ 续写和换行

```java
续写,追加写:使用两个参数的构造方法
FileWriter(String fileName, boolean append)
FileWriter(File file, boolean append)
参数:
String fileName,File file:写入数据的目的地
boolean append:续写开关 true:不会创建新的文件覆盖源文件,可以续写; false:创建新的文件覆盖源文件
```
 换行:换行符号

- windows:\r\n
- linux:/n
- mac:/r

```java
public class Demo04Writer {
    public static void main(String[] args) throws IOException {
        FileWriter fw = new FileWriter("09_IOAndProperties\\g.txt",true);
        for (int i = 0; i <10 ; i++) {
            fw.write("HelloWorld"+i+"\r\n");
        }
        fw.close();
    }
}
```

### 6.5 try..catch

a. jdk7之前

```java
/*
    在jdk1.7之前使用try catch finally 处理流中的异常
    格式:
        try{
            可能会产出异常的代码
        }catch(异常类变量 变量名){
            异常的处理逻辑
        }finally{
            一定会指定的代码
            资源释放
        }
 */
public class Demo01TryCatch {
    public static void main(String[] args) {
        //提高变量fw的作用域,让finally可以使用
        //变量在定义的时候,可以没有值,但是使用的时候必须有值
        //fw = new FileWriter("09_IOAndProperties\\g.txt",true); 执行失败,fw没有值,fw.close会报错
        FileWriter fw = null;
        try{
            //可能会产出异常的代码
            fw = new FileWriter("w:\\09_IOAndProperties\\g.txt",true);
            for (int i = 0; i <10 ; i++) {
                fw.write("HelloWorld"+i+"\r\n");
            }
        }catch(IOException e){
            //异常的处理逻辑
            System.out.println(e);
        }finally {
            //一定会指定的代码
            //创建对象失败了,fw的默认值就是null,null是不能调用方法的,会抛出NullPointerException,需要增加一个判断,不是null在把资源释放
            if(fw!=null){
                try {
                    //fw.close方法声明抛出了IOException异常对象,所以我们就的处理这个异常对象,要么throws,要么try catch
                    fw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

        }
    }
}
```

b. jdk7的新写法

```java
/*
    JDK7的新特性
    在try的后边可以增加一个(),在括号中可以定义流对象
    那么这个流对象的作用域就在try中有效
    try中的代码执行完毕,会自动把流对象释放,不用写finally
    格式:
        try(定义流对象;定义流对象....){
            可能会产出异常的代码
        }catch(异常类变量 变量名){
            异常的处理逻辑
        }
 */
public class Demo02JDK7 {
    public static void main(String[] args) {
        try(//1.创建一个字节输入流对象,构造方法中绑定要读取的数据源
            FileInputStream fis = new FileInputStream("c:\\1.jpg");
            //2.创建一个字节输出流对象,构造方法中绑定要写入的目的地
            FileOutputStream fos = new FileOutputStream("d:\\1.jpg");){

            //可能会产出异常的代码
            //一次读取一个字节写入一个字节的方式
            //3.使用字节输入流对象中的方法read读取文件
            int len = 0;
            while((len = fis.read())!=-1){
                //4.使用字节输出流中的方法write,把读取到的字节写入到目的地的文件中
                fos.write(len);
            }
        }catch (IOException e){
            //异常的处理逻辑
            System.out.println(e);
        }
    }
}
```

c. jdk9的新写法

```java
/*
    JDK9新特性
    try的前边可以定义流对象
    在try后边的()中可以直接引入流对象的名称(变量名)
    在try代码执行完毕之后,流对象也可以释放掉,不用写finally
    格式:
        A a = new A();
        B b = new B();
        try(a;b){
            可能会产出异常的代码
        }catch(异常类变量 变量名){
            异常的处理逻辑
        }
 */
public class Demo03JDK9 {
    public static void main(String[] args) throws IOException {
        //1.创建一个字节输入流对象,构造方法中绑定要读取的数据源
        FileInputStream fis = new FileInputStream("c:\\1.jpg");
        //2.创建一个字节输出流对象,构造方法中绑定要写入的目的地
        FileOutputStream fos = new FileOutputStream("d:\\1.jpg");

        try(fis;fos){
            //一次读取一个字节写入一个字节的方式
            //3.使用字节输入流对象中的方法read读取文件
            int len = 0;
            while((len = fis.read())!=-1){
                //4.使用字节输出流中的方法write,把读取到的字节写入到目的地的文件中
                fos.write(len);
            }
        }catch (IOException e){
            System.out.println(e);
        }

        //fos.write(1);//Stream Closed
    }
}
```

## 七、Properties

### 7.1 概念

``java.util.Properties extends Hashtable<K,V> implements Map<K,V>``

① 它表示一个持久的属性集，可以保存在流中或从流中加载；

② 它是唯一一个和IO流相结合的集合；

③ 可以使用Properties集合中的store方法，把集合中的临时数据，持久化写入到硬盘中存储。

④ 可以使用Properties集合中的load方法，把硬盘中保存的文件（键值对），读取到集合中使用。

⑤ Properties 集合是一个双列集合，key和value默认都是字符串

###  7.2 方法

```java
Properties集合有一些操作字符串的特有方法
Object setProperty(String key, String value) 调用 Hashtable 的方法 put。
String getProperty(String key) 通过key找到value值,此方法相当于Map集合中的get(key)方法
Set<String> stringPropertyNames() 返回此属性列表中的键集，其中该键及其对应值是字符串,此方法相当于Map集合中的keySet方法
```

```java
    private static void show01() {
        //创建Properties集合对象
        Properties prop = new Properties();
        //使用setProperty往集合中添加数据
        prop.setProperty("赵丽颖","168");
        prop.setProperty("迪丽热巴","165");
        prop.setProperty("古力娜扎","160");
        //prop.put(1,true);

        //使用stringPropertyNames把Properties集合中的键取出,存储到一个Set集合中
        Set<String> set = prop.stringPropertyNames();

        //遍历Set集合,取出Properties集合的每一个键
        for (String key : set) {
            //使用getProperty方法通过key获取value
            String value = prop.getProperty(key);
            System.out.println(key+"="+value);
        }
    }
```

**① store方法**

可以使用Properties集合中的方法store,把集合中的临时数据,持久化写入到硬盘中存储

```java
void store(OutputStream out, String comments)
void store(Writer writer, String comments)
参数:
- OutputStream out:字节输出流,不能写入中文
- Writer writer:字符输出流,可以写中文
- String comments:注释,用来解释说明保存的文件是做什么用的(不能使用中文,会产生乱码,默认是Unicode编码,一般使用""空字符串)
```
**使用步骤:**

- 1.创建Properties集合对象,添加数据。
- 2.创建字节输出流/字符输出流对象,构造方法中绑定要输出的目的地。
- 3.使用Properties集合中的方法store,把集合中的临时数据,持久化写入到硬盘中存储。
- 4.释放资源。

```java
    private static void show02() throws IOException {
        //1.创建Properties集合对象,添加数据
        Properties prop = new Properties();
        prop.setProperty("赵丽颖","168");
        prop.setProperty("迪丽热巴","165");
        prop.setProperty("古力娜扎","160");

        //2.创建字节输出流/字符输出流对象,构造方法中绑定要输出的目的地
        FileWriter fw = new FileWriter("09_IOAndProperties\\prop.txt");

        //3.使用Properties集合中的方法store,把集合中的临时数据,持久化写入到硬盘中存储
        prop.store(fw,"save data");

        //4.释放资源
        fw.close();

        //prop.store(new FileOutputStream("09_IOAndProperties\\prop2.txt"),"");//不能写中文
    }
```

**② load方法**

可以使用Properties集合中的方法load,把硬盘中保存的文件(键值对),读取到集合中使用

```java
void load(InputStream inStream)
void load(Reader reader)
参数:
InputStream inStream: 字节输入流,不能读取含有中文的键值对
Reader reader: 字符输入流,能读取含有中文的键值对
```
**使用步骤:**

- 1.创建Properties集合对象
- 2.使用Properties集合对象中的方法load读取保存键值对的文件
- 3.遍历Properties集合

**注意:**

- 1.存储键值对的文件中,键与值默认的连接符号可以使用=,空格(其他符号)
- 2.存储键值对的文件中,可以使用#进行注释,被注释的键值对不会再被读取
- 3.存储键值对的文件中,键与值默认都是字符串,不用再加引号

```java
    private static void show03() throws IOException {
        //1.创建Properties集合对象
        Properties prop = new Properties();
        //2.使用Properties集合对象中的方法load读取保存键值对的文件
        prop.load(new FileReader("09_IOAndProperties\\prop.txt"));
        //prop.load(new FileInputStream("09_IOAndProperties\\prop.txt"));
        //3.遍历Properties集合
        Set<String> set = prop.stringPropertyNames();
        for (String key : set) {
            String value = prop.getProperty(key);
            System.out.println(key+"="+value);
        }
    }
```

## 八、缓冲流

### 1.概述

![1565469829268](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565469829268.png)

![1565469765548](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565469765548.png)

### 2.字节缓冲输出流BuffereOutputStream

``java.io.BuffereOutputStream extends OutputStream``

![1565470131807](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565470131807.png)

![1565470240887](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565470240887.png)

### 3.字节缓冲输入流BuffereInputStream

![1565470443329](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565470443329.png)

![1565470624567](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565470624567.png)

### 4.复制文件

![1565470781065](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565470781065.png)

### 5.字符缓冲输出流BufferedWriter

![1565471023690](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565471023690.png)

**使用步骤**

![1565471105424](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565471105424.png)

### 6.字符缓冲输入流BufferedReader

![1565473778655](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565473778655.png)

**使用步骤**

![1565473800819](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565473800819.png)

### 7.文本排序

![1565473992398](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565473992398.png)

## 九、转换流

### 1.字符编码

![1565474261115](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565474261115.png)

### 2.转换流的原理

![1565475553344](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565475553344.png)

### 3.OutputStreamWriter

![1565475732000](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565475732000.png)

**使用步骤**

![1565475787990](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565475787990.png)

### 3.InputStreamReader

![1565476177344](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565476177344.png)

**使用步骤**

![1565476225440](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565476225440.png)

### 4.转换文件编码

![1565476484818](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565476484818.png)

## 十 、序列化与反序列化

### 1.原理

![1565476828864](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565476828864.png)

### 2.对象的序列化流ObjectOutputStream

``java.io.ObjectOutputStream extends OutputStream``

把对象以流的方式写入到文件中保存。

![1565483740898](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565483740898.png)

![1565483942654](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565483942654.png)

### 3.对象的反序列化流ObjectInputStream

``java.io.ObjectInputStream extends InputStream``

把文件中保存的对象，以流的方式读取出来使用。

![1565484216121](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565484216121.png)

![1565484324820](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565484324820.png)

### 4.transient关键字（瞬态关键字）

![1565484485864](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565484485864.png)

### 5.InvalidClassException异常

![1565485155273](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565485155273.png)

### 6.练习_序列化集合

![1565485332758](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565485332758.png)

## 十一、打印流

``java.io.PrintStream``

![1565485814932](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565485814932.png)

![1565485859732](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565485859732.png)

![1565486064339](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565486064339.png)

![1565486114985](C:\Users\nigream Lin\AppData\Roaming\Typora\typora-user-images\1565486114985.png)

