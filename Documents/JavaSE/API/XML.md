## XML用途

- XML指可扩展标记语言(Extensible Markup Language)
  - 时独立与软件和硬件的信息传输工具
- XML应用与web开发的许多方面，常用语简化数据的存储和共享
- XML简化数据共享
- XML简化数据传输
- XML简化平台的变更

## XML基本语法

### XML处理指令

- XML处理指令，简称PI(processing instruction)

- 处理指令用来指挥解析引擎如何解析XML文档内容

  ```xml
  <?xml version="1.0" encoding="utf-8" ?>
  ```

  - 在XML中，所有的处理指令都以`<?`开始`?>`结束
  - `<?`后紧跟着的是处理指令的名称
  - XML处理指令要求指定一个version属性
  - 允许指定可选的standalone和encoding，其中standalone是指是否允许使用外部声明，可设置为yes或no
    - yes:指定不使用外部声明
    - no:指定使用外部声明
  - encoding是指作者使用的字符编码格式
    - UTF-8
    - GBK
    - gb2312等

### 元素和属性

- XML文档包含XML元素
- XML元素指的是从(且包括)开始标签直到(且包括)结束标签的部分
- 元素可包含其他元素、文本或者两者的混合物
- 元素也可以拥有属性
- XML元素可以在开始标签中包含属性，属性(Attribute)提供关于元素的额外信息
- 属性通常提供不属于数据组成部分的信息，但是对需要处理这个元素的应用程序来说却很重要
- XML属性必须加引号，属性值必须被引号包围，单双引号均可使用
- 如果属性值本身包含双引号，那么有必要使用但引号包围它，或者可以使用实体引用

### 大小写敏感

- 在XML中，标记`<Letter>`和标记`<letter>`是不一样的
- 因此，起始和结束标记的大小写应该写成相同的
  - `<letter>...</letter>`
  - `<Letter>...</Letter>`

### 元素必须有关闭标签

- XML要求每个元素必须由其事标签和关闭标签组成
- 关闭标签与起始标签的名字相同，写法上多一个`"/"`
  - `<Letter>`只有起始标记是不行的
  - `<Letter>` `</Letter>`必须要有关闭标签
  - `<Letter/>`自关闭标签等同于无内容空元素

### 必须有根元素

- XML要求必须有根元素，所谓根元素就是不被其他元素包围

- 根元素只能有一个

  ```xml
  <datasource id = "db_oracle"> # 根元素
  	<property name = "url"></property>
    <property name = "..."></property>
    <property name = "..."></property>
  </datasource>
  ```

### 元素必须正确嵌套

- XML要求所有元素必须正确的嵌套

### 实体引用

- 实体可以是常用的短语，键盘字符，文件，数据库记录或任何包含数据的项

| 实体引用 | 字符 | 说明   |
| -------- | ---- | ------ |
| `&lt;`   | <    | 小于   |
| `&gt;`   | >    | 大于   |
| `&amp;`  | &    | 与字符 |
| `&apos;` | ''   | 单引号 |
| `&quot;` | ""   | 双引号 |

### CDATA段

- 格式
  - `<![CDATA[文本内容]]>`
- 特殊标签中的实体引用都被忽略，所有引用内容被当成一整块文本数据对待

```xml
<? xml version="1.0" encoding="utf-8">
<root>
	<![CDATA[
		<hello>-----------无论写什么，都会被当作一个文本
		<world>-----------
	]]>
 <subRoot></subRoot>
</root>
```

---

## XML解析

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gihxltij5qj31050u0797.jpg)

### XML解析方式

#### SAX解析方式

- SAX(simple API for XML)是一种XML解析的替代方法
- 相比于DOM，SAX是一种速度更快，更有效的方法
- 逐行扫描文档，一边扫描一边解析
- 相比于DOM，SAX可以在解析稳定的任意时刻停止解析
- 优点
  - 解析可以立即开始
  - 速度快
  - 没有内存压力
- 缺点
  - 不能对节点做修改

#### DOM解析方式

- DOM(Document Object Model，即文档对象模型)是W3C组织推荐的处理XML的一种方式
- DOM解析器在解析XML文档时，会把文档中的所有元素，按照其出现的层次关系，解析成一个个Node对象(节点)
- 优点
  - 把xml文件在内存中构造树型结构，可以遍历和修改节点
- 缺点
  - 如果文件比较大，内存有压力，解析的时间会比较长

### 读取XML

#### SAXReader读取XML文档

- 使用SAXReader需要导入dom4j-full.jar包

- dom4j是一个Java的XML API，类似于jdom，用来读写XML文件的

- dom4j是一个非常优秀的Java XML API，具有性能优异，功能强大和易用的特点，同时他也是一个开源软件

- 创建SAXReader来读取XML文档

  ```java
  public static DocumentReadXML(String filename) throws DocumentException{
    try{
      //创建SAXReader
      SAXReader reader = new SAXReader();
      //读取指定文件
      Document doc = readre.read(new File(filename));
      return doc;
    }catch(DocumentException e){
      e.printStackTrace();
      throw e;
    }
  }
  ```

#### Document的getRootElement方法

- Document对象是一棵文档树的根，可为我们提供对文档数据的最初(或最顶层)的访问入口

- Element对象表示XML文档中的元素

- 元素可包含属性、其他元素或文本

- 如果元素含有文本，则在文本节点中表示该文本

- Element getRootElement()用于获取根元素

  ```java
  try{
    Document doc = readXML("build.xml");
    //获取根元素
    Element root - doc.getRootElement();
  }catch(Exception e){
    e.printStackTrace();
  }
  ```

  

### Element

#### element方法

- Element element(String name)

  - 获取当前元素下的指定名字的子元素

  ```java
  //测试element方法
  public static void testElement(Element element){
    //获取当前元素下名为path的子元素
    Element e = element.element("path");
    System.out.println(e);
  }
  ```

#### elements方法

- List elements()

  - 获取当前元素下的所有子元素

  ```java
  //测试elements方法
  public static void testElements(Element element){
    List<Element> elements = element.elements();
    for(Element e : elements){
      System.out.println(e);
    }
  }
  ```

#### getName方法

- String getName()

  - 获取当前元素的元素名

  ```java
  //测试getName方法
  public static void testGetName(Element element){
    String name = element.getName();
    System.out.println(name);
  }
  ```

#### getText方法

- String getText()

  - 获取当前元素的文本节点(起始标记与结束标记之间的文本)

  ```java
  //测试getText方法
  public static void testGetText(Element element){
    String name = element.getText();
    System.out.println(name);
  }
  ```

#### attribute方法

- Attribute attribute(int index)

  - 获取当前元素的指定属性，index为索引，从0开始

- Attribute attribute(String name)

  - 获取当前元素的指定名字的属性

  ```java
  //测试attribute方法
  public static void testAttribute(Element element){
    //获取当前元素的第一个属性
    Attribute attr = element.attribute(0);
    //获取当前元素的name属性
    Attribute attr = element.attribute("name");
  }
  ```

  

### Attribute

#### getName,getValue

- Attribute对象用于描述一个元素中的某个属性信息

- 根据该对象可以获取属性名和属性值等信息

  - String getName():获取属性的名字
  - String getValue():获取属性的值

  ```java
  //测试attribute方法
  public static void testAttribute(Element element){
    //获取当前元素的第一个属性
    Attribute attr = element.attribute(0);
    System.out.println(attr.getName());
    System.out.println(attr.getValue());
  }
  ```

  