# JdbcTemplate

---

## 概述

- Spring框架对JDBC的简单封装。提供了一个 `JdbcTemplate` 对象简化JDBC的开发。

## 使用步骤

① 导入 `jar` 包

② 创建 `JdbcTemplate` 对象（依赖于数据源 `DataSource` ）。

* `JdbcTemplate template = new JdbcTemplate(ds);`

③ 调用 `JdbcTemplate` 的方法来完成 `CRUD` 操作

```properties
# druid.properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://127.0.0.1:3306/db3
username=root
password=root
initialSize=5
maxActive=10
maxWait=3000
```

```java
public class JDBCUtils {

    // 1.定义成员变量 DataSource
    private static DataSource ds ;

    static{
        try {
            // 1.加载配置文件
            Properties pro = new Properties();
            pro.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
            // 2.获取DataSource
            ds = DruidDataSourceFactory.createDataSource(pro);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // 获取连接
    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }

    // 释放资源
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
                // 归还连接
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

    // 获取连接池方法
    public static DataSource getDataSource(){
        return  ds;
    }
}
```

```java
public class JdbcTemplateDemo1 {
    public static void main(String[] args) {
        // 1.导入jar包
        // 2.创建JDBCTemplate对象
        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
        // 3.调用方法
        String sql = "update account set balance = 5000 where id = ?";
        int count = template.update(sql, 3);
        System.out.println(count);
    }
}
```

## 常用方法

```java
// 执行DML语句。增、删、改语句
update()
    
// 查询结果将结果集封装为map集合，将列名作为key，将值作为value 将这条记录封装为一个map集合（注意：这个方法查询的结果集长度只能是1，否则要用queryForList()）
queryForMap()
    
// 查询结果将结果集封装为list集合（注意：将每一条记录封装为一个Map集合，再将Map集合装载到List集合中）
queryForList()
    
// 查询结果，将结果封装为JavaBean对象
//    * query的参数：RowMapper
//    * 一般我们使用BeanPropertyRowMapper实现类。可以完成数据到JavaBean的自动封装
//    * new BeanPropertyRowMapper<类型>(类型.class)
query()
  
// 查询结果，将结果封装为对象（一般用于聚合函数的查询）
queryForObject() 
```

```java
/**
1. 修改1号数据的 salary 为 10000
2. 添加一条记录
3. 删除刚才添加的记录
4. 查询id为1的记录，将其封装为Map集合
5. 查询所有记录，将其封装为List
6. 查询所有记录，将其封装为Emp对象的List集合
7. 查询总记录数
*/
public class JdbcTemplateDemo2 {

    // Junit单元测试，可以让方法独立执行


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





