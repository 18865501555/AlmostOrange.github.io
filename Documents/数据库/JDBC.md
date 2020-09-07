![](https://tva1.sinaimg.cn/large/007S8ZIlly1gii6ajw9luj30u01ukh7u.jpg)

## JDBC原理

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gii2dh053ij31340rmn10.jpg)

### JDBC标准

#### JDBC是什么

- Java Database Connectivity
  - Java访问数据库的解决方案
- 希望用相同的方式访问不同的数据库，以实现与具体数据库无关的Java操作界面
- JDBC定义一套标准接口，即访问数据库的通用API，不同的数据库厂商根据各自数据库的特点去实现这些接口

#### JDBC接口及数据库厂商实现

- 驱动管理
  - DriverManager
- 连接接口
  - Connection
  - DatabaseMetaData
- 语句对象接口
  - Statement
  - PreparedStatement
  - CallableStatement
- 结果集接口
  - ResultSet
  - ResultSetMetaData

#### JDBC工作原理

- JDBC定义接口
- 数据库厂商实现接口
- 程序员调用接口，实际调用的是底层数据库厂商的实现部分

#### JDBC工作过程

- 加载驱动，建立连接
- 创建语句对象
- 执行SQL语句
- 处理结果集
- 关闭连接

#### Driver接口及驱动类加载

- 驱动类加载方式(MySQL)
  - `Class.forName("com.mysql.cj.jdbc.Driver");
    - ("com.mysql.cj.jdbc.Driver")装载驱动类
    - 驱动类通过static块实现在DriverManager中的"自动注册"

#### Connection接口

```java
Class.forName("com.mysql.cj.jdbc.Driver");
Connenction connection = DriverManager.getConnection("url","user","password");
//NOTE:Connection只是接口。真正的实现是由数据库厂商提供的驱动包完成的
```

#### Statement接口

- Statement statement = connection.createStatement();
  - boolean flag = statement.execute(sql);
  - ResultSet resultSet = statement.executeQuery(sql);
  - int flag = statement.executeUpdate(sql);

#### ResultSet接口

- 执行查询SQL语句后返回的结果集，由ResultSet接口接收

- 常用处理方式

  - 遍历/判断是否有结果

  ```java
  String sql = "select*from emp";
  ResultSet resultSet = statement.executeQuery(sql);
  while(rs.next()){
    System.out.println(resultSet.getInt("empno")+
  ","+resultSet.getString("ename"));
  }
  ```

- 查询的结果存放在ResultSet对象的一系列行中
- ResultSet对象的最初位置在行首
- ResultSet.next()方法用来在行间移动
- ResultSet.getXXX()方法用来取得字段的内容

### 数据库厂商实现

#### Oracle实现

- 下载对应的数据库的驱动
  - `ojdbc6.jar/ojdbc14.jar`
- 将驱动类导入到项目中
  - Mavern
- 加载驱动类
  - `Class.forName("oracle.jdbc.OracleDriver")`

#### MySQL实现

- 下载对应的数据库的驱动
  - `mysql-connector-java-5.0.4-bin.jar`
- 将驱动类导入到项目中
  - Mavern
- 加载驱动类
  - `Class.forName("com.mysql.cj.jdbc.Driver")`

---

## JDBC基础

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gii329kllsj310w0u043d.jpg)

### 连接管理

#### 通过连接工具类获取连接

- 在工程中，编写一个访问数据库的工具类，此后所有访问数据库的操作，都从工具类中获取连接
- 两种方式
  - 直接把数据配置写在工具类中
  - 把数据库配置写在一个properties属性文件里，工具类读取属性文件，逐行获取数据库参数(推荐)

#### 通过属性文件维护连接属性

```xml
# 驱动类名
jdbc.driver=com.mysql.cj.jdbc.Driver
# 连接字符串
jdbc:mysql://localhost:3306/newdb3?charcterEncoding=utf-8&useSSL=false&serverTimezone=Asia/Shanghai
# 访问数据库的用户名
jdbc.user = root
# 访问数据库的密码
jdbc.password = root
```

#### 从类路径中加载属性文件

```java
// 获得当前类的路径，加载指定属性文件
properties.load(DBUtility.class.getClassLoader().getResourceAsStream(path));
```

#### 连接的关闭

```java
//在工具类中定义公共的关闭连接的方法
//所有访问数据库的应用，共享此方法
public static void closeConnection(Connection connection){
  if(connection != null){
    try{
      con.close();
    }catch(SQLException e){
      e.printStackTrace();
    }
  }
}
```

### 连接池技术

- 连接池也只是接口，具体实现由厂商来完成

#### 为什么要使用连接池

- 数据库连接的建立及关闭资源消耗巨大
- 传统数据哭访问方式
  - 一次数据库访问对应一个物理连接
  - 每次操作数据库都要打开、关闭该无力连接
  - 系统性能严重受损
- 解决方案
  - 数据库连接池(Connection Pool)
- 系统初始运行时，主动建立足够的连接，组成一个池。每次应用程序请求数据库连接是，无需重新打开连接，而是从池中取出已有的连接，使用完后，不再关闭，而是归还

#### 连接池中连接的释放与使用规则

- 应用启动时，创建初始化数目的连接
- 当申请的无连接可用或者达到指定的最小连接数，按增量参数值创建新的连接
- 为确保连接池中最小的连接数的策略
  - 动态检查
    - 定时检查连接池，一旦发现数量小于最小连接数，则补充响应的新连接，保证连接池正常运转
  - 静态检查
    - 空闲连接不足时，系统才检测是否达到最小连接数
- 按需分配，用过归还，空闲超时释放，获取超时报错

#### Apache DBCP连接池

- DBCP(DataBase connection pool)
  - 数据库连接池
- Apache的一个Java连接池开源项目，同时也是Tomcat使用的连接池组件
- 连接池是创建和管理连接的缓冲池技术，将连接准备好被任何需要它们的应用使用
- 需要两个jar包文件
  - commons-dbcp-1.4.jar连接池的实现
  - commons-pool-1.5.jar连接池实现的依赖库
- 利用Maven导入

#### 通过DataSource获取连接

```java
private static BasicDateSource dataSource = new BasicDataSource();
dataSource.setDriverClassName(driveClassName);
dataSource.setUrl(url);
dataSource.setUserName(username);
dataSource.setPassword(password);
Connection connection  = dataSource.getConnection();
```

#### 连接池参数

- 常用参数

  - 初始连接数
  - 最大连接数
  - 最小连接数
  - 每次增加的连接数
  - 超时时间
  - 最大空闲连接
  - 最小空闲连接

- 根据应用需要，设置合适的值

- 重构DBUtils

  ```java
  import com.alibaba.druid.pool.DruidDataSource;
  import java.io.IOException;
  import java.io.InputStream;
  import java.sql.Connection;
  import java.util.Properties;
  
  /**
   * @author orange
   * @create 2020-07-21 1:45 下午
   */
  public class DBUtils {
      private static DruidDataSource ds;
      static {
          Properties p = new Properties();
          InputStream ips = DBUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
          //让对象和输入流关联
          try{
              p.load(ips);
          }catch (IOException e){
              e.printStackTrace();
          }
          //开始读取数据
          String driver = p.getProperty("db.driver");
          String url = p.getProperty("db.url");
          String name = p.getProperty("db.username");
          String password = p.getProperty("db.password");
          //读取最大链接数量和初始链接数量
          //从配置文件中 只能读取字符串
          String maxSize = p.getProperty("db.maxActive");
          String initSize = p.getProperty("db.initialSize");
          //创建链接池对象
          ds = new DruidDataSource();
          //设置链接信息
          ds.setDriverClassName(driver);
          ds.setUrl(url);
          ds.setUsername(name);
          ds.setPassword(password);
          //设置初始和最大数值
          ds.setInitialSize(Integer.parseInt(initSize));
          ds.setMaxActive(Integer.parseInt(maxSize));
      }
      public static Connection getConn() throws Exception {
          return ds.getConnection();
      }
  }
  ```

### 异常处理

#### SQLException简介

- java.sql.SQLException是在处理JDBC时常见的exception对象
- 用来表示JDBC操作过程中发生的具体错误
- 一般的SQLException都是因为操作数据库时出错，比如sql语句写错，或者数据库中的表或数据出错
- 常见异常
  - 登陆被拒绝
    - 程序里取键值对信息时的大小写和属性文件中不匹配
  - 列名无效
    - 查找的表和查找的列不匹配
  - 无效字符
    - sql语句语法有错，比如语句结尾时不能有分号
  - 无法转换为内部表示
    - 结果集取数据时注意数据类型
  - 表或视图不存在
    - 检查SQL中的表名是否正确
  - 不能将空值插入
    - 检查执行insert操作时，是否表有NOT NULL约束，而没有给出数据
  - 缺少表达式
    - 检查SQL语句的语法
  - SQL命令未正确结束
    - 检查SQL语句的语法
  - 无效数值
    - 企图将字符串类型的值填入数字型而造成，检查SQL语句
  - 文件找不到
    - db.properties文件路径不正确

#### 处理SQLException

- SQLException属于CheckedException
- 必须使用try...catch或throws明确处理

---

## JDBC核心API

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gii4mukkgfj31am0t4jvr.jpg)

### Statement

#### Statement执行查询

- 创建Statement的方式

  - Connection.createStatement();

- 执行INSERT,UPDATE和DELETE

  - Statement.executeUpdate();

- 执行SELECT

  - Statement.executeQuery();

  ```java
  String sql = "select empno,ename,sal,hiredate from emp";
  Statement statement = connection.createStatement();
  ResultSet result = statement.executeQuery(sql);
  statement.clost();
  ```

#### Statement执行新增

```java
String sql = "insert into emp(empno,ename,job,sal)values(1001,'XXX','Manager',9500)";
int flag = -1;
try{
  connection = ConnectionSource.getConnection();
  statement = connection.createStatement();
  flag = statement.executeUpdate(sql);
}catch(SQLException e){
  e.printStackTrace();
}
```

#### Statement执行修改

- 和INSERT操作完全相同，只是SQL语句不同

### PreparedStatement

#### PreparedStatement原理

- Statement主要用于执行静态SQL语句，即内容固定不变的SQL语句
- Statement每执行一次都要对传入的SQL语句编译一次，效率较差
- 某些情况下，SQL语句只是其中的参数有所不同，其余子句完全相同，适用于PreparedStatement
- 预防sql注入攻击
- PreparedStatement是接口，继承自Statement
- SQL语句提前编译，三种常用方法execute、executeQuery和executeUpdate已被更改，不再需要参数
- PreparedStatement实例包含已实现编译的SQL语句
- SQL语句可有一个或多个IN参数
  - IN参数的值在SQL语句创建时未被指定，该语句为每个IN参数保留一个问号("?")作为占位符
  - 每个问号的值必须在该语句执行之前，通过适当的setInt或者setString方法提供
- 由于PreparedStatement对象已预编译过，所以其执行速度要快于Statement对象。因此多次执行的SQL语句经常创建为PreparedStatement对象，以提高效率
- 批量处理

```java
//SQL语句已发送给数据库，并编译好为执行做好准备
PreparedStatement ps = connection.prepareStatement("UPDATE emp SET job=? WHERE empno=?");
//对占位符进行初始化
ps.setString(1,"Manager");
ps.setInt(2,1001);
//执行SQL语句
ps.executeUpdate();
```

#### 提升性能

- 数据库具备缓存功能，可以对statement的执行计划进行缓存，以免重复分析
- 缓存原理
  - 使用statement本身作为key并将执行计划存入与statement对应的缓存中
  - 对曾经执行过的statements，在运行时执行计划将重用
- 举例
  - SELECT a,b FROM t WHERE c = 1;
  - 再次发送相同的statement时，数据库会对先前使用过的执行计划进行重用，降低开销
- 悲剧
  - SELECT a,b FROM t WHERE c=1;
  - SELECT a,b FROM t WHERE c=2;
  - 被视作不同的SQL语句，执行计划不可重用
- 提升性能
  - SELECT a,b FROM t WHERE c=?
  - 被视作同一个的SQL语句，执行计划可重用

#### SQLInjection简介

- 场景

  ```java
  String sql = "select*from t where username='"+name+"'and password='"+password+"'";
  ```

  - 输入参数后，数据库接收到的完整sql语句是

  ```mysql
  select*from t where username='scott' and password='tiger';
  ```

  ---

  - 如果用户输入的password参数是`a'or'b'='b`则数据库收到的SQL语句将是

  ```mysql
  select*from t where username = 'scott' and password = 'a'or'b'='b';
  ```

  - 此SQL语句的where条件将永远为true

- 这种现象被称作SQL注入

#### 防止SQL Injection

- 对JDBC而言，SQL注入攻击只对Statement有效，对PreparedStatement无效，因为PreparedStatement不允许在插入参数时改变SQL语句的逻辑结构

- 使用预编译的语句对象，用户传入的任何数据不会和原SQL语句发生匹配关系，无需对输入的数据做过滤

- 如果用户将"or1=1"传入赋值给占位符，SQL语句将无法执行

  ```mysql
  select*from t where username = ? and password = ?;
  ```

### ResultSet

#### 结果集遍历

- 常用的遍历方式

  ```java
  String sql = "select empno,ename,sal,hiredate from emp";
  result = statement.executeQuery(sql);
  while(result.next()){
    int empno = result.getInt("empno");
    String ename = result.getString("ename");
    double sal = result.getDouble("sal");
    Date hiredate = result.getDate("hiredate");
  }
  result.close();
  ```

#### ResultSetMetaData

- ResultSetMetaData

  - 数据结果集的元数据

- 和查询出来的结果集相关，从结果集(ResultSet)中获取

  ```java
  ResultSetMetaData rsm = result.getMetaData();
  int columnCount = rsm.getColumnCount();
  String columnName = null;
  for(int i=1;i<=columnCount;i++){
    columnName = rsm.getColumnName(i);
  }
  ```

---

## JDBC高级编程

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gii6eyerlqj31940tcq7a.jpg)

### 事务处理

#### 事务简介

- 事务(Transaction):数据库中保证交易可靠的机制
- JDBC支持数据库中的事务概念
- 在JDBC中，事务默认是自动提交的

#### JDBC事务API

- 相关API
  - Connection.getAutoCommit()
    - 获得当前事务的提交方式，默认为true
  - Connection.setAutoCommit()
    - 设置事务的提交属性，参数是
      - true自动提交
      - false不自动提交
  - Connection.commit();
    - 提交事务
  - Connection.rollback()
    - 回滚事务

### 事务四大特性（ACID）

#### 原子性（Atomicity）

- 原子性是指事务包含的所有操作要么全部成功，要么全部失败回滚
- 因此事务的操作如果成功就必须要完全应用到数据库
- 如果操作失败则不能对数据库有任何影响。

#### 一致性（Consistency）

- 事务开始前和结束后，数据库的完整性约束没有被破坏。
- 比如A向B转账，不可能A扣了钱，B却没收到。

#### 隔离性（Isolation）

- 隔离性是当多个用户并发访问数据库时
  - 比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离。
- 同一时间，只允许一个事务请求同一数据，不同的事务之间彼此没有任何干扰。
  - 比如A正在从一张银行卡中取钱，在A取钱的过程结束前，B不能向这张卡转账。

#### 持久性（Durability）

- 持久性是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的
- 即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。

#### JDBC标准事务编程模式

```java
try{
  //获得自动提交状态
  autoCommit = connection.getAutoCommit();
  //关闭自动提交
  connnection.setAutoCommit(false);
  //执行SQL语句
  statement.executeUpdate(sql1);
  statement.executeUpdate(sql2);
  //提交
  connection.commit();
  //将自动提交功能恢复到原来的状态
  connection.setAutoCommit(autoCommit);
}catch(SQLException e){
  //异常时会滚
  conn.rollback();
}
```

### 批量更新

#### 批量更新的优势

- 批处理
  - 发送到数据库作为一个单元执行的一组更新语句
- 批处理降低了应用程序和数据库之间的网络调用
- 相比单个SQL语句的处理，批处理更为有效

#### 批量更新API

- addBatch(String sql)
  - Statement类的方法，可以将多条sql语句添加Statement对象的SQL语句列表中
- addBatch()
  - PreparedStatement类的方法，可以将多条预编译的sql语句添加到PreparedStatement对象的SQL语句列表
- executeBatch()
  - 把Statement对象或PreparedStatement对象语句列表中的所有SQL语句发送给数据库进行处理
- clearBatch()
  - 清空当前SQL语句列表

#### 防止OutOfMemory

- 如果PreparedStatement对象中的SQL列表包含过多的待处理SQL语句，可能会产生OutOfMemory错误

- 及时处理SQL语句列表

  ```java
  for(int i=0;i<1000;i++){
    sql = "insert into emp(empno,ename)values(emp_seq.nextval,'name"+i+"'")";
  	//将SQL语句加入到Batch中
    statement.addBatch(sql);
    if(i%500==0){
      //及时处理
      statement.executeBatch();
      //清空列表
      statement.clearBatch();
    }
  }
  //最后一次列表不足500条，处理
  statement.executeBatch();
  ```

### 返回自动主键

#### 关联数据插入

- 主表/从表关联关系，插入数据时需要保证数据完整性

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gii832lm7cj30o20x2gn7.jpg)

#### 通过自增类型产生主键

```mysql
CREATE TABLE animals(
  id INT NOT NULL AUTO_INCREMENT,
	name CHAR(30) NOT NULL,
  PRIMARY KEY (id)
);
INSERT INTO animals(name)VALUES('dog'),('cat'),('penguin'),('lax'),('whale'),('ostrich');
SELECT * FROM animals;
```

#### JDBC返回自动主键API

- 利用PreparedStatement的getGeneratedKeys方法获取自增类型的数据

  ```java
  // statement第二个参数是GeneratedKeys的字段名列表，字符串数组
  sql = "insert into animals(name) values(?)";
  statement = con.prepareStament(sql,new String[]{"id"});
  statement.setString(1,"Cat");
  statement.executeUpdate();
  // 获得主键值所在的result
  result = statement.getGeneratedKeys();
  result.next();
  // 从rs中取出主键值，从表使用
  int id = result.getInt(1);
  ```

---

## DAO

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gii6f7c7l1j311k0ietav.jpg)

### 什么是DAO

#### DAO封装对数据的访问

- DAO(Data Access Object)数据访问对象
- 建立在数据库和业务层之间，封装所有对数据库的访问
- 目的
  - 数据访问逻辑和业务逻辑分开
- 为了建立一个健壮的Java应用，需将所有对数据源的访问操作抽象封装在一个公共API中，包括
  - 建立一个接口，接口中定义了应用程序中将会用到的所有事务方法
  - 建立接口的实现类，实现接口对应的所有方法，和数据库直接交互
- 在应用程序中，当需要和数据源交互时则使用DAO接口，不涉及任何数据库的具体操作
- DAO通常包括
  - 一个DAO工厂类
  - 一个DAO接口
  - 一个实现DAO接口的具体类
  - 数据传递对象(实体对象或值对象)

#### 实体对象

- DAO层需要定义对数据库中表的访问
- 对象关系映射(ORM:Object/Relation Mapping)描述对象和数据表之间的映射，将Java程序中的对象对应到关系数据库的表中
  - 表和类对应
  - 表中的字段和类的属性对应
  - 记录和对象对应

### 编写DAO

#### 查询方法

```java
public Account findById(Integer id) throws SQLException{
  Connection connection = getConnection();
  //预先定义好的SQL查询语句
  String sql = SELECT_BY_ID;
  PrepareStatement ps = connection.prepareStatement(sql);
	//传入参数
  ps.setInt(1,id);
  ResultSet result = ps.executeQuery();
  Account account = null;
  //处理结果集
  while(rs.next()){
    account = new Account();
    account.setId(rs.getInt("ACCOUNT_ID"));
    //设置account对象所有的属性...
  }
  return account;
}
```

#### 更新方法

```java
public boolean update(Account account) throws SQLException{
  Connection connection = getConnection();
  //预先定义好的SQL语句
  String sql = UPDATE_STATUS;
  PreparedStatement ps = connection.prepareStatement(sql);
	//传入参数
  ps.setInt(1,account.getId());
  ps.setString(2,account.getStatus());
  int flag = ps.executeUpdate();
  return (flag > 0 ? true : false);
}
```

#### 异常处理机制

- 多层系统的异常处理原则
  - 谁抛出的异常，谁捕捉处理，因为只有异常抛出者，知道怎样捕捉处理异常
  - 尽量在当前层中捕捉处理抛出的异常，尽量不要抛出到上层接口
  - 尽量在每层中封装每层的异常类，这要可准确定位异常抛出的层
  - 如异常无法捕捉处理，则向上层接口抛出，直至抛给JVM
  - 应尽量避免将异常抛给JVM