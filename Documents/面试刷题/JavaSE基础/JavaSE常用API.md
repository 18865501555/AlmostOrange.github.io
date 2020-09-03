## Math.round(11.5)等于多少？Math.round(-11.5)又等于多少？

- Math.round(11.5)的返回值是12
- Math.round(-11.5)的返回值是-11
- 四舍五入的原理是在参数上加0.5然后进行取整

---

## switch是否能作用在byte上，是否能作用在long上，是否能作用在String上

- Java5以前switch(expr)中，expr只能是byte, short,char,int
- 从Java5开始，Java中引入了枚举类型，expr也可以是enum类型
- 从Java7开始，expr还可以是字符串(String)，但是长整型(long)在目前所有的版本中都是不可以的

---

## 数组有没有length()方法？String有没有length()方法？

- 数组没有length()方法，而是有length属性
- String有length()方法
- JavaScript中，获得字符串的长度是通过length属性得到的，这一点容易和Java混淆

---

## String、StringBuilder、StringBuffer的区别

- Java平台提供了两种类型的字符串:String和StringBuffer/StringBuilder，它们都可以存储和操作字符串,区别如下

- String

  - 只读字符串，也就意味着String引用的字符串内容是不能被改变的

    ```java
    String str = "abc";
    str = "bcd";
    ```

    - 如上字符串str仅仅是一个引用对象，它指向一个字符串对象abc

    - 第二行代码的含义是让str重新指向了一个新的字符串bcd对象

    - 而abc对象并没有任何改变

- StringBuffer/StringBuilder

  - 表示的字符串对象可以直接进行修改

- StringBuilder

  - 是Java5中引入的
  - 它和StringBuffer的方法完全相同
  - 区别在于它是在单线程环境下使用的
  - 因为它的所有方法都没有被synchronized修饰
  - 因此它的理论效率上也比StringBuffer要高

---

## 什么情况下用"+"运算符进行字符串连接比调用StringBuffer/StringBuilder对象的append方法连接字符串性能更好

- 字符串是Java程序中最常见的数据结构之一

- 在Java中String类已经重载了"+"，也就是说，字符串可以直接使用"+"进行连接，如

  ```java
  String s = "abc"+"ddd";
  ```

- 在Java中提供了一个StringBuilder类(这个类只在J2SE5及以上版本提供，以前的版本使用StringBuffer类)，这个类也可以起到"+"的作用。

  ```java
  String s = "abc";
  String ss = "ok"+s+"xyz"+5;
  System.out.println(ss);
  ```

  - 从运行结果来解释，"+"和StringBuilder是完全等效的
  - 从运行效率和消耗资源方面看，StringBuilder的效率更高

- StringBuffer和StringBuilder的功能基本一样，只是StringBuffer是线程安全的
- StringBuilder不是线程安全的，因此StringBuilder的效率高

## 请说出下面程序的输出

```java
String s1 = "Programming";
String s2 = new String("Programming");
String s3 = "Program";
String s4 = "ming";
String s5 = "Program"+"ming";
String s6 = s3+s4;
System.out.println(s1 == s2);						//false
System.out.println(s1 == s5);						//true
System.out.println(s1 == s6);						//false
System.out.println(s1 == s6.intern());	//true
System.out.println(s2 == s2.intern());	//false
```

- String对象的intern()方法会得到字符串对象在常量池中对应的版本的引用(如果常量池中有一个字符串与String对象的equals结果是ture)，如果常量池中没有对应的字符串，则该字符串将被添加到常量池中，然后返回常量池中字符串的引用；
- 字符串的+操作其本质是创建了StringBuilder对象进行append操作，然后将拼接后的StringBuilder对象用toString方法处理成String对象，者一点可以用java -c StringEqualTest.class命令获得class文件对应的JVM字节码指令就可以看出来

## 如何取得年月日、时分秒？

```java
Calendar cal = Calendar.getInstance();
System.out.println(cal.get(Calendar.YEAR));
System.out.println(cal.get(Calendar.MONTH));	//0-11
System.out.println(cal.get(Calendar.DATE));
System.out.println(cal.get(Calendar.HOUR_OF_DAY));
System.out.println(cal.get(Calendar.MINUTE));
System.out.println(cal.get(Calendar.SECOND));
//Java 8
LocalDateTime dt = LocalDateTime.now();
System.out.println(dt.getYear());
System.out.println(dt.getMonthValue());	//1-12
System.out.println(dt.getDayOfMonth());
System.out.println(dt.getHour());
System.out.println(dt.getMinute());
System.out.println(dt.getSecond());
```

## 如何取得从1970年1月1日0时0分0秒到现在的毫秒数？

```java
//第一种方式
Calendar.getInstance().getTimeInMillis();
//第二种方式
System.currentTimeMillis();
//Java 8
Clock.systemDefaultZone().millis();
```

## 如何取得某月的最后一天

```java
//Java 8
LocalDate today = LocalDate.now();
//本月的第一天
LocalDate firstDay = LocalDate.of(today.getYear(),today.getMonth(),1);
//本月的最后一天
LocalDate lastDay = today.with(TemporalAdjusters.lastDayOfMonth());
System.out.println("本月的第一天:"+firstDay);
System.out.println("本月的最后一天:"+lastDay);
```

## 如何格式化日期

- Java8中引入了新的时间日期API，其中包括LocalDate,LocalTime,LocalDateTime,Clock,Instant等，这些类的设计都使用了不变模式，因此是线程安全的设计

```java
DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyy/MM/dd");
LocalDate date = LocalDate.now();
System.out.println(date.format(dateTimeFormatter));
```

## Java8日期/时间特性

- Java8日期/时间API是JSR-310的实现，它的实现目标是克服旧的日期时间实现中所有的缺陷，新的日期/时间API的一些设计原则是
- 不变性
  - 所有的类都是不可变的，这对多线程环境有好处
- 关注点分离
  - 将人可读的日期时间和机器时间明确分离
  - 为日期(Date)、时间(Time)、日期时间(DateTime)、时间戳(unix timestamp)以及时区定义了不同的类
- 清晰
  - 在所有的类中，方法都被明确定义用以完成相同的行为
  - 为了更好的处理问题，所有的类都使用了工厂模式和策略模式
- 实用操作
  - 都实现了一系列方法用以完成通用的任务
    - 加、减、格式化、解析、从日期/时间中提取单独部分
- 可扩展性
  - 工作在ISO-8601日历系统上，也可以应用在非ISO的日历上