## `MyBatis`框架概述

---

### 什么是`mybatis`

- 是一个**持久层框架**
- 用`Java`编写的
- 封装了`jdbc`操作的很多细节，是开发者只需要关注`sql`语句本身，而无需关注注册驱动，创建连接等繁杂过程
- 使用了`ORM`思想实现了结果集的**封装**

- #### `ORM`

  - `Object Relational Mapping`对象关系映射

  - 把数据库表和实体类及实体类的属性对应起来，让我们可以操作实体类就实现操作数据库表

    | `user`表    | `User`类   |
    | ----------- | ---------- |
    | `id`        | `userId`   |
    | `user_name` | `userName` |

### `mybatis`的环境搭建

- 创建`maven`工程并导入坐标

  <img src="https://tva1.sinaimg.cn/large/007S8ZIlly1ghhjnzgvpxj30d40hy75f.jpg" style="zoom:50%;" />

- 创建`实体类`和`dao`的接口

  - `实体类`

    ```java
    package com.domain;
    
    import java.io.Serializable;
    import java.util.Date;
    
    public class User implements Serializable {
        private Integer id;
        private String username;
        private Date birthday;
        private String sex;
        private String address;
    
        public Integer getId() {return id;}
        public void setId(Integer id) {this.id = id;}
        public String getUsername() {return username;}
        public void setUsername(String username) {this.username = username;}
        public Date getBirthday() {return birthday;}
        public void setBirthday(Date birthday) {this.birthday = birthday;}
        public String getSex() {return sex;}
        public void setSex(String sex) {this.sex = sex;}
        public String getAddress() {return address;}
        public void setAddress(String address) {this.address = address;}
        @Override
        public String toString() {
            return "User{" +
                    "id=" + id +
                    ", username='" + username + '\'' +
                    ", birthday=" + birthday +
                    ", sex='" + sex + '\'' +
                    ", address='" + address + '\'' +
                    '}';
        }
    }
    ```

  - `dao`

    ```java
    package com.dao;
    
    import com.domain.User;
    import java.util.List;
    
    /**
     * 用户的持久层接口
     */
    public interface IUserDao {
        /**
         * 查询所有操作
         */
        List<User> findAll();
    }
    ```

    

- 创建`Mybatis`的主配置文件

  - `SqlMapConifg.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <!DOCTYPE configuration
            PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <!--mybatis的主配置文件-->
    <configuration>
        <!--配置环境-->
        <environments default="mysql">
            <!--配置mysql的环境-->
            <environment id="mysql">
                <!--配置事务的类型-->
                <transactionManager type="JDBC"></transactionManager>
                <!--配置数据源(链接池)-->
                <dataSource type="POOLED">
                    <!--配置链接数据库的4个基本信息-->
                    <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                    <property name="url" value="jdbc:mysql://localhost:3306/eesy_mybatis"/>
                    <property name="username" value="root"/>
                    <property name="password" value="root"/>
                </dataSource>
            </environment>
        </environments>
        <!--指定映射配置文件的位置，映射配置文件值得是每个dao独立的配置文件-->
        <mappers>
            <mapper resource="com/dao/IUserDao.xml"></mapper>
        </mappers>
    </configuration>
    ```

    

- 创建映射配置文件

  - `IUserDao.xml`

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.dao.IUserDao">
        <!--配置查询所有-->
        <select id="findAll" resultType="com.domain.User">
            select * from user
        </select>
    </mapper>
    ```

- `Pom`依赖

  ```xml
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.5</version>
  </dependency>
  <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.21</version>
  </dependency>
  ```

  

- #### 环境搭建的注意事项

  - 在`Mybatis`中把持久层的操作接口名称和映射文件也叫做:`Mapper`
    - 所以:`xxxDao`和`xxxMapper`是一样的
  - `Mybatis`的映射配置文件位置必须和`dao`接口的包结构相同
  - 映射配置文件的`mapper`标签`namespace`属性的取值必须是`dao`接口的全限定类名
  - 映射配置文件的操作配置(`select`)，`id`属性的取值必须是`dao`接口的方法名

  当我们遵从了上述注意事项之后，我们在开发中就无需再写`dao`的实现类

### `mybatis`案例

- 读取配置文件

- 创建`SqlSessionFactory`工厂

- 创建`SqlSession`

- 创建`Dao`接口的代理对象

- 执行`dao`中的方法

- 释放资源

  ```java
  public class MybatisTest {
      public static void main(String[] args) throws Exception {
          //读取配置文件
          InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
          //创建SqlSessionFactory工厂
          SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
          SqlSessionFactory factory = builder.build(in);
          //使用工厂生产一个SqlSession对象
          SqlSession session = factory.openSession();
          //使用SqlSession创建Dao接口的代理对象
          IUserDao userDao = session.getMapper(IUserDao.class);
          //使用代理对象执行方法
          List<User> users = userDao.findAll();
          users.forEach(user->System.out.println(user));
          //释放资源
          session.close();
          in.close();
      }
  }
  ```

  

注意事项：

- 不要忘记在映射配置中告知`mybatis`要封装到哪个实体类中

- 配置的方式

  - 指定实体类的全限定类名

- #### `mybatis`基于注解的案例

  - 把`IUserDao.xml`移除，在`dao`接口的方法上使用`@Select`注解，并且制定`SQL`语句

    ```java
    public interface IUserDao {
        /**
         * 查询所有操作
         */
        @Select("select*from user")
        List<User> findAll();
    }
    ```
    
    
    
  - 同时需要在`SqlMapConfig.xml`中的`mapper`配置时，使用`class`属性置顶`dao`接口的全限定类名
  
    ```xml
    <!--指定映射配置文件的位置，映射配置文件值得是每个dao独立的配置文件
            如果是使用注解来配置的话，此处应该使用class属性指定被注解的dao全限定类名
        -->
      <mappers>
            <mapper class="com.dao.IUserDao"></mapper>
      </mappers>
    ```

    

明确：

- 在实际开发中，都是越简便越好，所以都是采用不写`dao`实现类的方式
- 不管使用`XML`还是注解配置
- 但是`Mybatis`是支持写`dao`实现类的