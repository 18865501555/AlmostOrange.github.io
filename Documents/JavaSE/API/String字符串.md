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