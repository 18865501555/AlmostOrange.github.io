## jQuery概述
![](https://tva1.sinaimg.cn/large/007S8ZIlly1gija6ufwjej318c0s00x6.jpg)

### jQuery简介

#### 什么是jQuery

- jQuery是一个优秀的JavaScript框架，一个轻量级的JS库
- 封装了JS、CSS、DOM，提供了一致的、简洁的API
- 兼容CSS3，及各种浏览器
- 使用户更方便地处理HTML、Events、实现动画效果，并且方便的为网站提供AJAX交互
- 使用户的HTML页面保持代码和HTML内容分离
- **jQuery是一个JS框架，极大的简化了JS编程**
- jQuery的核心理念是write less, do more，2006年1月发布

### 使用jQuery

#### 浏览器如何解析HTML文档

- 浏览器读取HTML->生成DOM树->渲染

#### 利用选择器定位节点

```html
<html>
  <head></head>
  <body>
    <!-- $("div")选择器定位 -->
		<!-- $("#d1")选择器定位 -->
    <div id="d1">Hello</div>
  </body>
</html>
```

#### 调用方法操作节点

```html
<html>
  <head></head>
  <body>
    <!-- $("div").css("font-size","30px")操作节点 -->
		<!-- $("#d1").css("font-size","30px")操作节点 -->
    <div id="d1">Hello</div>
  </body>
</html>
```

#### jQuery的使用步骤

- 引入jQuery的js文件
- 使用选择器定位要操作的节点
- 调用jQuery的方法进行操作

### jQuery对象

#### 什么是jQuery对象

- jQuery为了解决浏览器的兼容问题而提供的一种统一封装后的对象描述
- jQuery提供的方法都是针对jQuery对象特有的，而且大部分方法的返回值类型也是jQuery对象，所以方法可以连缀调用"jQuery对象.方法().方法().方法()..."
- 通过jQuery选择器选中的对象为jQuery对象
  - `$("div")`
  - `$("#d1")`

#### jQuery对象与DOM对象

- jQuery对象本质上是一个DOM对象数组，它在该数组上扩展了一些操作数组中元素的方法
- 可以直接操作这个数组
  - `obj.length`
    - 获取数组长度
  - `obj.get(index)`
    - 获取数组中的某一个DOM对象
  - `obj[index]`
    - 等价于obj.get(index)

#### DOM对象转换为jQuery对象

- `$(DOM对象)`

---

## jQuery选择器

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gijb5z0rotj30z10u00yy.jpg)

### jQuery选择器

#### 什么是jQuery选择器

- jQuery选择器类似于CSS选择(定位元素，施加样式)，能够实现定位元素，施加行为
- 使用jQuery选择器能够将内容与行为分离

#### 选择器的种类

- 基本选择器
- 层次选择器
- 过滤选择器
- 表单选择器

### 基本选择器

- 元素选择器
  - 一句标签名定位元素
  - `$("标签名")`
- 类选择器
  - 根据class属性定位元素
  - `$(".class属性名")`
- id选择器
  - 根据id属性定位元素
  - `$("#id")`
- 选择器组
  - 定位一组选择器所对应的所有元素
  - `$("#id,.importent")`

### 层次选择器

- 在select1元素下，选中所有满足select2的子孙元素
  - `$("select1 select2")`
- 在select1元素下，选中所有满足select2的子元素
  - `$("select1>select2")`
- 选中select1元素的，满足select2的下一个弟弟
  - `$("select1 + select2")`
- 选中select1元素的，满足select2的所有弟弟
  - `$("select1 ~ select2")`

### 过滤选择器

#### 基本过滤选择器

- 根据元素的基本特征定位元素，常用于表格和列表
  - `first`
    - 第一个元素
  - `last`
    - 最后一个元素
  - `not(selector)`
    - 把selector排除在外
  - `even`
    - 挑选偶数行
  - `odd`
    - 挑选奇数行
  - `eq(index)`
    - 下标等于index的元素
  - `gt(index)`
    - 下标大于index的元素
  - `lt(index)`
    - 下标小于index的元素

#### 内容过滤选择器

- 根据文本内容定位元素
  - `contains(text)`
    - 匹配包含给定文本的元素
  - `empty`
    - 匹配所有不包含子元素或文本的空元素

#### 可见性过滤选择器

- 根据可见性定位元素
  - `hidden`
    - 匹配所有不可见元素，或type为hidden的元素
  - `visible`
    - 匹配所有的可见元素

#### 属性过滤选择器

- 根据属性定位元素
  - `[attribute]`
    - 匹配具有attribute属性的元素
  - `[attribute = value]`
    - 匹配属性等于value的元素
  - `[attribute != value]`
    - 匹配属性不等于value的元素

#### 状态过滤选择器

- 根据状态定位元素
  - `enabled`
    - 匹配可用的元素
  - `disabled`
    - 匹配不可用的元素
  - `checked`
    - 匹配选中的checkbox
  - `selected`
    - 匹配选中的option

### 表单选择器

- 表单选择器包括
  - `text`
    - 匹配文本框
  - `password`
    - 匹配密码框
  - `radio`
    - 匹配单选框
  - `checkbox`
    - 匹配多选框
  - `submit`
    - 匹配提交按钮
  - `reset`
    - 匹配重置按钮
  - `button`
    - 匹配普通按钮
  - `file`
    - 匹配文件框
  - `hidden`
    - 匹配隐藏框

---

## jQuery操作DOM

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gijbmlx9l7j31600qwadk.jpg)

### 读写节点

- 读写节点的HTML内容
  - `obj.html()/obj.html("<span>123</span>")`
- 读写节点的文本内容
  - `obj.text()/obj.text("123")`
- 读写节点的value属性值
  - `obj.val()/obj.val("abc")`
- 读写节点的属性值
  - `obj.attr("属性名")/obj.val("属性名","属性值")`

### 增删节点

#### 创建DOM节点

- 语法
  - `$("节点内容")`
  - `$("<span>你好</span>")`

#### 插入DOM节点

- `parent.append(obj)`
  - 作为最后一个子节点添加进来
- `parent.prepend(obj)`
  - 作为第一个子节点添加进来
- `brother.after(obj)`
  - 作为下一个兄弟节点添加进来
- `brother.before(obj)`
  - 作为上一个兄弟节点添加进来

#### 删除DOM节点

- `obj.remove()`
  - 删除节点
- `obj.remove(selector)`
  - 只删除满足selector的节点
- `obj.empty()`
  - 清空节点

### 样式操作

- `addClass("")`
  - 追加样式
- `removeClass("")`
  - 移除指定样式
- `removeClass()`
  - 移除所有样式
- `toggleClass("")`
  - 切换样式
- `hasClass("")`
  - 判断是否有某个样式
- `css("")`
  - 读取css的值
- `css("","")`
  - 设置多个样式

### 遍历节点

- `children()/children(selector)`
  - 直接子节点
- `next()/next(selector)`
  - 下一个兄弟节点
- `prev()/prev(selector)`
  - 上一个兄弟节点
- `siblings()/siblings(selector)`
  - 所有兄弟
- `find(selector)`
  - 查找满足选择器的所有后代
- `parent()`
  - 父节点

---

## jQuery事件

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gijcbuyxg5j315s0saq74.jpg)

### 事件处理

#### 使用jQuery实现事件绑定

- 语法
  - `$obj.bind(事件类型,事件处理函数)`
  - 如`$obj.bind('click',fn)`
  - 简写形式`$obj.click(fn)`
- `$obj.click()`代表出发了click事件

#### 获得事件对象event

- 只需要为事件处理函数传递任意一个参数
  - 如`$obj.click(function(e){...})`
- e就是事件对象，但已经超过jQuery对底层事件对象的封装
- 封装后的事件对象可以方便的兼容各浏览器

#### 事件对象的常用属性

- 获取事件源e.target，返回值是DOM对象
- 获取鼠标点击的坐标
  - e.pageX
  - e.pageY

### 事件冒泡

#### 什么是事件冒泡

- 子节点产生的事件会依次向上抛给父节点

#### 如何取消事件冒泡

- `e.stopPropagation()`可以取消事件冒泡

  ```js
  $('a').click(function(e){
  	alert('点击了一个链接');
  	e.stopPropagation();
  });
  ```

### 合成事件

#### jQuery的合成事件种类

- hover(mouseenter,mouseleave)模拟光标悬停事件
- toggle()在多个事件响应中切换

### 模拟操作

#### 模拟操作的语法

- `$obj.trigger(事件类型)`
  - 如`$obj.trigger("focus")`
  - 简写形式`$obj.focus()`

---

## jQuery动画

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gijcy49i1gj30z80n8mzp.jpg)

### 显示、隐藏的动画效果

- `show()/hide()`
- 作用
  - 通过同时改变元素的宽度和高度来实现显示或者隐藏
- 用法
  - `$obj.show(执行时间,回调函数)`
    - 执行时间
      - slow, normal,fast或毫秒数
    - 回调函数
      - 动画执行完毕之后要执行的函数

### 上下滑动式的动画实现

- `slideDown()/slideUp()`
- 作用
  - 通过改变高度来实现显示或者隐藏的效果
- 用法同`show()/hide()`

### 淡入淡出式动画效果

- `fadeIn()/fadeOut()`
- 作用
  - 通过改变不透明度opacity来实现显示或者隐藏
- 用法同`show()/hide()`

### 自定义动画效果

- `animate(偏移位置,执行时间,回调函数)`

  - 偏移位置
    - {}描述动画执行之后元素的样式
  - 执行时间
    - 毫秒数
  - 回调函数
    - 动画执行结束后要执行的函数

  ```js
  $("div").click(function(){
    $(this).animate({'left':'500px'},4000);
    $(this).animate({'top':'300px'},2000);
  });
  ```

  