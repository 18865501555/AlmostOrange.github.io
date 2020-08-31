### StringBuilder定义

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