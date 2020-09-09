![](https://tva1.sinaimg.cn/large/007S8ZIlly1gikgw1aoh4j31ac0cygn9.jpg)

## @Controller注解应用

- 推荐使用@Controller注解声明Controller组件，这样可以是的Controller定义更加灵活，可以不用实现Controller接口，请求处理的方法也可以灵活定义

  ```java
  @Controller
  public class HelloController{
    public String execute() throws Exception{
      return "hello";
    }
  }
  ```

- 为了使@Controller注解生效，需要在Spring的XML配置文件中开启组件扫描定义，并制定Controller组件所在包

  ```xml
  <context:component-scan base-package="com.controller"/>
  ```

## @RequestMapping注解应用

- @RequestMapping可以用在类定义和方法定义上

- @RequestMapping标明这个类或方法与哪一个客户请求对应

  ```java
  @Controller
  @RequestMapping("/day01")
  public class HelloController{
    @RequestMapping("/hello.form")
    public String execute() throws Exception{
      return "hello";
    }
  }
  ```

- 开启@RequestMapping注解映射，需要在Spring的XML配置文件中定义两个bean组件

  - RequestMappingHandlerMapping(类定义前)

    ```xml
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
    ```

  - RequestMappingHandlerAdapter(方法定义前)

    ```xml
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>
    ```

- 提示:Spring3.1版本之前需要定义两个组件

  - DefaultAnnotationHandlerMapping
  - AnnotationMethodHandlerAdapter

- 从Spring3.2版本开始可以使用下面XML配置简化定义`<mvc:annotation-driven/>`
  - RequestMappingHandlerMapping
  - RequestMappingHandlerAdapter