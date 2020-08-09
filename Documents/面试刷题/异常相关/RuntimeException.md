## 给一个你最常见到的`runtime exception`

| 名称                              | 翻译               | 解释                                                         |
| --------------------------------- | ------------------ | ------------------------------------------------------------ |
| `ArithmeticException`             | 算术异常           | 当出现异常的运算条件时，抛出此异常                           |
| `ArrayStoreException`             | 阵列存储异常       | 试图将错误类型的对象存储到一个对象数组时抛出的异常。         |
| `BufferOverflowException`         | 缓冲区溢出异常     | 相对*放置*操作达到目标缓冲区的限制时引发的未经检查的异常     |
| `BufferUnderflowException`        | 缓冲区溢出异常     | 相对的`get`操作达到源缓冲区的限制时引发的未经检查的异常      |
| `CannotRedoException`             | 不能重做异常       | 在告知`redo()`和不告知`UndoableEdit`时抛出                   |
| `CannotUndoException`             |                    | 在告知`undo()`和不告知`UndoableEdit`时抛出                   |
| `ClassCastException`              |                    | 抛出该异常以指示代码已尝试将对象强制转换为不是实例的子类     |
| `CMMException`                    | `CMM`异常          | 如果本机`CMM`返回错误，则抛出此异常                          |
| `ConcurrentModificationException` | 并发修改异常       | 当不允许对对象进行并发修改的方法被检测到时，可能会引发此异常 |
| `DOMException`                    |                    | `DOM`操作仅在“异常”情况下（即，由于逻辑原因，数据丢失或实现变得不稳定）而无法执行操作时才会引发异常 |
| `EmptyStackException`             | 空栈异常           | 由`Stack`类中的方法抛出以指示堆栈为空                        |
| `IllegalArgumentException`        |                    | 被抛出以指示方法已传递了非法或不适当的参数                   |
| `IllegalMonitorStateException`    |                    | 抛出该异常指示线程试图在对象的监视器上等待，或者通知其他线程在对象监视器上等待而没有拥有指定的监视器 |
| `IllegalPathStateException`       |                    | 如果在路径上执行的操作与正在执行的特定操作相比处于非法状态，例如在没有初始`moveto`的情况下将一个路径段追加到`GeneralPath`上，就会抛出一个异常。 |
| `IllegalStateException`           |                    | 标志着一个方法在非法或不适当的时间被调用。换句话说，`Java`环境或`Java`应用程序不在请求操作的适当状态 |
| `ImagingOpException`              | 成像`Op`异常       | 如果一个`BufferedImageOp`或`RasterOp`滤镜方法不能处理图像，则会抛出`ImagingOpException`。 |
| `IndexOutOfBoundsException`       | 数组下标越界异常   | 表示某种索引（例如数组，字符串或向量）的索引超出范围         |
| `MissingResourceException`        | 缺失资源异常       | 表示资源丢失                                                 |
| `NegativeArraySizeException`      | 负数组尺寸异常     | 如果应用程序尝试创建负大小的数组，则抛出该异常               |
| `NoSuchElementException`          |                    | 被各种访问器方法抛出以指示所请求的元素不存在                 |
| `NullPointerException`            | 空指针异常         | 当应用程序试图在需要对象的情况下使用`null`时抛出。这些情况包括<br/>调用`null`对象的实例方法。<br/>访问或修改一个`null`对象的字段。<br/>把`null`的长度当作一个数组。<br/>像访问数组一样访问或修改空对象的槽。<br/>将空值当作一个可抛出的值来抛出。<br/>应用程序应该抛出该类的实例，以表明对null对象的其他非法使用。`NullPointerException`对象可以由虚拟机构造，就像抑制被禁用和/或堆栈跟踪不可写一样。 |
| `ProfileDataException`            | 配置文件数据异常   | 当访问或处理`ICC_Profile`对象时发生错误时，会抛出这个异常。  |
| `ProviderException`               | 提供者异常         | 提供者异常（如配置错误或不可恢复的内部错误）的运行时异常，它可以被`Providers`子类化，以抛出专门的、特定于提供者的运行时错误。 |
| `RasterFormatException`           | 栅格格式异常       | 如果`Raster`中存在无效的布局信息，则抛出`RasterFormatException`。 |
| `SecurityException`               | 安全异常           | 由安全管理器抛出以指示安全违规。                             |
| `SystemException`                 | 系统异常           | 所有`CORBA`标准异常的根类。这些异常可能作为任何`CORBA`操作调用的结果被抛出，也可能被许多标准`CORBA API`方法返回。标准异常包含一个小代码，允许更详细的规范，以及一个完成状态。这个类被子类化来生成每一个标准`ORB`异常集。`SystemException`扩展了`java.lang.RuntimeException`；因此`SystemException`异常都不需要在IDL接口中操作映射的`Java`方法的签名中声明。 |
| `UndeclaredThrowableException`    | 未声明的可丢弃异常 | 如果其调用处理程序的`invoke`方法调用抛出了一个检查异常（一个不可分配给 `RuntimeException` 或 `Error` 的 `Throwable`），该异常不可分配给在代理实例上调用并派发给调用处理程序的方法的 `throws` 子句中声明的任何异常类型，则由该方法调用抛出。<br/>`UndeclaredThrowableException`实例包含由调用处理程序抛出的未声明的检查异常，它可以通过`getUndeclaredThrowable`()方法来检索。`UndeclaredThrowableException`扩展了`RuntimeException`，所以它是一个包装了检查异常的未检查异常。<br/><br/>从1.4版本开始，这个异常已经被改造成符合通用的异常链机制。可以在构造时提供 "由调用处理程序抛出的未声明的检查异常"，并通过`getUndeclaredThrowable`()方法访问，现在被称为原因，可以通过`Throwable.getCause()`方法以及前述 "遗留方法 "访问。" |
| `UnmodifiableSetException`        |                    | 抛出表示由于该集合不可修改而无法执行请求的操作。             |
| `UnsupportedOperationException`   | 不支持的操作异常   | 抛出该异常以指示不支持请求的操作。                           |

