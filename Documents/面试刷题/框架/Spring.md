## 谈谈你对Spring的理解

- Spring是一个开源框架，为简化企业级应用开发而生
- Spring可以是使简单的JavaBean实现以前只有EJB才能实现的功能
- Spring是一个IOC和AOP容器框架

### Spring容器的主要核心

#### 控制反转(IOC)

- 传统的Java开发模式中，当需要一个对象时，我们会自己使用new或者getInstance等直接或者间接调用构造方法创建一个对象
- 在Spring开发模式中，Spring容器使用了工厂模式为我们创建了所需要的对象，不需要我们自己创建了，直接调用Spring提供的对象就可以了，这就是控制反转的思想

#### 依赖注入(DI)

- Spring使用JavaBean对象的set方法或者带参数的构造方法为我们在创建所需对象时将其属性自动设置所需要的值的过程，就是依赖注入的思想

#### 面向切面编程(AOP)

- 在面向对象编程(OOP)思想中，我们将事物纵向抽成一个个的对象
- 在面向切面编程中，我们将一个个的对象某些类似的方面横向抽成一个切面，对这个切面进行一些如权限控制、事务管理、记录日志等公用操作处理的过程
- AOP底层是动态代理，如果是接口采用JDK动态代理，如果是类采用CGLIB方式实现动态代理

---

## Spring中的设计模式

### 单例模式

- 在Spring的配置文件中设置bean默认为单例模式

- Spring中两种代理方式，若目标对象实现了若干接口，Spring使用JDK的`java.lang.reflect.Proxy`类代理
- 若目标兑现没有实现任何接口，Spring使用CGLIB库生成目标类的子类

### 模版方式模式

- 用来解决代码重复的问题
- 比如:RestTemplate、JmsTemplate、JpaTemplate

### 前端控制器模式

- Spring提供了前端控制器DispatherServlet来对请求进行分发

### 试图帮助

- Spring提供了一系列的JSP标签，高效宏来帮助分散的代码整合在视图中

### 依赖注入

- 贯穿于`BeanFactory/ApplacationContext`接口的核心理念

### 工厂模式

- 创建对象时不会对客户端暴露创建逻辑，并且是通过使用同一个接口来指向新创建的对象
- Spring中使用beanFactory来创建对象的实例

---

## Spring的常用注解

- Spring在2.5版本以后开始支持注解的方式来配置依赖注入
- 可以用注解的方式来代替xml中bean的描述
- 注解注入将会被容器在xml注入之前被处理，所以后者会覆盖掉前者对于同一个属性的处理结果
- 注解装配在Spring中默认是关闭的，所以需要在Spring的核心配置文件中配置才可以使用注解的装配模式`<context:annotation-config/>`
- @Required
  - 应用于设值方法
- @Autowired
  - 应用于有值设值方法、非设值方法、构造方法和变量
- @Qualifier
  - 和@Autowired搭配使用，用于消除特定bean自动装配的歧义

---

## 简单介绍一下SpringBean的生命周期

### bean定义

- 在配置文件里用`<bean></bean>`来定义

### bean初始化(两种)

- 在配置文件中通过指定`inti-method`属性来完成
- 实现`org.springframwork.beans.factory.InitializingBean`接口

### bean调用

- 有三种方式可以得到bean实例，并进行调用

### bean销毁(两种)

- 使用配置文件指定的`destroy-method`属性
- 实现`org.springframwork.bean.factory.DisposeableBean`接口

---

## Spring结构图

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gj7m1jt2luj31t00u0q9j.jpg)

### 核心容器

- 包括Core、Beans、Context、EL模块

#### Core模块

- 封装了框架依赖的最底层部分，包括资源访问、类型转换及一些常用工具类

#### Beans模块

- 提供了框架的基础部分，包括控制反转和依赖注入
- 其中Bean Factory是容器核心，本质是"工厂设计模式"的实现，而且无需编程实现"单例设计模式"，单例完全由容器控制，而且提倡面向接口编程，而非面向实现编程
- 所有应用程序对象及对象间关系由框架管理

#### Context模块

- 以Core和Beans为基础，集成Beans模块功能并添加资源绑定、数据验证、国际化、JavaEE支持、容器生命周期、事件传播等
- 核心接口是ApplicationContext

#### EL模块

- 提供强大的表达式语言支持
- 支持访问和修改属性值，方法调用
- 支持访问及修改数组、容器和索引器，命名变量
- 支持算数和逻辑运算
- 支持从Spring容器获取Bean
- 支持列表投影、选择和一般的列表聚合等

### AOP模块

- SpringAOP模块提供了符合AOP Alliance规范的面向切面的编程实现
- 提供比如日志记录、权限控制、性能统计等通用功能和业务逻辑分离的技术
- 并且能动态的把这些功能添加到需要的代码中
- 各司其职，降低业务逻辑和通用功能的耦合

### Aspects模块

- 提供了对AspectJ的集成
- AspectJ提供了比Spring ASP更强大的功能

### Test模块

### 数据访问/集成模块

- 包括了JDBC、ORM、OXM、JMS和事务管理

#### 事务模块

- 用于Spring管理事务，只要是Spring管理对象都能得到Spring管理事务的好处
- 无需在代码中进行事务控制，而且支持编程和声明性的事务管理

#### JDBC模块

- 提供了一个JDBC的样例模版，使用这些模版能消除传统冗长的JDBC编码还有必须的事务控制，而且能享受到Spring管理事务的好处

#### ORM模块

- 提供与流行的"对象-关系"映射框架的无缝集成，包括Hibernate、JPA、MyBatis等
- 可以使用Spring事务管理，无需额外控制事务

#### OXM模块

- 提供了一个对Object/XML映射实现，将Java对象映射成XML数据，或者将XML数据映射成Java对象，Object/XML映射实现包括JAXB、Castor、XMLBeans和XStream

#### JMS模块

- 用于JMS(Java Messaging Service),提供一套"消息生产者、消息消费者"模版用于更加简单的使用JMS，JMS用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信

### Web/Remoting模块

- 

#### Web模块

#### Web-Servlet模块

#### Web-Struts模块



