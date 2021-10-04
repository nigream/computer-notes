## 一、概念

> Java DataBase Connectivity  Java 数据库连接， Java语言操作数据库

* JDBC本质：其实是官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口。各个数据库厂商去实现这套接口，提供数据库驱动jar包。我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类。

## 二、快速入门

### 2.1 步骤

```java
1. 导入驱动jar包 mysql-connector-java-5.1.37-bin.jar
	- 复制mysql-connector-java-5.1.37-bin.jar到项目的libs目录下
	- 右键-->Add As Library
2. 注册驱动
3. 获取数据库连接对象 Connection
4. 定义sql
5. 获取执行sql语句的对象 Statement
6. 执行sql，接受返回结果
7. 处理结果
8. 释放资源
```

### 2.2 代码实现

```java
//1. 导入驱动jar包
//2.注册驱动
Class.forName("com.mysql.jdbc.Driver");
//3.获取数据库连接对象
Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db3", "root", "root");
//4.定义sql语句
String sql = "update account set balance = 500 where id = 1";
//5.获取执行sql的对象 Statement
Statement stmt = conn.createStatement();
//6.执行sql
int count = stmt.executeUpdate(sql);
//7.处理结果
System.out.println(count);
//8.释放资源
stmt.close();
conn.close();
```

## 三、各对象详解

### 3.1 DriverManager

> 驱动管理对象

**① 注册驱动：告诉程序该使用哪一个数据库驱动jar。**

- DriverManager类的注册驱动方法

```java
static void registerDriver(Driver driver) ：注册给定的驱动程序 DriverManager 。 
```

- 注册驱动代码

```java
Class.forName("com.mysql.jdbc.Driver");
```

- 通过查看源码发现：在 `com.mysql.jdbc.Driver` 类中存在静态代码块

```java
static {
	try {
		java.sql.DriverManager.registerDriver(new Driver());
	} catch (SQLException E) {
		throw new RuntimeException("Can't register driver!");
	}
}
```

**综上：** 当使用 `Class.forName()` 将 `com.mysql.jdbc.Driver` 类导入内存时，会首先自动调用静态代码块，静态代码块中的 `java.sql.DriverManager.registerDriver(new Driver())` 语句注册了一个驱动。

**注意：** mysql5之后的驱动jar包可以省略注册驱动的步骤。因为在 `mysql-connector-java-5.1.37-bin.jar!\META-INF\services\java.sql.Driver` 这个文件中配置了 `com.mysql.jdbc.Driver` 类，如果没写注册驱动的代码，程序会自动帮助注册。

**② 获取数据库连接**

```java
static Connection getConnection(String url, String user, String password) 
```

* url：指定连接的路径
	* 语法：jdbc:mysql://ip地址(域名):端口号/数据库名称
	* 例子：jdbc:mysql://localhost:3306/db3
	* 细节：如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql:///数据库名称
* user：用户名
* password：密码 

### 3.2 Connection

> 数据库连接对象

**① 获取执行sql 的对象**

```java
Statement createStatement()
PreparedStatement prepareStatement(String sql)  
```

**② 管理事务**

```java
- 开启事务：setAutoCommit(boolean autoCommit) ：调用该方法设置参数为false，即开启事务
- 提交事务：commit() 
- 回滚事务：rollback() 
```

### 3.3 Statement

> 执行sql的对象

- `boolean execute(String sql)` ：可以执行任意的sql (了解) 
- `int executeUpdate(String sql)` ：执行DML（insert、update、delete）语句、DDL(create，alter、drop)语句
  - 返回值：影响的行数，可以通过这个影响的行数判断DML语句是否执行成功 返回值 >0 的则执行成功，反之，则失败。
- `ResultSet executeQuery(String sql)`  ：执行DQL（select)语句

```java
Statement stmt = null;
Connection conn = null;
try {
    //1. 注册驱动
    Class.forName("com.mysql.jdbc.Driver");
    //2. 定义sql
    String sql = "insert into account values(null,'王五',3000)";
    //3.获取Connection对象
    conn = DriverManager.getConnection("jdbc:mysql:///db3", "root", "root");
    //4.获取执行sql的对象 Statement
    stmt = conn.createStatement();
    //5.执行sql
    int count = stmt.executeUpdate(sql);//影响的行数
    //6.处理结果
    System.out.println(count);
    if(count > 0){
        System.out.println("添加成功！");
    }else{
        System.out.println("添加失败！");
    }

} catch (ClassNotFoundException e) {
    e.printStackTrace();
} catch (SQLException e) {
    e.printStackTrace();
}finally {
    //stmt.close();
    //7. 释放资源
    //避免空指针异常
    if(stmt != null){
        try {
            stmt.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    if(conn != null){
        try {
            conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

### 3.4 ResultSet

#### 3.4.1 方法

> 结果集对象，封装查询结果。

* boolean next()：游标向下移动一行，判断当前行是否是最后一行的下面一行，如果是，则返回false，如果不是则返回true
* getXxx(参数)：获取数据（Xxx：代表数据类型，如： int getInt() ,	String getString()）
		* 参数：
		1. int：代表列的编号，从1开始。如： getString(1)
		2. String：代表列名称。 如： getDouble("balance")

#### 3.4.2 使用步骤

```java
//循环判断游标是否是最后一行末尾。
while(rs.next()){
    //获取数据
    int id = rs.getInt(1);
    String name = rs.getString("name");
    double balance = rs.getDouble(3);
    System.out.println(id + "---" + name + "---" + balance);
 }
```
#### 3.4.3 练习

##### ① 抽取工具类

- 抽取 **注册驱动**
- 抽取一个方法 **获取连接对象**
- 抽取一个方法 **释放资源**

```java
//JDBCUtils.java
public class JDBCUtils {
    private static String url;
    private static String user;
    private static String password;
    private static String driver;
    static {
        try {
            Properties properties = new Properties();
            ClassLoader classLoader = JDBCUtils.class.getClassLoader();
            URL resource = classLoader.getResource("jdbc.properties");
            String path = resource.getPath();
            properties.load(new FileReader(path));
            url = properties.getProperty("url");
            user = properties.getProperty("user");
            password = properties.getProperty("password");
            driver = properties.getProperty("driver");
            Class.forName(driver);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url,user,password);
    }


    public static void close(Statement statement,Connection connection){
        if (statement != null){
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if (connection != null){
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }


    public static void close(ResultSet resultSet, Statement statement, Connection connection){
        if (resultSet != null){
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if (statement != null){
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if (connection != null){
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

```java
//jdbc.properties（放在src目录下）
url = jdbc:mysql://localhost:3306/db4
user = root
password = root
driver = com.mysql.jdbc.Driver
```

##### ② 使用工具类

```java
定义一个方法，查询emp表的数据将其封装为对象，然后装载集合，返回。
1. 定义Emp类
2. 定义方法 public List<Emp> findAll(){}
3. 实现方法 select * from emp;
```

 ```java
public class Demo06 {
    public static void main(String[] args) {
        System.out.println(findAll());
    }
    public static List<Emp> findAll(){
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;
        List<Emp> list = null;
        try {
            connection = JDBCUtils.getConnection();
            statement = connection.createStatement();
            String sql = "select * from emp";
            resultSet = statement.executeQuery(sql);
            list = new ArrayList<>();
            while(resultSet.next()){
                int id = resultSet.getInt("id");
                String ename = resultSet.getString("ename");
                int job_id = resultSet.getInt("job_id");
                int mgr = resultSet.getInt("mgr");
                Date joindate = resultSet.getDate("joindate");
                double salary = resultSet.getDouble("salary");
                double bonus = resultSet.getDouble("bonus");
                int dept_id = resultSet.getInt("dept_id");

                Emp emp = new Emp();
                emp.setId(id);
                emp.setEname(ename);
                emp.setJob_id(job_id);
                emp.setMgr(mgr);
                emp.setJoindate(joindate);
                emp.setSalary(salary);
                emp.setBonus(bonus);
                emp.setDept_id(dept_id);

                list.add(emp);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            JDBCUtils.close(resultSet,statement,connection);
        }
        return list;
    }
}
 ```

##### ③ 登录案例

```java
public class Demo07 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入用户名");
        String username = scanner.nextLine();
        System.out.println("请输入密码");
        String password = scanner.nextLine();
        if(login(username,password)){
            System.out.println("登录成功");
        } else {
            System.out.println("登录失败");
        }
    }
    
    public static boolean login(String username, String password){
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;
        if(username == null || password == null){
            return false;
        }
        try {
            connection = JDBCUtils.getConnection();
            String sql = "select * from user where username = '" + username + "'" + "and password = '" + password + "';";
            statement = connection.createStatement();
            resultSet = statement.executeQuery(sql);
            return resultSet.next();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.close(resultSet,statement,connection);
        }
        return false;
    }
}
```

### 3.5 PreparedStatement

> 执行sql的对象

#### 3.5.1 SQL注入问题

- 在拼接sql时，有一些sql的特殊关键字参与字符串的拼接。会造成安全性问题：输入用户随便，输入密码：`a' or 'a' = 'a`，得到 `sql：select * from user where username = 'fhdsjkf' and password = 'a' or 'a' = 'a'` 登录成功。
- 解决sql注入问题：使用PreparedStatement对象来解决，采用预编译的SQL：参数使用?作为占位符

#### 3.5.2 使用步骤

① 导入驱动jar包 `mysql-connector-java-5.1.37-bin.jar`

② 注册驱动

③ 获取数据库连接对象 Connection

④ 定义sql

* 注意：sql的参数使用 ? 作为占位符。 如：`select * from user where username = ? and password = ?;`

⑤ 获取执行sql语句的PreparedStatement对象，使用Connection类中的  `prepareStatement(String sql)`  方法

⑥ 给 ? 赋值

- 方法： setXxx(参数1,参数2)
  - 参数1：？的位置编号 从1 开始
  - 参数2：？的值

⑦ 执行sql，接受返回结果，不需要传递sql语句

⑧ 处理结果

⑨ 释放资源

**注意：** 后期都会使用PreparedStatement来完成增删改查的所有操作

- 可以防止SQL注入
- 效率更高

### 3.6 事务管理

#### 3.6.1 事务

一个包含多个步骤的业务操作。如果这个业务操作被事务管理，则这多个步骤要么同时成功，要么同时失败。

有以下三种操作：① 开启事务 ② 提交事务 ③ 回滚事务

#### 3.6.2 管理事务

 使用Connection对象管理事务

**① 开启事务** ：setAutoCommit(boolean autoCommit) ：调用该方法设置参数为false，即开启事务。（在执行sql之前开启事务）

**② 提交事务** ：commit() （当所有sql都执行完提交事务）

**③ 回滚事务** ：rollback() （在catch中回滚事务）

```java
public class JDBCDemo10 {
    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement pstmt1 = null;
        PreparedStatement pstmt2 = null;

        try {
            //1.获取连接
            conn = JDBCUtils.getConnection();
            //开启事务
            conn.setAutoCommit(false);

            //2.定义sql
            //2.1 张三 - 500
            String sql1 = "update account set balance = balance - ? where id = ?";
            //2.2 李四 + 500
            String sql2 = "update account set balance = balance + ? where id = ?";
            //3.获取执行sql对象
            pstmt1 = conn.prepareStatement(sql1);
            pstmt2 = conn.prepareStatement(sql2);
            //4. 设置参数
            pstmt1.setDouble(1,500);
            pstmt1.setInt(2,1);

            pstmt2.setDouble(1,500);
            pstmt2.setInt(2,2);
            //5.执行sql
            pstmt1.executeUpdate();
            // 手动制造异常
            int i = 3/0;

            pstmt2.executeUpdate();
            //提交事务
            conn.commit();
        } catch (Exception e) {//由于开始事务和关闭事务之间可能存在各种错误，所以，此处抓的错误是大的Exception
            //事务回滚
            try {
                if(conn != null) {
                    conn.rollback();
                }
            } catch (SQLException e1) {
                e1.printStackTrace();
            }
            e.printStackTrace();
        }finally {
            JDBCUtils.close(pstmt1,conn);
            JDBCUtils.close(pstmt2,null);
        }

    }

}
```

## 四、数据库连接池

### 4.1 概念

其实就是一个容器(集合)，存放数据库连接的容器。当系统初始化好后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完之后，会将连接对象归还给容器。

好处：① 节约资源 ② 用户访问高效
### 4.2 实现

- 标准接口：javax.sql.DataSource

- 方法：

  ① 获取连接：getConnection()

  ② 归还连接：Connection类的close()。如果连接对象Connection是从连接池中获取的，那么调用Connection的close()方法，则不会再关闭连接了，而是归还连接

- 一般我们不去实现它，有数据库厂商来实现

  ① C3P0：数据库连接池技术，同hibernate一同发布的，较老。

  ② Druid：数据库连接池实现技术，十分高效，由阿里巴巴提供的。

### 4.3 C3P0

① 导入jar包 (两个) `c3p0-0.9.5.2.jar` 和 `mchange-commons-java-0.2.12.jar` 
  * 不要忘记导入数据库驱动jar包

② 定义配置文件

* 名称： c3p0.properties 或者 c3p0-config.xml
* 路径：直接将文件放在src目录下即可。

③ 创建核心对象：数据库连接池对象 ComboPooledDataSource

④ 获取连接： getConnection()

```java
//1.创建数据库连接池对象
DataSource ds  = new ComboPooledDataSource();
//2. 获取连接对象
Connection conn = ds.getConnection();
```
```java
//c3p0-config.xml
<c3p0-config>
  <!-- 使用默认的配置读取连接池对象 -->
  <default-config>
  	<!--  连接参数 -->
    <property name="driverClass">com.mysql.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/db4</property>
    <property name="user">root</property>
    <property name="password">root</property>
    
    <!-- 连接池参数 -->
    <!-- 初始申请的连接数量 -->
    <property name="initialPoolSize">5</property>
    <!-- 最大的连接数量 -->
    <property name="maxPoolSize">10</property>  
    <!-- 超时时间 -->
    <property name="checkoutTimeout">3000</property>
  </default-config>

  <named-config name="otherc3p0"> 
    <!--  连接参数 -->
    <property name="driverClass">com.mysql.jdbc.Driver</property>
    <property name="jdbcUrl">jdbc:mysql://localhost:3306/db4</property>
    <property name="user">root</property>
    <property name="password">root</property>
    
    <!-- 连接池参数 -->
    <property name="initialPoolSize">5</property>
    <property name="maxPoolSize">8</property>
    <property name="checkoutTimeout">1000</property>
  </named-config>
</c3p0-config>
```

```java
//获取指定配置名称的ComboPooledDataSource
DataSource ds  = new ComboPooledDataSource("otherc3p0");
```

### 4.4 druid

#### 4.4.1 使用步骤

① 导入jar包 druid-1.0.9.jar

② 定义配置文件
* 是properties形式的
* 可以叫任意名称，可以放在任意目录下

③ 加载配置文件：Properties

④ 获取数据库连接池对象：通过工厂来来获取  DruidDataSourceFactory

⑤ 获取连接：getConnection()

```java
//加载配置文件
Properties pro = new Properties();
InputStream is = DruidDemo.class.getClassLoader().getResourceAsStream("druid.properties");
pro.load(is);
//获取连接池对象
DataSource ds = DruidDataSourceFactory.createDataSource(pro);
//获取连接
Connection conn = ds.getConnection();
```

```java
//druid.properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/db3
username=root
password=root
initialSize=5
maxActive=10
maxWait=3000
```

#### 4.4.2 定义工具类

① 定义一个类 JDBCUtils

② 提供静态代码块加载配置文件，初始化连接池对象

③ 提供方法

- 获取连接方法：通过数据库连接池获取连接
- 释放资源
- 获取连接池的方法

```java
public class JDBCUtils {

    //1.定义成员变量 DataSource
    private static DataSource ds ;

    static{
        try {
            //1.加载配置文件
            Properties pro = new Properties();
            pro.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
            //2.获取DataSource
            ds = DruidDataSourceFactory.createDataSource(pro);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //获取连接
    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }

    //释放资源
    public static void close(Statement stmt,Connection conn){
       close(null,stmt,conn);
    }

    public static void close(ResultSet rs , Statement stmt, Connection conn){
        if(rs != null){
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if(stmt != null){
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if(conn != null){
            try {
                conn.close();//归还连接
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

    //获取连接池方法
    public static DataSource getDataSource(){
        return  ds;
    }

}
```

## 五、jdbc Template

### 5.1 步骤

> Spring框架对JDBC的简单封装。提供了一个JDBCTemplate对象简化JDBC的开发。

① 导入jar包

② 创建JdbcTemplate对象。依赖于数据源DataSource

* JdbcTemplate template = new JdbcTemplate(ds);

③ 调用JdbcTemplate的方法来完成CRUD的操作
* `update()` ：执行DML语句。增、删、改语句
* `queryForMap()` ：查询结果将结果集封装为map集合，将列名作为key，将值作为value 将这条记录封装为一个map集合（注意：这个方法查询的结果集长度只能是1，否则要用`queryForList()`）
* `queryForList()` ：查询结果将结果集封装为list集合（注意：将每一条记录封装为一个Map集合，再将Map集合装载到List集合中）
* `query()` ：查询结果，将结果封装为JavaBean对象
	* query的参数：RowMapper
		* 一般我们使用BeanPropertyRowMapper实现类。可以完成数据到JavaBean的自动封装
		* new BeanPropertyRowMapper<类型>(类型.class)
* `queryForObject()` ：查询结果，将结果封装为对象（一般用于聚合函数的查询）

### 5.2 入门

```java
public class JdbcTemplateDemo1 {
    public static void main(String[] args) {
        //1.导入jar包
        //2.创建JDBCTemplate对象
        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());//这里用的是4.4.2的工具类
        //3.调用方法
        String sql = "update account set balance = 5000 where id = ?";
        int count = template.update(sql, 3);
        System.out.println(count);
    }
}
```

### 5.3 练习

```java
1. 修改1号数据的 salary 为 10000
2. 添加一条记录
3. 删除刚才添加的记录
4. 查询id为1的记录，将其封装为Map集合
5. 查询所有记录，将其封装为List
6. 查询所有记录，将其封装为Emp对象的List集合
7. 查询总记录数
```

```java
public class JdbcTemplateDemo2 {

    //Junit单元测试，可以让方法独立执行


    //1. 获取JDBCTemplate对象
    private JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
    /**
     * 1. 修改1号数据的 salary 为 10000
     */
    @Test
    public void test1(){

        //2. 定义sql
        String sql = "update emp set salary = 10000 where id = 1001";
        //3. 执行sql
        int count = template.update(sql);
        System.out.println(count);
    }

    /**
     * 2. 添加一条记录
     */
    @Test
    public void test2(){
        String sql = "insert into emp(id,ename,dept_id) values(?,?,?)";
        int count = template.update(sql, 1015, "郭靖", 10);
        System.out.println(count);

    }

    /**
     * 3.删除刚才添加的记录
     */
    @Test
    public void test3(){
        String sql = "delete from emp where id = ?";
        int count = template.update(sql, 1015);
        System.out.println(count);
    }

    /**
     * 4.查询id为1001的记录，将其封装为Map集合
     * 注意：这个方法查询的结果集长度只能是1
     */
    @Test
    public void test4(){
        String sql = "select * from emp where id = ? or id = ?";
        Map<String, Object> map = template.queryForMap(sql, 1001,1002);
        System.out.println(map);
        //{id=1001, ename=孙悟空, job_id=4, mgr=1004, joindate=2000-12-17, salary=10000.00, bonus=null, dept_id=20}

    }

    /**
     * 5. 查询所有记录，将其封装为List
     */
    @Test
    public void test5(){
        String sql = "select * from emp";
        List<Map<String, Object>> list = template.queryForList(sql);

        for (Map<String, Object> stringObjectMap : list) {
            System.out.println(stringObjectMap);
        }
    }

    /**
     * 6. 查询所有记录，将其封装为Emp对象的List集合
     */

    @Test
    public void test6(){
        String sql = "select * from emp";
        List<Emp> list = template.query(sql, new RowMapper<Emp>() {

            @Override
            public Emp mapRow(ResultSet rs, int i) throws SQLException {
                Emp emp = new Emp();
                int id = rs.getInt("id");
                String ename = rs.getString("ename");
                int job_id = rs.getInt("job_id");
                int mgr = rs.getInt("mgr");
                Date joindate = rs.getDate("joindate");
                double salary = rs.getDouble("salary");
                double bonus = rs.getDouble("bonus");
                int dept_id = rs.getInt("dept_id");

                emp.setId(id);
                emp.setEname(ename);
                emp.setJob_id(job_id);
                emp.setMgr(mgr);
                emp.setJoindate(joindate);
                emp.setSalary(salary);
                emp.setBonus(bonus);
                emp.setDept_id(dept_id);

                return emp;
            }
        });


        for (Emp emp : list) {
            System.out.println(emp);
        }
    }

    /**
     * 6. 查询所有记录，将其封装为Emp对象的List集合
     */

    @Test
    public void test6_2(){
        String sql = "select * from emp";
        List<Emp> list = template.query(sql, new BeanPropertyRowMapper<Emp>(Emp.class));
        for (Emp emp : list) {
            System.out.println(emp);
        }
    }

    /**
     * 7. 查询总记录数
     */

    @Test
    public void test7(){
        String sql = "select count(id) from emp";
        Long total = template.queryForObject(sql, Long.class);
        System.out.println(total);
    }

}
```





