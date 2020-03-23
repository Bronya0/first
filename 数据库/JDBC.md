## 流程

**Step1**.调用DriverManager类中的静态方法getConnection()创建连接对象

getConnection(String url, String uesr, String password) 

getConnection(String url, Properties info)

参数：

url：连接字符串，不同的数据库URL是不同的，

协议名:子协议://服务器名或IP地址:端口号/数据库名?参数=参数值

* mysql的写法（本地时） jdbc:mysql://localhost:3306/数据库[?参数名=参数值]   （中括号表示附加条件，可无）

user：登录用户名 ； password：密码

```
Connection connection = DriverManager.getConnection(url, "root", "root");
```

or：

```
String url = "jdbc:mysql://localhost:3306/day24";
Properties info = new Properties();
//把用户名和密码放在info对象中
info.setProperty("user","root");
info.setProperty("password","root");
Connection connection = DriverManager.getConnection(url, info);
```

**Step2**.调用Connection接口的方法createStatement()创建语句声明对象

**Step3**.对语句对象传入sql

**Step4**.执行sql，用Statement对象的方法

```
int executeUpdate(String sql) 
```

用于发送DML
语句，增删改的操作，insert参数：SQL语句
返回值：返回对数据库影响的行数

```
ResultSet executeQuery(String sql) 
```

用于发送DQL
语句，执行查询的操作。select
参数：SQL语句
返回值：查询的结果集

**Step5**.处理结果

**Step6**.释放资源，先开后关，后开先关。包括ResultSet 结果集，Statement 语句，Connection 连接

# Statement举例

## DDL创建表

```
try {
    conn = DriverManager.getConnection("jdbc:mysql:///day24", "root", "root");
    //2. 通过连接对象得到语句对象
    statement = conn.createStatement();
    //3. 通过语句对象发送SQL语句给服务器
    //4. 执行SQL
    statement.executeUpdate("create table student (id int PRIMARY key auto_increment, " +
    "name varchar(20) not null, gender boolean, birthday date)");
    //5. 返回影响行数(DDL没有返回值)
    System.out.println("创建表成功");
} catch (SQLException e) {
    e.printStackTrace();
}
//6. 释放资源
finally {
    //关闭之前要先判断
    if (statement != null) {
    try {
    statement.close();
} catch (SQLException e) {
	e.printStackTrace();
}
```

## DML添加记录

```
public static void main(String[] args) throws SQLException {
    // 1) 创建连接对象
    Connection connection = DriverManager.getConnection("jdbc:mysql:///day24", "root",
    "root");
    // 2) 创建Statement语句对象
    Statement statement = connection.createStatement();
    // 3) 执行SQL语句：executeUpdate(sql)
    int count = 0;
    // 4) 返回影响的行数
    count += statement.executeUpdate("insert into student values(null, '孙悟空', 1, '1993-03-
    24')");
    count += statement.executeUpdate("insert into student values(null, '白骨精', 0, '1995-03-
    24')");
    count += statement.executeUpdate("insert into student values(null, '猪八戒', 1, '1903-03-
    9 / 21
    24')");
    count += statement.executeUpdate("insert into student values(null, '嫦娥', 0, '1993-03-
    11')");
    System.out.println("插入了" + count + "条记录");
    // 5) 释放资源
    statement.close();
    connection.close();
}
}

```

## DQL查询

ResultSet 接口：
作用：封装数据库查询的结果集，对结果集进行遍历，取出每一条记录。

方法：

* boolean next() 

  1) 游标向下移动1 行
  2) 返回boolean 类型，如果还有下一条记录，返回true，否则返回false

* 数据类型getXxx() 

  1) 通过字段名，参数是String 类型。返回不同的类型
  2) 通过列号，参数是整数，从1 开始。返回不同的类型

```
public static void main(String[] args) throws SQLException {
    //1) 得到连接对象
    Connection connection =
    DriverManager.getConnection("jdbc:mysql://localhost:3306/day24","root","root");
    //2) 得到语句对象
    Statement statement = connection.createStatement();
    //3) 执行SQL语句得到结果集ResultSet对象
    ResultSet rs = statement.executeQuery("select * from student");
    //4) 循环遍历取出每一条记录
    while(rs.next()) {
    int id = rs.getInt("id");
    String name = rs.getString("name");
    boolean gender = rs.getBoolean("gender");
    Date birthday = rs.getDate("birthday");
    //5) 输出的控制台上
    System.out.println("编号：" + id + ", 姓名：" + name + ", 性别：" + gender + ", 生日：" +
    birthday);
    }
    //6) 释放资源
    rs.close();
    statement.close();
    connection.close();
}
```

关于ResultSet 接口中的注意事项：
1) 如果光标在第一行之前，使用rs.getXX()获取列值，报错：Before start of result set
2) 如果光标在最后一行之后，使用rs.getXX()获取列值，报错：After end of result set
3) 使用完毕以后要关闭结果集ResultSet，再关闭Statement，再关闭Connection

# JdbcUtils

数据库工具类JdbcUtils

如果一个功能经常要用到，我们建议把这个功能做成一个工具类，可以在不同的地方重用。

代码中出现了很多重复的代码，可以把这些公共代码抽取出来。

创建类JdbcUtil 包含3 个方法：
1) 可以把几个字符串定义成常量：用户名，密码，URL，驱动类
2) 得到数据库的连接：getConnection()
3) 关闭所有打开的资源：
close(Connection conn, Statement stmt)，close(Connection conn, Statement stmt, ResultSet rs)

```
public class JdbcUtils {
    //可以把几个字符串定义成常量：用户名，密码，URL，驱动类
    private static final String USER = "root";
    private static final String PWD = "root";
    private static final String URL = "jdbc:mysql://localhost:3306/day24";
    private static final String DRIVER= "com.mysql.jdbc.Driver";
    /**
    * 注册驱动
    */
    static {
        try {
        Class.forName(DRIVER);
        } catch (ClassNotFoundException e) {
        e.printStackTrace();
        }
    }
    /**
    * 得到数据库的连接
    */
    public static Connection getConnection() throws SQLException {
    	return DriverManager.getConnection(URL,USER,PWD);
    }
    /**
    * 关闭所有打开的资源
    */
    public static void close(Connection conn, Statement stmt) {
        if (stmt!=null) {
            try {
            stmt.close();
            } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    if (conn!=null) {
        try {
        conn.close();
        } catch (SQLException e) {
        e.printStackTrace();
        }
    }
    }
    /**
    * 关闭所有打开的资源
    */
    public static void close(Connection conn, Statement stmt, ResultSet rs) {
        if (rs!=null) {
        try {
        rs.close();
        } catch (SQLException e) {
        e.printStackTrace();
        13 / 21
        }
    }
    close(conn, stmt);
}
}
```

# PreparedStatement

PreparedStatement是Statement接口的子接口，继承于父接口中所有的方法。它是一个预编译的SQL 语句,优点：

1. prepareStatement()会先将SQL语句发送给数据库预编译。PreparedStatement会引用着预编译后的结果。可以多次传入不同的参数给PreparedStatement对象并执行。减少SQL编译次数，提高效率。
2. 安全性更高，没有SQL注入的隐患。
3. 提高了程序的可读性

Connection 创建PreparedStatement 对象:

```
PreparedStatement prepareStatement(String sql) 
```

指定预编译的SQL语句，SQL语句中使用占位符?创建一个语句对象.

PreparedStatement 接口中的方法：

```
int executeUpdate() 执行DML，增删改的操作，返回影响的行数。
ResultSet executeQuery() 执行DQL，查询的操作，返回结果集
```

## 举例
1)编写SQL语句，未知内容使用?占位："SELECT * FROM user WHERE name=? AND password=?";
2)获得PreparedStatement对象
3)设置实际参数：setXxx(占位符的位置,真实的值)
4)执行参数化SQL语句
5)关闭资源

```
public class DemoLogin {
    //从控制台上输入的用户名和密码
    public static void main(String[] args) throws SQLException {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名：");
        String name = sc.nextLine();
        System.out.println("请输入密码：");
        String password = sc.nextLine();
        login(name, password);
    }
    /**
    * 登录的方法
    * @param name
    17 / 21
    * @param password
    */
    private static void login(String name, String password) throws SQLException {
        Connection connection = JdbcUtils.getConnection();
        //写成登录SQL语句，没有单引号
        String sql = "select * from user where name=? and password=?";
        //得到语句对象
        PreparedStatement ps = connection.prepareStatement(sql);
        //设置参数
        ps.setString(1, name);
        ps.setString(2,password);
        ResultSet resultSet = ps.executeQuery();
        if (resultSet.next()) {
        	System.out.println("登录成功：" + name);
        }
        else {
        	System.out.println("登录失败");
        }
        //释放资源,子接口直接给父接口
        JdbcUtils.close(connection,ps,resultSet);
    }
}
```

```
public class DemoStudent {
    public static void main(String[] args) throws SQLException {
        //创建学生对象
        Student student = new Student();
        18 / 21
        Connection connection = JdbcUtils.getConnection();
        PreparedStatement ps = connection.prepareStatement("select * from student where id=?");
        //设置参数
        ps.setInt(1,2);
        ResultSet resultSet = ps.executeQuery();
        if (resultSet.next()) {
            //封装成一个学生对象
            student.setId(resultSet.getInt("id"));
            student.setName(resultSet.getString("name"));
            student.setGender(resultSet.getBoolean("gender"));
            student.setBirthday(resultSet.getDate("birthday"));
        }
        //释放资源
        JdbcUtils.close(connection,ps,resultSet);
       
        System.out.println(student);
        }
}
```

```
public class Demo10List {
    public static void main(String[] args) throws SQLException {
        //创建一个集合
        List<Student> students = new ArrayList<>();
        Connection connection = JdbcUtils.getConnection();
        PreparedStatement ps = connection.prepareStatement("select * from student");
        //没有参数替换
        ResultSet resultSet = ps.executeQuery();
        while(resultSet.next()) {
            //每次循环是一个学生对象
            Student student = new Student();
            //封装成一个学生对象
            student.setId(resultSet.getInt("id"));
            19 / 21
            student.setName(resultSet.getString("name"));
            student.setGender(resultSet.getBoolean("gender"));
            student.setBirthday(resultSet.getDate("birthday"));
            //把数据放到集合中
            students.add(student);
        }
        //关闭连接
        JdbcUtils.close(connection,ps,resultSet);
        //使用数据
        for (Student stu: students) {
        	System.out.println(stu);
        }
    }
}

```

DML:

```
public class DemoDML {
    public static void main(String[] args) throws SQLException {
        //insert();
        //update();
        delete();
    }
    //插入记录
    private static void insert() throws SQLException {
        Connection connection = JdbcUtils.getConnection();
        PreparedStatement ps = connection.prepareStatement("insert into student
        values(null,?,?,?)");
        ps.setString(1,"小白龙");
        ps.setBoolean(2, true);
        ps.setDate(3,java.sql.Date.valueOf("1999-11-11"));
        int row = ps.executeUpdate();
        System.out.println("插入了" + row + "条记录");
        JdbcUtils.close(connection,ps);
    }
    //更新记录: 换名字和生日
    private static void update() throws SQLException {
        Connection connection = JdbcUtils.getConnection();
        PreparedStatement ps = connection.prepareStatement("update student set name=?, birthday=?
        where id=?");
        ps.setString(1,"黑熊怪");
        ps.setDate(2,java.sql.Date.valueOf("1999-03-23"));
        ps.setInt(3,5);
        int row = ps.executeUpdate();
        System.out.println("更新" + row + "条记录");
        JdbcUtils.close(connection,ps);
    }
    20 / 21
    //删除记录: 删除第5条记录
    private static void delete() throws SQLException {
        Connection connection = JdbcUtils.getConnection();
        PreparedStatement ps = connection.prepareStatement("delete from student where id=?");
        ps.setInt(1,5);
        int row = ps.executeUpdate();
        System.out.println("删除了" + row + "条记录");
        JdbcUtils.close(connection,ps);
    }
}

```

# JDBC处理事务

```
void setAutoCommit(boolean autoCommit) 
参数是true或false
如果设置为false，表示关闭自动提交，相当于开启事务
```

```
void commit() 提交事务
void rollback() 回滚事务
```

步骤：

1)获取连接
2)开启事务
3)获取到PreparedStatement
4)使用PreparedStatement执行两次更新操作
5)正常情况下提交事务
6)出现异常回滚事务
7)最后关闭资源

```
public class Demo12Transaction {
    //没有异常，提交事务，出现异常回滚事务
    public static void main(String[] args) {
        21 / 21
        //1) 注册驱动
        Connection connection = null;
        PreparedStatement ps = null;
        try {
            //2) 获取连接
            connection = JdbcUtils.getConnection();
            //3) 开启事务
            connection.setAutoCommit(false);
            //4) 获取到PreparedStatement
            //从jack扣钱
            ps = connection.prepareStatement("update account set balance = balance - ? where
            name=?");
            ps.setInt(1, 500);
            ps.setString(2,"Jack");
            ps.executeUpdate();
            //出现异常
            System.out.println(100 / 0);
            //给rose加钱
            ps = connection.prepareStatement("update account set balance = balance + ? where
            name=?");
            ps.setInt(1, 500);
            ps.setString(2,"Rose");
            ps.executeUpdate();
            //提交事务
            connection.commit();
            System.out.println("转账成功");
            } catch (Exception e) {
            e.printStackTrace();
            try {
            //事务的回滚
            connection.rollback();
        } catch (SQLException e1) {
        e1.printStackTrace();
        }
        System.out.println("转账失败");
      }
        finally {
        //7) 关闭资源
        JdbcUtils.close(connection,ps);
    }
}

```

# 数据库连接池

参考：https://blog.csdn.net/crankz/article/details/82874158

数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个。

数据库连接是一种关键的有限的昂贵的资源，这一点在多用户的网页应用程序中体现得尤为突出。  一个数据库连接对象均对应一个物理数据库连接，每次操作都打开一个物理连接，使用完都关闭连接，这样造成系统的 性能低下。 数据库连接池的解决方案是在应用程序启动时建立足够的数据库连接，并讲这些连接组成一个连接池(简单说：在一个“池”里放了好多半成品的数据库联接对象)，由应用程序动态地对池中的连接进行申请、使用和释放。对于多于连接池中连接数的并发请求，应该在请求队列中排队等待。并且应用程序可以根据池中连接的使用率，动态增加或减少池中的连接数。 连接池技术尽可能多地重用了消耗内存地资源，大大节省了内存，提高了服务器地服务效率，能够支持更多的客户服务。通过使用连接池，将大大提高程序运行效率，同时，我们可以通过其自身的管理机制来监视数据库连接的数量、使用情况等。 
一次访问的时候，需要建立连接。 但是之后的访问，均会**复用**之前创建的连接，直接执行SQL语句。

步骤：

        第一、连接池的建立。一般在系统初始化时，连接池会根据系统配置建立，并在池中创建了几个连接对象，以便使用时能从连接池中获取。连接池中的连接不能随意创建和关闭，这样避免了连接随意建立和关闭造成的系统开销。Java中提供了很多容器类可以方便的构建连接池，例如Vector、Stack等。
        第二、连接池的管理。连接池管理策略是连接池机制的核心，连接池内连接的分配和释放对系统的性能有很大的影响。其管理策略是：
    
        当客户请求数据库连接时，首先查看连接池中是否有空闲连接，如果存在空闲连接，则将连接分配给客户使用；如果没有空闲连接，则查看当前所开的连接数是否已经达到最大连接数，如果没达到就重新创建一个连接给请求的客户；如果达到就按设定的最大等待时间进行等待，如果超出最大等待时间，则抛出异常给客户。
    
        当客户释放数据库连接时，先判断该连接的引用次数是否超过了规定值，如果超过就从连接池中删除该连接，否则保留为其他客户服务。
    
        该策略保证了数据库连接的有效复用，避免频繁的建立、释放连接所带来的系统资源开销。
    
        第三、连接池的关闭。当应用程序退出时，关闭连接池中所有的连接，释放连接池相关的资源，该过程正好与创建相反。
使用连接池时，要配置一下参数：

最小连接数：是连接池一直保持的数据库连接,所以如果应用程序对数据库连接的使用量不大,将会有大量的数据库连接资源被浪费.
最大连接数：是连接池能申请的最大连接数,如果数据库连接请求超过次数,后面的数据库连接请求将被加入到等待队列中,这会影响以后的数据库操作
最大空闲时间
获取连接超时时间
超时重试连接次数

Druid 相对于其他数据库连接池的优点

强大的监控特性，通过Druid提供的监控功能，可以清楚知道连接池和SQL的工作情况。

1. 监控SQL的执行时间、ResultSet持有时间、返回行数、更新行数、错误次数、错误堆栈信息；

2. SQL执行的耗时区间分布。什么是耗时区间分布呢？比如说，某个SQL执行了1000次，其中0~1毫秒区间50次，1~10毫秒800次，10~100毫秒100次，100~1000毫秒30次，1~10秒15次，10秒以上5次。通过耗时区间分布，能够非常清楚知道SQL的执行耗时情况；

3. 监控连接池的物理连接创建和销毁次数、逻辑连接的申请和关闭次数、非空等待次数、PSCache命中率等。

4. 方便扩展。Druid提供了Filter-Chain模式的扩展API，可以自己编写Filter拦截JDBC中的任何方法，可以在上面做任何事情，比如说性能监控、SQL审计、用户名密码加密、日志等等。Druid集合了开源和商业数据库连接池的优秀特性，并结合阿里巴巴大规模苛刻生产环境的使用经验进行优化

 Druid：数据库连接池实现技术，由阿里巴巴提供
	1. 步骤：
		1. 导入jar包 druid-1.0.9.jar
		2. 定义配置文件：
			* 是properties形式的
			* 可以叫任意名称，可以放在任意目录下
		3. 加载配置文件Properties
		4. 获取数据库连接池对象：通过工厂来来获取  DruidDataSourceFactory
		5. 获取连接：getConnection
	* 代码：
		 //3.加载配置文件
        
        ```
        Properties pro = new Properties();
        InputStream is = DruidDemo.class.getClassLoader().getResourceAsStream("druid.properties");
        pro.load(is);
        ```
        
	   //4.获取连接池对象
	   
	   ```
	   DataSource ds = DruidDataSourceFactory.createDataSource(pro);
	   ```
	   
	   
	   //5.获取连接
	   
	   ```
	   Connection conn = ds.getConnection();
	   ```
	2. 定义工具类
		1. 定义一个类 JDBCUtils
		2. 提供静态代码块加载配置文件，初始化连接池对象
		3. 提供方法
			1. 获取连接方法：通过数据库连接池获取连接
			2. 释放资源
			3. 获取连接池的方法



```
* 代码：
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
		
		    /**
		     * 获取连接
		     */
		    public static Connection getConnection() throws SQLException {
		        return ds.getConnection();
		    }
		
		    /**
		     * 释放资源
		     */
		    public static void close(Statement stmt,Connection conn){
		       /* if(stmt != null){
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
		        }*/
		
		       close(null,stmt,conn);
		    }
```

# SpringJDBC

* Spring框架对JDBC的简单封装。提供了一个JDBCTemplate对象简化JDBC的开发
* 步骤：
	1. 导入jar包
	
	2. 创建JdbcTemplate对象。依赖于数据源DataSource
	
	```
		JdbcTemplate template = new JdbcTemplate(ds);
		```
		
	3. 调用JdbcTemplate的方法来完成CRUD的操作:

```
* update():执行DML语句。增、删、改语句
* queryForMap():查询结果将结果集封装为map集合，将列名作为key，将值作为value 将这条记录封装为一个map集合.注意：这个方法查询的结果集长度只能是1
* queryForList():查询结果将结果集封装为list集合.注意：将每一条记录封装为一个Map集合，再将Map集合装载到List集合中
* query():查询结果，将结果封装为JavaBean对象
		参数：RowMapper
		一般我们使用BeanPropertyRowMapper实现类。可以完成数据到JavaBean的自动封装
		new BeanPropertyRowMapper<类型>(类型.class)
* queryForObject()：查询结果，将结果封装为对象,一般用于聚合函数的查询
```

4. 练习：
	* 需求：
		1. 修改1号数据的 salary 为 10000
		2. 添加一条记录
		3. 删除刚才添加的记录
		4. 查询id为1的记录，将其封装为Map集合
		5. 查询所有记录，将其封装为List
		6. 查询所有记录，将其封装为Emp对象的List集合
		7. 查询总记录数

* 代码：
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