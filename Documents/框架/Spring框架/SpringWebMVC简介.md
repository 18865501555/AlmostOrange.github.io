![](https://tva1.sinaimg.cn/large/007S8ZIlly1gikfoj4ev7j31ao0n8juj.jpg)

## MVC模式简介

- M-Model模型
  - 模型(Model)的职责是负责业务逻辑
  - 包含两层
    - 业务数据
    - 业务处理逻辑
  - 比如实体类、DAO、Service都属于模型层
- V-View视图
  - 视图(View)的职责是负责显示界面和用户交互(收集用户信息)
  - 属于视图的组件是不包含业务逻辑和控制逻辑的JSP
- C-Controller控制器
  - 控制器是模型层M和视图层V之间的桥梁，用于控制流程

## 什么是Spring Web MVC

- Spring web MVC是Spring框架一个非常重要的功能模块
- 实现了MVC结构，便于简单、快速开发MVC结构的Web程序
- Spring web MVC提供的API封装了Web开发中常用的功能，简化了Web过程

## Spring Web MVC的核心组件

- Spring Web MVC提供了M、V和C相关的主要实现组件，具体如下
  - DispatcherServlet(控制器，请求入口)
  - HandlerMapping(控制器，请求派发)
  - Controller(控制器，请求处理流程)
  - ModelAndView(模型，封装业务处理结果和视图)
  - ViewResolver(视图，视图显示处理器)

## Spring Web MVC的处理流程

- request -> HandlerMapping -> Controller -> ModelAndView -> viewResolver ->View ->response
- 浏览器向Spring发出请求，请求交给前端控制器DispatcherServlet处理
- 控制器通过HandlerMapping找到相应的Controller组件处理请求
- 执行Controller组件约定方法处理请求，在约定方法调用模型组件完成业务处理。约定方法可以返回一个ModelAndView对象，封装了处理结果数据和视图名称信息
- 控制器接收ModelAndView之后，调用ViewResolver组件，定位View(JSP)并传递数据信息，生成响应界面结果

