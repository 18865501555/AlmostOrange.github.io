## Java的基本数据类型都有哪些各占几个字节

| 四类   | 八种    | 字节数 | 数据表示范围                       |
| ------ | ------- | ------ | ---------------------------------- |
| 整型   | byte    | 1      | -128~127                           |
|        | short   | 2      | -32768~32767                       |
|        | int     | 4      | -2147483648~2147483647             |
|        | long    | 8      | -2^63~(2^63)-1                     |
| 浮点型 | float   | 4      | -3.403E38~3.403E38                 |
|        | double  | 8      | -1.798E308~1.798E308               |
| 字符型 | char    | 2      | 表示一个字符，如('a','A','0','家') |
| 布尔型 | boolean | 1      | true\false                         |

---

## String是基本数据类型吗

- String是引用类型
- 底层用char数组实现的

---

## short s1 = 1;s1=s1+1;有错吗？short s1 =1;s1+=1;有错吗？

- short s1 = 1;s1=s1+1;错误
  - 由于1是int类型，因此s1+1运算结果也是int型
  - 需要强制转换类型才能赋值给short型
- short s1 =1;s1+=1;正确
  - 因为s1+=1;相当于s1 = (short)(s1+1);
  - 其中有隐含的强制类型转换

---

## int 和 Integer有什么区别

- Java为每一个基本数据类型都引入了对应的包装类型(wrapper class)
- int的包装类就是Integer
- 从Java5开始引入了自动装箱/拆箱机制，使得二者可以互相转换

| 原始类型 | 包装类型  |
| -------- | --------- |
| boolean  | Boolean   |
| char     | Character |
| byte     | Byte      |
| short    | Short     |
| int      | Integer   |
| long     | Long      |
| float    | Float     |
| double   | Double    |

```java
Integer a = new Integer(3);
Integer b = 3;								//将3自动装箱成Integer类型
int c = 3;						
System.out.println(a == b);		//false	两个引用没有引用同一对象
System.out.println(a == c);		//true	a自动拆箱成int类型再和c比较
```

---

## 下面Integer类型的数值比较输出的结果为？

- 如果整型字面量的值在-128到127之间，不会new新的Integer对象，而是直接引用常量池中的Integer对象

```java
Integer f1=100,f2=100,f3=150,f4=150;
System.out.println(f1==f2);	//true
System.out.println(f3==f4);	//false
```

## String类常用方法

| 方法                                          | 说明                                             |
| --------------------------------------------- | ------------------------------------------------ |
| int length()                                  | 返回当前字符串的长度                             |
| int indexOf(int ch)                           | 查找ch字符在该字符串中第一次出现的位置           |
| int indexOf(String str)                       | 查找str子字符串在该字符串中第一次出现的位置      |
| int lastIndexOf(int ch)                       | 查找ch字符在该字符串中最后一次出现的位置         |
| int lastIndexOf(String str)                   | 查找str子字符串在该字符串中最后一次出现的位置    |
| String substring(int beginIndex)              | 获取从beginIndex位置开始到结束的子字符串         |
| String substring(int beginIndex,int endIndex) | 获取从beginIndex位置开始到endIndex位置的子字符串 |
| String trim()                                 | 返回去除了前后空格的字符串                       |
| boolean equals(Object obj)                    | 将该字符串与指定对象比较，返回true或false        |
| String toLowerCase()                          | 将字符串转换为小写                               |
| String toUpperCase()                          | 将字符串转换为大写                               |
| char charAt(int index)                        | 获取字符串中指定位置的字符                       |
| String[] split(String regex, int limit)       | 将字符串分割为子字符串，返回字符串数组           |
| byte[] getBytes()                             | 将该字符串转换为byte数组                         |

## String、StringBuffer、StringBuilder的区别

- 可变不可变
  - String
    - 字符串常量，在修改时不会改变自身
    - 若修改，等于重新生成新的字符串对象
  - StringBuffer
    - 在修改时改变对象自身，每次操作都是对StringBuffer对象本身进行修改，不是生成新的对象
    - 使用场景:
      - 对字符串经常改变情况下
    - 主要方法:
      - append()
      - insert()
- 线程是否安全
  - String
    - 对象定义后不可变，线程安全
  - StringBuffer
    - 是线程安全的(对调用方法加入同步锁)，执行效率慢，适用于多线程下操作字符串缓冲区大量数据
  - StringBuilder
    - 是线程不安全的，适用于单线程下操作字符串缓冲区大量数据
- 共同点
  - StringBuilder与StringBuffer有公共父类AbstractStringBuilder(抽象类)
  - StringBuilder、StringBuffer的方法都会调用AbstractStringBuilder中的公共方法
    - super
    - append(...)
  - StringBuffer会在方法上加synchronized关键字,进行同步
  - 如果程序不是多线程的，那么使用StringBuilder效率高于StringBuffer

---

## 字符串如何转基本数据类型

- 调用基本数据类型对应的包装类中的方法parseXXX(String)或valueOf(String)即可返回响应基本类型

## 基本数据类型如何转字符串

- 一种方法是将基本数据类型与空字符串("")连接(+)即可获得其所对应的字符串
- 另一种方法是调用String类中的valueOf()方法返回相应字符串