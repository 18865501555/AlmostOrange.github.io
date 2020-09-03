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

