![](https://tva1.sinaimg.cn/large/007S8ZIlly1gikhn6iq02j315y0u00xf.jpg)

## Spring对DAO提供了哪些支持

- Spring对JDBC等数据库访问技术编写DAO提供了以下几个重要支持
  - Spring对DAO异常提供了统一处理
  - Spring对DAO编写提供了支持的抽象类
  - 提高编程效率，减少JDBC编码量

## Spring对DAO异常支持

- Spring把特定某种技术的异常，如SQLException统一转化为自己的异常类型，这些异常以DataAccessException为父类。它们封装了原始异常对象，不会丢失原始错误信息
- DataAccessException继承与RuntimeException，是非检查异常，不会因为没有处理异常而出现编译错误
- 异常必须处理，可以用拦截器或者在界面层统一处理

## Spring对DAO编写支持

- Spring为了便于以一种一致的方式使用各种数据访问技术，如JDBC和Hibernate
- Spring提供了一套抽象的DAO类
- 这些抽象类提供了一些方法，通过它们可以获得与数据访问技术相关的数据源和其他配置信息
  - JdbcTemplate
    - 封装常用JDBC方法
  - HibernateTemplate
    - 封装常用Hibernate方法
  - JdbcDaoSupport
    - JDBC数据访问对象的基类
  - HibernateDaoSupport
    - Hibernate数据访问对象的基类

## JdbcDaoSupport

- JdbcDaoSupport是利用JDBC技术编写DAO的父类，通过该类提供的方法，可便于获取Connection和JdbcTemplate等对象信息
- JdbcDaoSupport使用时需要注入一个DataSource对象
- JdbcDaoSupport对代码有一定的侵入性

## JdbcTemplate

- JdbcTemplate封装了链接获取以及链接释放等工作，从而简化了我们对JDBC的使用，避免忘记关闭连接等错误
- JdbcTemplate提供了以下主要方法
  - queryForInt()
  - queryForObject()
  - query()
  - update()
  - execute()

## 如何编写DAO组件

- 基于JDBC技术编写DAO组件可以采用下面两种模式

  - DAO继承JdbcDaoSupport，通过getJdbcTemplate()方法获取JdbcTemplate对象，需要在DAO实现类中注入一个DataSource对象来完成JdbcTemplate的实例化
  - DAO不继承JdbcDaoSupport，在Spring容器中配置一个JdbcTemplate的bean，然后注入给DAO实现类

- 不继承JdbcDaoSupport的用法-----DAO实现类

  ```JAVA
  public class JdbcEmpDAO implements EmpDAO{
    private JdbcTemplate template;
    public void setTemplate(JdbcTemplate template){
      this.template=template;
    }
    public void add(Emp emp){
      String sql = "insert into ...";
      Object[] params ={emp.getName(),emp.getSalary...};
      template.update(sql,params);
    }
  }
  ```

- 不继承JdbcDaoSupport的用法-----DAO配置

  ```xml
  <bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
  	<property name="driverClassName" value="..."/>
    <property name="url" value="..."/>
    <property name="username" value="..."/>
    <property name="password" value="..."/>
  </bean>
  <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
  	<property name="dataSource" ref="myDataSource"></property>
  </bean>
  <bean id="jdbcEmpDao" class="org.dao.jdbcEmpDAO">
  	<property name="template" ref="JdbcTemplate"></property>
  </bean>
  ```

  也可以使用@Resource注解方式注入

## Spring+JDBC Template应用

### 基于SpringMVC和JDBC技术开发的主要步骤
  - 创建工程，搭建SpringMVC和JDBC技术环境
  - 基于JdbcTemplate实现DAO组件
  - 编写和配置SpringMVC的主要组件，例如Controller，HandlerMapping,ViewResolver等
  - 编写JSP视图组件，利用标签和表达式显示模型数据
  - 测试程序

### 如何搭建SpringMVC和JDBC技术环境

- 创建一个Web工程
- 添加JDBC相关技术环境
  - 引入数据库驱动包
  - 引入dbcp连接池开发包
- 添加SpringMVC相关技术环境
  - 引入Spring idc,jdbc,tx等支持的开发包
  - 引入Spring webmvc支持的开发包
  - 在src下添加applicationContext.xml配置文件
  - 在web.xml中配置DispatcheerServlet主控制器

### 如何基于JdbcTemplate实现DAO组件

- 根据数据表编写实体类
- 编写DAO接口和实现类
- 在Spring容器中配置DAO实现类
  - 定义DAO对象，注入JdbcTemplate
- 测试Spring容器的DAO组件

### 如何编写和配置SpringMVC的主要组件

- 编写Controller和请求处理方法
- 配置`<mvc:annotation-driven/>`支持@RequestMapping
- 配置Controller组件
  - 开启组件扫描，将Controller扫描到Spring容器
  - 需要DAO时采用注入方式使用
  - 在请求处理方法上使用@RequestMapping制定对应的请求
- 配置ViewResolver组件

### 在JSP视图组件中如何显示模型数据

- JSP可以使用JSTL标签库，需要引入开发包
- JSP可以使用EL表达式
- JSP可以使用SpringMVC的表单标签



