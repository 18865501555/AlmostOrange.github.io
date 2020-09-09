![](https://tva1.sinaimg.cn/large/007S8ZIlly1gik82zewetj315o0rm78b.jpg)

## 简介

### Spring容器简介

- 在Spring中，任何的Java类和JavaBean都被当成Bean处理，这些Bean通过容器管理和使用
- Spring容器实现了IOC和AOP机制，这些机制可以简化Bean对象创建和Bean对象之间的解耦
- Spring容器有BeanFactory和ApplicationContext两种类型

### 什么是JavaBean

- 一种简单规范的Java对象

### 何时使用Spring

- 当需要管理JavaBean对象时候就可以使用
- Spring是最简洁的对象管理方案之一

### 如何使用Spring

- 遵守Spring定义的规则
- 基于配置和默认规则
- 减少了代码的书写

### Spring容器的实例化(如何创建对象)

- ApplicationContext继承自BeanFactory接口，拥有更多的企业级方法，推荐使用该类型

- 实例化方法如下

  ```java
  //加载工程classpath下的配置文件实例化
  String conf = "applicationContext.xml";
  ApplicationContext ac = new ClassPathXmlApplicationContext(conf); 
  ```

### Spring容器的使用

- 从本质上讲，BeanFactory和ApplicationContext仅仅只是一个维护Bean定义以及相互依赖关系的高级工厂接口

- 通过BeanFactory和ApplicationContext我们可以访问bean定义

- 首先在容器配置文件applicationContext.xml中添加Bean定义

  ```xml
  <bean id = "标识符" class="Bean类型"/>
  ```

- 然后在创建BeanFactory和ApplicationContext容器对象后，调用getBean()方法获取Bean实例即可

  `getBean("标识符")`

## 容器对Bean的管理

### Bean的实例化

- Spring容器创建Bean对象的方法有以下三种

  - 用构造器来实例化
  - 使用静态工厂方法实例化
  - 使用实例工厂方法实例化

- 将对象创建规则告诉Spring，Spring会帮你取创建对象

- 基于配置和默认规则，减少了代码的书写

- 用构造器来实例化

  ```xml
  <bean id="calendarObj1" class="java.util.GregorianCalendar"/>
  <bean name="calendarObj2" class="java.util.GregorianCalendar"/>
  ```

  id或name书写用于指定Bean名称，用于从Spring中查找这个Bean对象

  class用于指定Bean类型，会自动调用无参数构造器创建对象

- 用静态工厂方法实例化

  ```xml
  <bean id="calendarObj2" class="java.util.Calendar" factory-method="getInstance"/>
  ```

  id属性用于指定Bean名称

  class属性用于指定工厂类型

  factory-method属性用于指定工厂中创建Bean对象的方法，必须用static修饰的方法

- 使用实例工厂方法实例化

  ```xml
  <bean id="calendarObj3" class="java.util.GregorianCalendar"/>
  <bean id="dateObj" factory-bean="calendarObj3" factory-method="getTime"/>
  ```

  id用于指定Bean名称

  factory-bean属性用于指定工厂Bean对象

  factory-method属性用于指定工厂中创建Bean对象的方法

### Bean的命名

- Bean的名称

  - 在Spring容器中，每个Bean都需要有名字(即标识符)，该名字可以用`<bean>`元素的id或name属性指定

- Bean的别名

  - 为已定义好的Bean，再增加另外一个名字引用，可以使用`<alias>`指定

    ```xml
    <alias name="fromName" alias="toName"/>
    ```

### Bean的作用域

- Spring容器在实例化Bean时，可以创建一下作用域的Bean对象

  | 作用域         | 描述                                                         |
  | -------------- | ------------------------------------------------------------ |
  | singleton      | 在每个Spring IOC容器中一个bean定义对应一个对象实例。默认项   |
  | prototype      | 一个bean定义对应多个对象实例                                 |
  | request        | 在一次HTTP请求中，一个bean定义对应一个实例。仅限于Web环境    |
  | session        | 在一个HTTP Session中，一个bean定义对应一个实例。仅限于Web实例 |
  | global Session | 在一个全局的HTTP Session中，一个bean定义对应一个实例:仅在基于portlet的Web应用中才有意义Portlet规范定义了全局Session的概念 |

  上面的Bean作用域，可以通过`<bean>`定义的scope属性指定

### Bean的声明周期回调

- 指定初始化回调方法

  ```xml
  <bean id="exampleBean" class="com.foo.ExampleBean" init-method="init"></bean>
  ```

- 指定销毁回调方法，仅适用于singleton模式的bean

  ```xml
  <bean id="exampleBean" class="com.foo.ExampleBean" destroy-method="destroy"></bean>
  ```

- Spring会管理对象的创建过程

- 在顶级的`<beans/>`元素中的default-init-method属性，可以为容器所有`<bean>`指定初始化回调方法

  ```xml
  <beans default-init-method="init">
  	<bean id="exampleBean" class="com.foo.ExampleBean"/>
  </beans>
  ```

- 在顶级的`<beans/>`元素中的default-destroy-method属性，可以为容器所有`<bean>`指定销毁回调方法

  ```xml
  <beans default-destroy-method="destroy">
  	<bean id="exampleBean" class="com.foo.ExampleBean"/>
  </beans>
  ```

### Bean延迟实例化

- 在ApplicationContext实现的默认行为就是在启动时讲所有singleton bean提前进行实例化

- 如果不想让一个singleton bean在ApplicationContext初始化时被提前实例化，可以使用`<bean>`元素的lazy-init="true"属性改变

- 一个延迟初始化bean将在第一次被用到时实例化

  ```xml
  <bean id="exampleBean" lazy-init="true" class="com.foo.ExampleBean"/>
  ```

- 在顶级的`<beans/>`元素中的default-lazy-init属性，可以为容器所有`<bean>`指定延迟实例化特性

- 适用于使用频率很低的单例对象

