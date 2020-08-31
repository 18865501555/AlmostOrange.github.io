## 基本数据类型的包装类

### 为什么需要包装类`(Wrapper Class)`

- `Java`并不是纯面向对象的语言
- `Java`语言是一个面向对象的语言，但是`Java`中的基本数据类型却不是面向对象的
- 我们在实际使用中经常需要将基本数据转化为对象，便于操作
  - 比如集合的操作中
  - 我们就需要将基本类型数据转化成对象

### 包装类和基本数据类型的对应关系

- 包装类均位于`java.lang`包

| 基本数据类型 | 包装类      |
| ------------ | ----------- |
| `byte`       | `Byte`      |
| `boolean`    | `Boolean`   |
| `short`      | `Short`     |
| `char`       | `Character` |
| `int`        | `Integer`   |
| `long`       | `Long`      |
| `float`      | `Float`     |
| `double`     | `Double`    |

### 如何使用包装类？

- 包装类的作用
  - 提供:**字符串**、**基本数据类型**、**对象**之间互相转化的方式
  - 包含每种基本数据类型的相关属性
    - 最大值
    - 最小值等
- 所有的包装类`(Wrapper Class)`都有类似的方法

### 自动装箱和自动拆箱

- 自动装箱`boxing`
  - 基本类型就自动的封装到与它相同类型的包装中
    - `Integer i = 100;`
    - 本质上是，编译器编译时为我们添加了
    - `Integer i = Integer.valueOf(100);`
- 自动拆箱`autounboxing`
  - 包装类对象自动转换成基本类型数据
    - int a = new Integer(100);
    - 本质上是，编译器编译时为我们添加了
    - `int a = new Integer(100).intValue();`

---

## 字符串相关类

### `String`(不可变字符序列)

- `Java`字符串就是`Unicode`字符序列
  - 例如`"Java"`就是**4**个`Unicode`字符`J`,`a`,`v`,`a`组成的
- `Java`允许使用符号`"+"`把两个字符串连接起来
  - `String s1 = "Hello";String s2 = "World!";`
  - `String s = s1+s2;//HelloWorld!`

### `String`类的常用方法

- `char charAt(int index)`
  - 返回字符串中第`index`个字符
- `boolean equals(String other)`
  - 如果字符串与`other`相等，返回`true`
- `boolean equalsgnoreCase(String other)`
  - 如果字符串与`other`相等(忽略大小写),则返回`true`
- `int indexOf(String str)` `lastIndesOf()`
- `int length()`
  - 返回字符串的长度
- `String replace(char oldChar,char newChar)`
  - 返回一个新字符串，它是通过用`newChar`替换此字符串中出现的所有`oldChar`而生成的
- `boolean startsWith(String prefix)`
  - 如果字符串以`prefix`开始，则返回`true`
- `boolean endsWith(String prefix)`
  - 如果字符串以`prefix`结尾，则返回`true`
- `String substring(int beginIndex)`
- `String substring(int beginIndex,int endIndex)`
  - 返回一个新字符串，该字符串包含从原始字符串`beginIndex`到结尾或`endIndex-1`的所有字符
- `String toLowerCase()`
  - 返回一个新字符串，该字符串将原始字符串中的所有大写字母改成小写字母
- `String toUpperCase()`
  - 返回一个新字符串，该字符串将原始字符串中的所有小写字母改成大写字母
- `String trim()`
  - 返回一个新字符串，该字符串删除了原始字符串头部和尾部的空格

### 字符串相等的判断(一般使用`equals`方法)

- `equals`判断字符串值相等
- `==`判断字符串对象引用相等

```java
public class StringTest{
  public static void main(String[] args){
    String s1 = "abc";
    String s2 = "abc";
    String s3 = new String("abc");
    String s4 = new String("abc");
    System.out.println(s1==s2);//true
    System.out.println(s1==s3);//false
    System.out.println(s3==s4);//false
  }
}
```

### `StringBuffer`和`StringBuilder`

- `StringBuffer`和`StringBuilder`非常类似，均代表可变的字符序列，而且方法也一样
- 当对字符串进行修改的时候，需要使用 `StringBuffer` 和 `StringBuilder` 类
- 和 `String` 类不同的是，`StringBuffer` 和 `StringBuilder` 类的对象能够被多次的修改，并且不产生新的未使用对象
- `StringBuilder` 类在 `Java 5` 中被提出，它和 `StringBuffer` 之间的最大不同在于 `StringBuilder` 的方法**不是线程安全**的（不能同步访问）
- 由于 `StringBuilder` 相较于 `StringBuffer` 有速度优势，所以多数情况下建议使用 `StringBuilder` 类
- 在应用程序要求**线程安全**的情况下，则**必须**使用 `StringBuffer` 类

### 字符串选用

- `String`

  - 不可变字符序列

- `StringBuilder`

  - 可变字符序列、效率高、线程不安全

- `StringBuffer`

  - 可变字符序列、效率低、线程安全

- `String`使用陷阱

  ```java
  String s = "a";//创建了一个字符串
  s=s+"b";
  ```

  实际上原来的`"a"`字符串对象已经丢弃了，现在有产生了一个字符串`s+"b"`。

  如果多次执行这些改变字符串内容的操作，会导致大量副本字符串对象留存在内存中，降低效率。

  如果这样的操作放到循环中，会极大影响程序的性能

## 时间处理相关类

- 在标准`Java`类库中包含一个`Date`类
  - 它的对象表示一个特定的瞬间，精确到毫秒
- `Java`中时间的表白说白了也是数字
  - 是从标准纪元`1970.1.1` `0`点开始到某个时刻的毫秒数
  - 类型是`long`

### `DateFormat`和`SimpleDateFormat`

- 完成字符串和时间对象的转化
- `format`
- `parse`

### `Calendar`日历类

- 人们对于时间的认识是
  - 某年某月某日，这样的日期概念
- 计算机是`long`类型的数字
- 通过`Calendar`在二者之间搭起桥梁

### `GregorianCalendar`公历

- `GregorianCalendar`是`Calendar`的一个具体子类
- 提供了世界上大多数国家/地区使用的标准日历系统
- 注意
  - 月份:一月是`0`，二月是`1`，以此类推，`12`月是`11`
  - 星期:周日是`1`，周一是`2`，以此类推，周六是`7`

## 枚举类

- 只能取特定值中的一个
- 使用`enum`关键字
- 所有的枚举类型隐性的继承自`java.lang.Enum`
- 枚举实质上还是类，而每个枚举的成员实质就是一个枚举类型的实例，默认都是`public static final`的，可以直接通过枚举类型名直接使用它们
- 强烈建议当你需要定义一组常量时，使用枚举类型
- 尽量不要使用枚举的高级特性，事实上高级特性都可以使用普通类来实现，没必要引入复杂性

## `Math`类和`Random`类

- 包含了常见的数学运算函数
- `random()`
  - 生成`[0,1)`之间的随机浮点数
- 生成`0-10`之间的任意整数
  - `int a = (int)(10*Math.random());`
- 生成`20-30`之间的任意整数
  - `int b = 20 + (int)(10*Math.random());`

## `File`类

- 文件和目录路径名的抽象表示形式
- 一个`File`对象可以代表一个文件或目录
- 可以实现获取文件和目录属性等功能
- 可以实现对文件和目录的创建、删除等功能
- `File`不能访问文件内容

```java
File file = new File("d:\\test\\java.text");
File file = new File("d:/test/java.text");
File file = new File("java.text");
```

路径可以时绝对路径和相对路径，分隔符采用`\\`或者`/`

### 通过`File`对象可以访问文件的属性

```java
public String getName();
public String getPath();
public boolean isFile();
public boolean isDirectory();
public boolean canRead();
public boolean canWrite();
public boolean exists();
public long length();
public boolean isHidden();
public long lastModified();
public File[] listFiles();
```

### 通过`File`对象创建空文件或目录(在该对象所指的文件或目录不存在的情况下)

```java
public boolean createNewFile()throws IOException;
public boolean delete();
public boolean mkdir();
public boolean mkdirs();
```

