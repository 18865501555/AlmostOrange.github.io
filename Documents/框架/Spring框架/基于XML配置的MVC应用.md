![](https://tva1.sinaimg.cn/large/007S8ZIlly1gikg56r35ej315k0u0jvw.jpg)

## 搭建Spring Web MVC环境

- 搭建Spring Web MVC开发环境的主要步骤如下
  - 创建Web工程，导入Spring Web MVC相关开发包
    - Spring API、web、webmvc等开发包
  - 在src下添加Spring的XML配置文件
    - 名称可以自定义，例如spring-mvc.xml
  - 在web.xml中配置DispatcherServlet前端控制器组件
    - DispatcherServlet组件在Spring mvc中已提供，只需要配置即可
    - 配置DispatcherServlet时，同时指定XML配置文件

## DispatcherServlet控制器

```xml
<servlet>
	<servlet-name>springmvc</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <init-param>
  	<param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring-mvc.xml</param-value>
  </init-param>
  <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
	<servlet-name>springmvc</servlet-name>
  <url-pattern>*.from</url-pattern>
</servlet-mapping>
```

## Controller组件

- Controller组件负责执行具体的业务处理，可调用DAO等组件，编写时需要实现Controller接口及约定方法

  ```java
  public class HelloController implements Controller{
    public ModelAndView handleRequest(
      HttpServletRequest req,
    	HttpServletResponse res) throws Exception{
      System.out.println("HelloSpring!");
      return new ModelAndView("hello");
    }
  }
  ```

## ModelAndView组件

- Controller组件约定的handleRequest方法执行后返回一个ModelAndView对象，该对象可封装模型数据和视图名响应信息
- ModelAndView构造器如下
  - ModelAndView(String viewName)
  - ModelAndView(String viewName,Map model)
    - viewName是jsp页面的名字
    - model的数据存储到request的attribute中

## HandlerMapping组件

- 通过HandlerMapping组件，DispatcherServlet控制器可以将客户HTTP请求映射到Controller组件上

  - SimpleUrlHandlerMapping

    - 维护一个HTTP请求和Controller映射关系列表(map)

    - 根据列表对应关系调用Controller

    - 使用定义如下

      ```xml
      <bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
      	<property name="mappings">
        	<props>
          	<prop key="/login.form">loginController</prop>
            <prop key="/hello.form">helloController</prop>
          </props>
        </property>
      </bean>
      <bean id="helloController" class="org.web.HelloController"/>
      ```

  - RequestMappingHandlerMapping

  - RequestMappingHandlerAdapter

    - 在Controller类和方法上使用@RequestMapping注解指定对应的客户HTTP请求

## ViewResolver组件

- 所有Controller组件都返回一个ModelAndView实例，封装了视图名

- Spring中的视图以名字为标识，视图解析器ViewResolver通过名字来解析视图

- Spring提供了多种视图解析器

  | `ViewResolver`类型                              | 功能描述                                                     |
  | ----------------------------------------------- | ------------------------------------------------------------ |
  | `UrlBasedViewResolver`                          | 将视图名直接解析成对应的`URL`，不需要显示的映射定义。如果你的视图名和视图资源名字是一致的，就可使用该解析起，而无需进行映射 |
  | `InternalResourceViewResolver`                  | `UrlBasedViewResolver`的子类,它支持`InternalResourceView`(对`Servlet`和`JSP`的包装)，以及其子类`IstlView`和`TilesView`响应类型 |
  | `XmlViewResolver`                               | 支持用xml文件定义具体的响应视图文件                          |
  | `VelocityViewResolver`/`FreeMarkerViewResolver` | `UrlBasedViewResovler`的子类，它能支持`Velocity`和`FreeMarker`等视图技术 |

- InternalResourceViewResolver使用示例

  ```xml
  <bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
  	<property name="prefix" value="/WEB-INF/jsp/"/>
    <property name="suffix" value=".jsp"/>
  </bean>
  ```

  如：视图名hello通过以上配置可以映射到/WEB-INF/jsp/hello.jsp

