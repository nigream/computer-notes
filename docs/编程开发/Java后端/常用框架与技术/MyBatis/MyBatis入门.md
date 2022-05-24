# MyBatis入门

---

参考：

```wiki
1. 黑马程序员的官方教程笔记
```



## 持久层技术

- `JDBC` 技术： `Connection` 、 `PreparedStatement` 、 `ResultSet` 。
- Spring 的 `JdbcTemplate` ： Spring 中对 jdbc 的简单封装。
- Apache 的 `DBUtils` ：它和 Spring 的 `JdbcTemplate` 很像，也是对 Jdbc 的简单封装。
- 以上这些都不是框架， `JDBC` 是规范， Spring 的 `JdbcTemplate` 和 Apache 的 `DBUtils` 都只是工具类。

## 为什么要使用框架

**使用 JDBC ：**

```java
public static void main(String[] args) {
    Connection connection = null;
    PreparedStatement preparedStatement = null;
    ResultSet resultSet = null;
    try {
        //加载数据库驱动
        Class.forName("com.mysql.jdbc.Driver");
        //通过驱动管理类获取数据库链接
        connection = DriverManager
            .getConnection("jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8", "root", "root");
        //定义 sql 语句 ?表示占位符
        String sql = "select * from user where username = ?";
        //获取预处理 statement
        preparedStatement = connection.prepareStatement(sql);
        //设置参数，第一个参数为 sql 语句中参数的序号（从 1 开始），第二个参数为设置的参数值
        preparedStatement.setString(1, "王五");
        //向数据库发出 sql 执行查询，查询出结果集
        resultSet = preparedStatement.executeQuery();
        //遍历查询结果集
        while (resultSet.next()) {
            System.out.println(resultSet.getString("id") + " " + resultSet.getString("username"));
        }
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        //释放资源
        if (resultSet != null) {
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (preparedStatement != null) {
            try {
                preparedStatement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (connection != null) {
            try {
                connection.close();
            } catch (SQLException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    }
}  
```

**存在的问题：**

1. **数据库连接创建、释放频繁** ：造成系统资源浪费从而影响系统 **性能** ，如果使用 **数据库连接池** 可解决此问题。
2. **Sql 语句在代码中硬编码** ：造成代码不易维护，实际应用 sql 变化的可能较大，sql 变动需要改变 java 代码。
3. **使用 preparedStatement 向占位符传参数存在硬编码** ：因为 sql 语句的 where 条件不一定，可能多也可能少，修改 sql 还要修改代码，系统不易维护。
4. **对结果集解析存在硬编码 (查询列名)** ：sql 变化导致解析代码变化，系统不易维护，如果能将数据库记录封装成 pojo 对象解析比较方便。

## MyBatis是什么

- mybatis是一个持久层框架，用java编写的。
- 它封装了 jdbc 操作的很多细节，使开发者只需要关注 sql 语句本身，而无需关注注册驱动，创建连接等繁杂过程。
- 它使用了 ORM 思想实现了结果集的封装。

## 什么是ORM

**Object Relational Mappging 对象关系映射**

简单地说，就是把 **数据库表** 和 **实体类及实体类的属性** 对应起来，让我们可以操作实体类就实现操作数据库表。

## 入门案例

### 环境准备

#### 创建maven工程并导入坐标	

```xml
<dependencies>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.9</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.25</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.17.2</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
    </dependency>
</dependencies>
```

#### 创建实体类和dao的接口

实体类 `com.itheima.domain.User` 省略

```java
package com.itheima.dao;
public interface IUserDao {
    List<User> findAll();
}
```

#### 创建Mybatis的主配置文件

`SqlMapConifg.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!-- mybatis的主配置文件 -->
<configuration>
    <!-- 配置环境 -->
    <environments default="mysql">
        <!-- 配置mysql的环境-->
        <environment id="mysql">
            <!-- 配置事务的类型-->
            <transactionManager type="JDBC"></transactionManager>
            <!-- 配置数据源（连接池） -->
            <dataSource type="POOLED">
                <!-- 配置连接数据库的4个基本信息 -->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/eesy_mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="1234"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 指定映射配置文件的位置，映射配置文件指的是每个dao独立的配置文件 -->
    <mappers>
        <mapper resource="com/itheima/dao/IUserDao.xml"/>
    </mappers>
</configuration>
```

#### 创建映射配置文件

`IUserDao.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.dao.IUserDao">
    <!--配置查询所有-->
    <select id="findAll" resultType="com.itheima.domain.User">
        select * from user
    </select>
</mapper>
```

- 映射配置文件位置必须和 **dao接口** 的 **包结构** 相同。
- 映射配置文件的 mapper 标签 **namespace 属性** 的取值必须是 **dao接口** 的 **全限定类名** 。
- 映射配置文件的 **操作配置（如select）** ，**id** 属性的取值必须是 **dao接口** 的 **方法名** 。
