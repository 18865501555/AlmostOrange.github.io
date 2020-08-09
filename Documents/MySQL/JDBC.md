## `JDBC`

- Java DataBase Connectivity
  - Java数据库连接
- 学习JDBC主要学习如何在Java代码中执行SQL语句
- JDBC实际上是Sun公司所提供的一套连接数据库的API(应用程序编程接口)，JDBC接口中封装了一堆和数据库连接相关的抽象方法(只有方法声明没有方法实现)
- 为什么使用JDBC
  - Java程序员有可能连接多种数据库软件
  - 如果没有JDBC接口，每一种数据库都有可能提供一套不同的方法
  - 使用JDBC接口吧方法名在接口中定义好，各个数据库厂商根据此接口实现里面的方法
  - 这样Java程序员只需要学习JDBC接口的方法调用即可
  - 如果完全按照JDBC接口的标准写代码，就算将来更换数据库，代码也不需要改变

```xml
<!-- 连接MySQL数据库的POM依赖 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.15</version>
</dependency>
```

### Statement SQL执行对象

- 用于执行SQL语句的对象
- 执行SQL语句的方法有以下几种:
  - execute(sql);  此方法可以执行任意的SQL语句,但是建议执行DDL(数据定义语言, 数据库相关和表相关的SQL,包括:create drop alter等)
  - executeUpdate(sql); 此方法执行增删改相关的SQL (insert,delete,update)
  - executeQuery(sql); 此方法执行DQL(查询相关的SQL语句)

