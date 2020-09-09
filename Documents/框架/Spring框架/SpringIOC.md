![](https://tva1.sinaimg.cn/large/007S8ZIlly1gikca8h6bkj30xc0hm409.jpg)

## IOC简介

### IOC概念

- IOC全称是Inversion Of Control，被译为**控制反转**
- IOC是指程序中对象的获取方式发生反转，由最初的new方式创建，转变为由第三方框架创建、注入(DI)，它降低了对象之间的耦合度
- Spring容器是采用DI方式实现了IOC控制
- IOC是Spring框架的基础和核心
- DI全称是Dependency Injection，被译为**依赖注入**
- DI的基本原理就是将一起工作具有关系的对象，通过构造方法参数或方法参数传入建立关联，因此容器的工作就是创建bean时注入那些依赖关系
- IOC是一种思想，而DI是实现IOC的主要技术途径
- DI主要有两种注入方式
  - Setter注入
  - 构造器注入

## IOC应用

### Setter注入

- 通过调用无参构造器或无参static工厂方法实例化bean之后，调用该bean的setter方法，即可实现setter方式的注入

  ```java
  public class Computer implements Serializable{
    private String mainboard;//主板
    public String getMainboard(){
      return mainboard;
    }
    public void setMainboard(String mainboard){
      this.mainboard = mainboard;
    }
    //其他代码
  }
  ```

- 在容器xml配置中，配置注入参数

  ```xml
  <bean id="computer" class="com.bean.Computer">
  	<property name = "mainboard" value="技嘉"/>
    <property name = "hdd" value="希捷"/>
    <property name = "ram" value="金士顿"/>
  </bean>
  ```

### 构造器注入

- 基于构造器的注入是通过调用带参数的构造器来实现的，容器在bean被实例化的时候，根据参数类型执行响应的构造器

  ```java
  public class MobilePhone implements Serializable{
    private String cpu;
    private String ram;
    public MobilePhone(String cpu,String ram){
      this.cpu = cpu;
      this.ram = ram;
    }
  }
  ```

- 构造器注入，可以强制给bean注入某些参数，比Setter注入更严格

- 按构造参数索引指定注入

  ```xml
  <bean id="phone" class="com.bean.MobilePhone">
  	<constructor-arg index="0" value="ARM"/>
    <constructor-arg index="1" value="2G"/>
  </bean>
  ```

### 自动装配

- Spring IOC容器可以自动装配(autowire)相互协作bean之间的关联关系，autowire可以针对单个bean进行设置，autowire的方便之处在于减少xml的注入配置

- 在xml配置文件中，可以在`<bean/>`元素中使用autowire属性指定自动装配规则，一共有五种类型值

  | 属性值      | 描述                                                         |
  | ----------- | ------------------------------------------------------------ |
  | no          | 禁用自动装配，默认值                                         |
  | byName      | 根据属性名自动装配。此选项将检查容器并根据名字查找与属性完全一致的bean，并将其与属性自动装配 |
  | byType      | 如果容器中存在一个与指定属性类型相同的bean，那么将与该属性自动装配 |
  | constructor | 与byType的方式类似，不同之处在于它应用与构造器参数           |
  | autodetect  | 通过bean类来决定是使用constructor还是byType方式进行自动装配。如果发现默认的构造器，那么将使用byType方式 |

- 配置示例

  ```xml
  <bean id="computer" class="com.bean.Computer">
  	<property name = "mainboard" value="技嘉"/>
    <property name = "hdd" value="希捷"/>
    <property name = "ram" value="金士顿"/>
  </bean>
  <bean id="phone" class="com.bean.MobilePhone">
  	<constructor-arg index="0" value="ARM"/>
    <constructor-arg index="1" value="2G"/>
  </bean>
  <bean id="student" class="com.bean.Student" autowire="byType"></bean>
  ```

  

