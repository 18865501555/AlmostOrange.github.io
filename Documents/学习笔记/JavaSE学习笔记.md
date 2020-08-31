## API

- 应用编程接口
  - 现成的程序组件
- Java核心API
  - Java中最常见的程序组件
    - 字符串
    - 包装类
    - IO
    - 网络
    - 线程
    - ……

## String字符串

- Java中用于表示输入输出文字的API

  - 如Abc就是一个字符串
  - 任何ABC都是字符串
  - 字符串类型提供了很多字符串操作API，这些API可以方便程序编写，提高开发效率
- 字符串都是对象
  - 如果定义了与JavaAPI同名的类，则在后续编码中，必须不能省略（加上）Java的包名，如果：java.lang.String使用非常不方便！
    - 建议：不要将类名定义成与Java API类名同名！
  

### 字符串特点

- 字符串对象内部封装的是一个固定不变的字符数组，称为字符串对象不可以改变，简称"不变对象"
  - 可以根据序号获取字符串中的字符
  - 可以利用length()检查字符的个数
- 任何字符串API方法或者字符串连接操作都不会改变字符串对象
- 字符串字面量是字符串对象
- Java为了性能，提供字符串常量池

### 字符串对象不可改变

- "字符串对象"不可改变
- 字符串引用变量可以改变
- Java中字符串发生改变时候，都会创建新的字符串对象

### 字符串常量池

- Java为了优化字符串性能，而设计字符串常量池
- 利用常量池"复用"字符串字面量、常量、字面量运算结果
- 字符串在操作期间，会大量产生新字符串，如果全部复用，势必会造成内存不够
- 如果不进行字符串对象复用，又会因为反复创建字符串造成大量性能开销
- java设计了一个折中策略
  - 静态字符串进行重复使用，第一次使用时创建
  - 动态生成的字符串，一次使用，使用后，不被引用时销毁
- 静态字符串创建后存储到常量池，如果出现复用的时候，重复使用同一个字符串

### 将字符串转换为大写字符串

- toUpperCase() 将字符串每个字符，转换为对应的大写
- toUpperCase() JAVA提供的API方法。
- 可以在字符串“字面量”上直接调用API方法

```java
String s = "Hello World";
String s1 = s.toUpperCase();
System.out.println(s1);//HELLO WORLD
String str = "Hello World!".toUpperCase();
```

### 将字符串转换为小写字符串

- toLowerCase() 将字符串每个字符，转换为对应的小写
- toLowerCase() JAVA提供的API方法。
- 可以在字符串“字面量”上直接调用API方法

```java
String s = "Hello World";
String s1 = s.toLowerCase();
System.out.println(s1);//hello world
String str = "Hello World!".toLowerCase();
```

### 字符串中的字符

- 字符串封装了字符数组，也就是字符串由一个一个字符组成
- 与字符数组类似，字符串中每个字符也具有唯一的下标序号
- charAt()方法根据序号获得字符串中的一个字符
- length()方法获得当前字符串中字符的个数

### 检查是否为空字符串

- 没有内容的字符串，称为空字符串
- isEmpty()用于检查字符串是否为空字符串，如果是空的就返回true

### 判断两个字符串相等

- ==用于比较字符串时，用于检查是否是同一个字符串对象
  - 如果是同一个对象，则true
  - 不引用同一个对象，则false
- ==不能用来比较字符串内容是否一样
- Java提供了API方法equals(相等)，用来比较两个字符串是否相等
- equals底层就是将两个字符串的每个字符逐一进行对比，判断是否相等

### 查找字符串中字符(字符串)的位置

- indexOf检索一个字符在字符串中出现的位置(索引位置)

### 从后向前查找字符(字符串)的位置

- lastIndexOf

- 例如从URL截取文件名

  - `http://tomcat/demo.css`文件名`demo.css`

  - 如何得到文件名的位置，从后向前查找"/"的位置，找到以后“/”加1就是文件名的起始位置

  - 使用substring(index), index是文件名第一个字符的起始位置

    ```java
    String url = "http://tomcat/demo.css";
    int index = url.lastIndexOf("/");
    String file = url.substring(index+1);
    System.out.println(file); 
    ```

### 去除字符串两端的空白

- str.trim() 去除字符串前后的空白
- 空白包括，Unicode编号小于等于\U0020的字符，如 空格，回车、换行等
- trim() 经常用于去除用户意外输入的空白字符 

### 子字符串

- 获取子字符串
  - 截取一个字符串中的一部分
- 从原始字符串str中截取一部分，返回时候包含开始位置，不包含结束位置
  - beginIndex开始位置
  - endIndex结束位置
- substring方法可以按照长度截取字符串
  - substring(开始位置,开始位置+截取长度);
- substring提供了重载的方法
  - 从指定位置开始，截取到字符串末尾

## StringBuilder

- String:字符串
- Builder:构建器
- Java提供的动态字符串API，用于动态处理拼接字符，作为String的操作工具，用于生产String对象
- 因为String对象不可以改变，String在操作时候，会利用复制(数组复制)来创建新字符串对象，复制数组是非常耗时的操作。String的动态处理性能不好
  - toLowerCase() trim()连接，都会利用数组复制产生新的对象
- java提供了StringBuilder,是可变字符串对象，其内部的字符数组可以改变，StringBuilder在更改时候，可以减少char数组复制
- StringBuilder相对于String具有更好性能
- 操作字符串时候，尽量使用StringBuilder，处理结束以后，生成String

### 使用StringBuilder API

- StringBuilder提供的方法，内部封装了可以改变的char数组
- StringBuilder的API可以更新其内部char数组内容
- StringBuilder在其内部char数组容量不足时会自动扩容char数组
  - 新容量一般为1倍+2

### StringBuilder动态操作字符串

- append(内容)
  - 追加字符内容
- insert(位置,内容)
  - 在指定位置插入内容，插入时候原有内容会向后移动
- delete(开始位置,结束位置)
  - 将指定范围内的字符删除，包含开始位置，不包含结束位置
- append insert delete方法返回值就是当前对象本身
- 所以适合连续方法调用，其好处就是可以减少编码量，提高编程效率
- 连续使用`.`调用方法的编码风格称为:函数式编程
- buf.capacity()
  - 检查数组容量大小

### String与StringBuilder使用建议

- 一般情况下，输出文字信息都使用String
- 需要大量修改字符串，就使用StringBuilder，性能好
- StringBuilder处理结束以后，转换为String
- String的连接底层就是利用StringBuilder实现的

## 正则表达式

- 简称正则

### 什么是正则表达式

- 正则本身是包含正则语法字符串
- 正则用于约定目标字符串的规则
- 几乎所有的编程语言都支持正则，语法基本一样

### 正则表达式核心用途

- 匹配目标字符串的规则

### 常见字符串规则

- 密码规则
  - 包含大写、小写、数字和特殊符号,至少3个
  - 用户名规则
  - 电话号码规则
  - Java代码规则
  - ……
- 我们软件中也是需要检验一个字符串是否符合特定的规则
- 正则表达式就是用于检验字符串规则的语法

### 字符集

- 约定一个字符的可选择范围

  ```java
  Hello[abc] : 规则是；连续6个字符，前五个必须是Hello，最后一个是 a b c 3个之一
  Hello[a-z] : 连续6个字符，前五个必须是Hello，最后一个是 a到z 之一
  Hello[a-zA-Z] : 连续6个字符，前五个必须是Hello，最后一个是 a到z 或 A到Z 之一 
  ```

### 缩写字符集

