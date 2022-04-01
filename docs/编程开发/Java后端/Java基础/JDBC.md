# JDBC

---

## 概念

> Java DataBase Connectivity  Java 数据库连接，Java语言操作数据库

* JDBC本质：其实是官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口。
* 各个数据库厂商去实现这套接口，提供数据库驱动 jar 包。
* 我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动 jar 包中的实现类。

## 如何使用

### 步骤

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

### 代码实现

```java
// 1.导入驱动jar包
// 2.注册驱动
Class.forName("com.mysql.jdbc.Driver");
// 3.获取数据库连接对象
Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db3", "root", "root");
// 4.定义sql语句
String sql = "update account set balance = 500 where id = 1";
// 5.获取执行sql的对象 Statement
Statement stmt = conn.createStatement();
// 6.执行sql
int count = stmt.executeUpdate(sql);
// 7.处理结果
System.out.println(count);
// 8.释放资源
stmt.close();
conn.close();
```

## 各对象的使用

### DriverManager

> 驱动管理对象

**① 注册驱动：告诉程序该使用哪一个数据库驱动 jar。**

- DriverManager 类的注册驱动方法

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

**注意：** mysql5 之后的驱动jar包可以省略注册驱动的步骤。因为在 `mysql-connector-java-5.1.37-bin.jar!\META-INF\services\java.sql.Driver` 这个文件中配置了 `com.mysql.jdbc.Driver` 类，如果没写注册驱动的代码，程序会自动帮助注册。

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

### Connection

> 数据库连接对象

#### 获取执行sql 的对象

```java
Statement createStatement()
PreparedStatement prepareStatement(String sql)  
```

#### 管理事务

- 事务：一个包含多个步骤的业务操作，，如果这个业务操作被事务管理，则这多个步骤要么同时成功，要么同时失败。

```java
- 开启事务：setAutoCommit(boolean autoCommit) ：调用该方法设置参数为false，即开启事务（在执行sql之前开启事务）
- 提交事务：commit() （当所有sql都执行完提交事务）
- 回滚事务：rollback() （在catch中回滚事务）
```

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

### Statement

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

### ResultSet

#### 方法

> 结果集对象，封装查询结果。

* boolean next()：游标向下移动一行，判断当前行是否是最后一行的下面一行，如果是，则返回false，如果不是则返回true
* getXxx(参数)：获取数据（Xxx：代表数据类型，如： int getInt() ,	String getString()）
	- 参数：
	  - int：代表列的编号，从1开始。如： getString(1)
	  - String：代表列名称。 如： getDouble("balance")

#### 如何使用

```java
// 循环判断游标是否是最后一行末尾。
while(rs.next()){
    // 获取数据
    int id = rs.getInt(1);
    String name = rs.getString("name");
    double balance = rs.getDouble(3);
    System.out.println(id + "---" + name + "---" + balance);
 }
```
#### 使用案例

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

```properties
# jdbc.properties（放在src目录下）
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

### PreparedStatement

> 执行sql的对象

#### SQL注入问题

- 在拼接sql时，有一些sql的特殊关键字参与字符串的拼接。会造成安全性问题：输入用户随便，输入密码：`a' or 'a' = 'a`，得到 `sql：select * from user where username = 'fhdsjkf' and password = 'a' or 'a' = 'a'` 登录成功。
- 解决sql注入问题：使用PreparedStatement对象来解决，采用预编译的SQL：参数使用?作为占位符

#### 使用步骤

1. 导入驱动jar包 `mysql-connector-java-5.1.37-bin.jar`
2. 注册驱动
3. 获取数据库连接对象 Connection
4. 定义sql
   - 注意：sql的参数使用 ? 作为占位符。 如：`select * from user where username = ? and password = ?;`
5. 获取执行sql语句的PreparedStatement对象，使用Connection类中的  `prepareStatement(String sql)`  方法
6. 给 ? 赋值
   - 方法： setXxx(参数1,参数2)
     - 参数1：？的位置编号 从1 开始
     - 参数2：？的值
7. 执行sql，接受返回结果，不需要传递sql语句
8. 处理结果
9. 释放资源

**注意：** 后期都会使用PreparedStatement来完成增删改查的所有操作

- 可以防止SQL注入
- 效率更高



