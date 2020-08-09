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



## 枚举类

## Math类和Random类

## File类

