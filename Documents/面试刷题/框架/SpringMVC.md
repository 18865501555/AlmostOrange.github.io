## SpringMVC的工作原理

- 用户向服务器发送请求，请求被SpringMVC前端控制器DispatchServlet捕获
- DispatcherServlet对请求URL进行解析，得到请求资源标识符(URL)，然后根据该URL调用HandlerMapping将请求映射到处理器HandlerExcutionChain
- DispatchServlet根据获得Handler选择一个合适的HandlerAdapter适配器处理
- Handler堆数据处理完成以后将返回一个ModelAndView()对象给DisPatchServlet
- Handler返回的ModelAndView()只是一个逻辑视图并不是一个正式的视图，DispatcherServlet通过ViewResolver视图解析器将逻辑视图转化为真正的视图View
- DispatcherServlet通过model解析出ModelAndView()中的参数进行解析最终展现出完整的view并返回给客户端

---

## SpringMVC常用注解都有哪些

- @requestMapping
  - 用于请求url映射
- @RequestBody
  - 注解实现接收http请求的json数据，将json数据转换为Java对象
- @ResponseBody
  - 注解实现将controller方法返回对象转化为json响应给客户

---

## 如何开启注解处理器和适配器

- 在项目中一般会在`springmvc.xml`中通过开启`<mvc:annotaion-driven>`来实现注解处理器和适配器的开启

---

## 如何解决get和post乱码问题

- 解决post请求乱码

  - 在`web.xml`里配置一个`CharacterEncodingFilter`过滤器，设置为`utf-8`

- 解决get请求乱码(两种)

  - 修改tomcat配置文件添加编码与工程编码一致

  - 对参数重新编码

    ```java
    String userName = new String(Request.getParameter("userName").getBytes("ISO8859-1"),"utf-8");
    ```

---

