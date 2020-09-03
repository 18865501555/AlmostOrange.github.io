## Java中有几种类型的流

- 按照流的方向

  - inputStream输入流
  - outputStream输出流

- 按照实现功能划分

  - 节点流
    - 可以从或向一个特定的地方(节点)读写数据
    - 如FileReader
  - 处理流
    - 是对一个已存在的流的连接和封装，通过所封装的流的功能调用实现数据读写
    - 如BufferedReader
    - 处理流的构造方法总是要带一个其他的流对象做参数
    - 一个流对象经过其他流的多次帮助，成为流的链接

- 按照处理数据的单位

  - 字节流

    - 继承于InputStream和OutputStream

      ![](https://tva1.sinaimg.cn/large/007S8ZIlly1gidb3h8pdoj31e40ridjq.jpg)

  - 字符流

    - 继承于InputStreamReader和OutputStreamWriter

      ![](https://tva1.sinaimg.cn/large/007S8ZIlly1gidaygc9orj31lw0u00ww.jpg)

---

## 字节流如何转为字符流

- 字节输入流转字符输入流
  - 通过InputStreamReader实现
  - 该类的构造函数可以传入InputStream对象
- 字节输出流转字符输出流
  - 通过OutputStreamWriter实现
  - 该类的构造函数可以传入OutputStream对象

---

## 如何将一个Java对象序列化到文件里

- 在Java中能够被序列化的类必须先实现Serializable接口
- 该接口没有任何抽象方法只是起到一个标记作用

```java
//对象输出流
ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(new File("路径")));
objectOutputStream.writeObject(new User("yasuo",100));
objectOutputStream.close();
```

```java
//对象输入流
ObjectInputStream objectInputStream = new ObjectInputStream(new FileInputStream(new File("路径")));
User user = (User)objectInputStream.readObject();
System.out.println(user);
objectInputStream.close();
```

---

## 字节流和字符流的区别

- 字节流
  - 读取的时候，读到一个字节就返回一个字节
  - 可以处理所有类型数据
    - 图片、MP3、AVI视频文件等
  - 主要是操作byte类型数据
    - 以byte数组为准，主要操作类就是OutputStream、InputStream
  - 处理的单元为1个字节，操作字节和字节数组
- 字符流
  - 使用了字节流读到一个或多个字节时，先去查指定的编码表，将查到的字符返回
    - 中文对应的字节数是两个，在UTF-8码表中是3个字节
  - 只能处理字符数据
  - 处理的单元为2个字节的Unicode字符，分别操作字符、字符数组或字符串
  - 是由Java虚拟机将字节转化为2个字节的Unicode字符为单位的字符而成的，所以对多国语言支持性比较好
- 只要是处理纯文本数据，优先考虑使用字符流，除此之外都用字节流

---

## 如何实现对象克隆

- 实现Cloneable接口并重写Object类中的clone()方法
- 实现Serializable接口，通过对象的序列化和饭序列化实现克隆，可以实现真正的深度克隆
- 基于序列化和反序列化实现的克隆不仅仅是深度克隆，更重要的是通过泛型限定，可以检查出要克隆的对象是否支持序列化，这项检查时编译器完成的，不是在运行时抛出异常，这种事方案明显优于使用Object类的clone方法克隆对象

---

## 什么是Java序列化

- 序列化就是一种用来处理对象流的机制，所谓对象流也就是将对象的内容进行流化
- 可以对流化后的对象进行读写操作，也可以将流化后的对象传输与网络之间
- 序列化是为了解决在对对象流进行读写操作时所引发的问题

---

## 如何实现Java序列化

- 将需要被序列化的类实现Serializable接口，该接口没有需要实现的方法，implements Serializable只是为了标注该对象时可被序列化的
- 然后使用一个输出流(如FileOutputStream)来构造一个ObjectoutputStream(对象流)对象
- 使用ObjectOutputStream对象的writeObjec(Object obj)方法就可以将参数为obj的对象写出(即保存其状态)
- 要恢复的话则用输入流