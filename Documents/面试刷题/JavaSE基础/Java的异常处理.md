## Java中异常分为哪些种类

- 按照异常需要处理的时机分为编译时异常(也叫强制性异常)也叫CheckedException和运行时异常(也叫非强制性异常)也叫RuntimeException
- 只有Java语言提供了Checked异常，Java认为Checked异常都是可以被处理的异常，所以Java程序必须显式处理Checked异常
- 如果程序没有处理Checked异常，该程序在编译时就会发生错误无法编译
- 这体现了Java的设计哲学
  - 没有完善错误处理的代码根本没有机会被指向
- 对Checked异常(编译时异常)垂立方法有两种
  - 当前方法知道如何处理该异常，则用try...catch块来处理该异常
  - 当前方法不知道如何处理，则在定义该方法时声明抛出该异常
- 运行时异常只有当代码在运行时才发生的异常，编译时不需要try...catch
- Runtim如除数是0和数组下标越界等，其产生频繁，处理麻烦，若显示声明或者捕获将会对程序的可读性和运行效率影响很大
- 所以由系统自动检测并将它们交给缺省的异常处理程序
- 当然如果你有处理要求也可以显示捕获它们

---

## 调用下面的方法，得到的返回值是什么

```java
public int getNum(){
  try{
    int a = 1/0;
    return 1;
  }catch(Exception e){
    return 2;
  }finally{
    return 3;
  }
}
```

- 代码在走到第3行的时候遇到了一个MathException，这时第四行的代码就不会执行了，代码直接跳转到catch语句中，走到第6行的时候，异常机制有这么一个原则，如果在catch中遇到了return或者异常等能使该函数终止的话，那么有finally就必须先执行完finally代码块里的代码然后再返回值
- 因此代码跳到第8行，第8行是一个return语句，那么这个时候方法就结束了，因此第6行的返回结果就无法被真正返回
- 如果finally仅仅是处理了一个释放资源的操作，那么该题最终返回结果就是2
- 因此，此题返回值是3

---

## Error和Exception的区别

- Error类和Exception类的父类都是Throwable类

### Error类

- 一般是指与虚拟机相关的问题
- 如系统崩溃，虚拟机错误，内存空间不足，方法调用栈溢出等
- 对于这类错误的导致的应用程序中断，仅靠程序本身无法恢复和预防，遇到这样的错误，建议让程序终止

### Exception类

- 表示程序可以处理的异常，可以捕获且可能恢复
- 遇到这类异常，应该尽可能处理异常，使程序恢复运行，而不应该随意终止异常
- Exception类又分为运行时异常(Runtime Exception)和受检查的异常(Checked Exception)

#### 运行时异常(Runtime Exception)

- ArithmaticException,IllegalArgumentException，编译能通过，但是一运行就终止了
- 程序不会处理运行时异常，出现这类异常，程序会终止

#### 受检查的异常(Checked Exception)

- try...catch捕获
- throws子句声明抛出，交给它的父类处理
- 否则编译不通过

---

## Java异常处理机制

- Java对异常进行了分类，不同类型的异常分别用不同的Java类表示，所有的根类为java.lang.Throwable
- Throwable下面又派生了两个子类
  - Error
    - 应用程序本身无法克服和恢复的一种严重问题
  - Exception
    - 程序还能够克服和恢复的问题，其中又分为
      - 系统异常
        - 软件本身缺陷所导致的问题,也就是软件开发人员考虑不周所导致的问题
        - 软件使用者无法克服和恢复这种问题，但在这种问题下还可以让软件系统继续运行或者让软件死掉
        - 例如
          - 数组下标越界(ArrayIndexOutOfBoundsException)
          - 空指针异常(NullPointerException)
          - 类转换异常(ClassCastException)
          - ...
      - 普通异常
        - 运行环境的变化或异常所导致的问题
        - 是用户能够客户的问题
        - 例如
          - 网络断线
          - 硬盘空间不够
        - 发生这样的异常后，程序不应该死掉
- Java为系统异常和普通异常提供了不同的解决方法，编译器强制普通异常必须try...catch处理或用throws声明继续抛给上层调用方法处理，所以普通异常也称为checked异常，而系统异常可以处理也可以不处理，所以，编译器不强制用try...catch处理或用throws声明，所以系统异常也称为unchecked异常

---

## 请写出你最常见的5个RuntimeException

- java.lang.NullPointerException空指针异常
  - 调用了未经初始化的对象或者是不存在的对象
- java.lang.ClassNotFoundException找不到指定类
  - 类的名称和路径加载错误
  - 通常是程序试图通过字符串来加载某个类时可能引发异常
- java.lang.NumberFormatException字符串转换为数字异常
  - 字符型数据中包含非数字型字符
- java.lang.IndexOutOfBoundsException数组下标越界异常
  - 操作数组对象时发生
- java.lang.IllegalArgumentException方法传递参数错误
- java.lang.ClassCastException数据类型转换异常
- java.lang.NoClassDefFoundException未找到类定义错误
- SQLException
  - 操作数据库时SQL语句错误
- java.lang.InstantiationExceptiong实例化异常
- java.lang.NoSuchMethodException方法不存在异常

---

## throw和throws的区别

- throw
  - throw语句用在方法体内，表示抛出异常，由方法体内的语句处理
  - throw是具体向外抛出异常的动作，所以它抛出的是一个异常实例，执行throw一定是抛出了某种异常
- throws
  - throws语句是用在方法声明后面，表示如果抛出异常，由该方法的调用者来进行异常的处理
  - throws主要是声明这个方法会抛出某种类型的异常，让它的使用者要知道需要捕获的异常的类型
  - throws表示出现异常的一种可能性，并不一定会发生这种异常

---

## final、finally、finalize的区别

- final
  - 用于声明属性，方法和类
  - 分别表示属性不可变，方法不可覆盖，被其修饰的类不可继承
- finally
  - 异常处理语句结构的一部分，表示总是执行
- finalize
  - Object类的一个方法，在垃圾回收器执行的时候会调用被回收对象的方法，可以覆盖此方法提供垃圾收集时的其他资源回收，例如关闭文件等
  - 该方法更像是一个对象生命周期的临终方法，当该方法被系统调用则代表该对象即将死亡
  - 需要注意的是，我们主动行为上去调用该方法并不会导致该对象死亡
  - 这是一个被动的方法(其实就是回调方法)，不需要我们调用