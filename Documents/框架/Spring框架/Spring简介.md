![](https://tva1.sinaimg.cn/large/007S8ZIlly1gik74f3wqyj31au0qaq79.jpg)

### 什么是Spring

- Spring是一个开源的轻量级的应用开发框架，其目的是用于简化企业级应用程序开发，降低侵入性
- Spring提供的IOC和AOP功能，可以将组件的耦合度降至最低，即解耦，便于系统日后的维护和升级
- Spring为系统提供了一个整体的解决方案，开发者可以利用它本身提供的功能外，也可以与第三方框架和技术整合应用，可以自由选择猜中哪种技术进行开发

### 为什么要用Spring

- Spring的本质是管理软件中的对象，即**创建对象**和维护**对象之间的关系**

### Spring的主要功能

- Spring Core(核心容器)
  - 核心容器提供Spring框架的基本功能。
  - Spring以bean的方式组织和管理Java应用中的各个组件及其关系。
  - Spring使用BeanFactory来产生和管理Bean，它是工厂模式的实现。
  - BeanFactory使用控制反转(IoC)模式将应用的配置和依赖性规范与实际的应用程序代码分开。
  - BeanFactory使用依赖注入的方式提供给组件依赖。

- Spring Context(Spring上下文)
  - Spring上下文是一个配置文件，向Spring框架提供上下文信息。
  - Spring上下文包括企业服务
    - 如JNDI、EJB、电子邮件、国际化、校验和调度功能。
- Spring AOP(Spring面向切面编程)
  - 通过配置管理特性，Spring AOP 模块直接将面向方面的编程功能集成到了 Spring框架中。
  - 可以很容易地使 Spring框架管理的任何对象支持 AOP。
  - Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。
  - 通过使用 Spring AOP，不用依赖 EJB 组件，就可以将声明性事务管理集成到应用程序中。
- Spring DAO模块(API)
  - DAO模式主要目的是将持久层相关问题与一般的的业务规则和工作流隔离开来。
  - Spring 中的DAO提供一致的方式访问数据库，不管采用何种持久化技术，Spring都提供一直的编程模型。
  - Spring还对不同的持久层技术提供一致的DAO方式的异常层次结构。
- Spring ORM模块(接口)
  - Spring 与所有的主要的ORM映射框架都集成的很好，包括hibernate、JDO实现、TopLink和IBatis SQL Map等。
  - Spring为所有的这些框架提供了模板之类的辅助类，达成了一致的编程风格。
- Spring Web模块(API和接口)
  - Web上下文模块建立在应用程序上下文模块之上，为基于Web的应用程序提供了上下文。
  - Web层使用Web层框架，可选的，可以是Spring自己的MVC框架，或者提供的Web框架
    - 如Struts、Webwork、tapestry和jsf。
- Spring MVC框架(Spring WebMVC)
  - MVC框架是一个全功能的构建Web应用程序的MVC实现。
  - 通过策略接口，MVC框架变成为高度可配置的。
  - Spring的MVC框架提供清晰的角色划分
    - 控制器
    - 验证器
    - 命令对象
    - 表单对象
    - 模型对象
    - 分发器
    - 处理器映射
    - 视图解析器
  - Spring支持多种视图技术。