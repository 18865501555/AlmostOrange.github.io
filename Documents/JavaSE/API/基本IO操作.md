![](https://tva1.sinaimg.cn/large/007S8ZIlly1gij187gh47j30u00zdgu8.jpg)

## IS与OS

### 输入与输出

- 输入
  - 是一个从外界进入到程序的方向，通常我们需要"读取"外界的数据时，使用输入
  - 输入是用来**读取数据**的
- 输出
  - 是一个从程序发送到外界的方向，通常我们需要"写出"数据到外界时，使用输出
  - 使出是用来**写出数据**的

### 节点流和处理流

- 按照流是否直接与特定的地方(如磁盘、内存、设备等)相连，分为节点流和处理流两类
- 节点流
  - 可以从或向一个特定的地方(节点)读写数据
- 处理流
  - 是对一个一存在的流的连接和封装，通过所封装的流的功能调用实现数据读写
- 处理流的构造方法总是要带一个其他的流对象做参数
- 一个流对象经过其他流的多次包装，称为流的链接
- 通常节点流也称为低级流
- 通常处理流也称为高级流或过滤流

### IS和OS常用方法

- InputStream
  - 所有字节输入流的父类，其定义了基础的读取方法
  - int read()
    - 读取一个字节，以int形式返回，该int值的"低八位"有效，若返回值为-1则表示EOF
  - int read(byte[ ] d)
    - 尝试最多读取给定数组的length个字节并存入该数组，返回值为实际读取到的字节量
- OutputStream
  - 所有字节数处流的父类，其定义了基础的写出方法
  - void write(int d)
    - 写出一个字节，写的是给定的int的"低八位"
  - void write(byte[ ] d)
    - 将给定的字节数组中的所有字节全部写出

## 文件流

### 创建FOS对象(重写模式)

- FileOutputStream

  - 文件的字节输出流，我们使用该流可以以字节为单位将数据写入文件
  - 构造方法
    - FileOutputStream(File file)
      - 创建一个向指定File对象表示的文件中写出数据的文件输出流
    - FileOutputStream(String filename)
      - 创建一个向具有指定名称的文件中写出数据的文件输出流

- 若指定的文件已经包含内容，那么当使用FOS对其写入数据时，会将该文件中原有数据全部清除

  ```java
  public void testFosByAppend() throws Exception{
    //创建文件字节输出流
    FileOutputStream fos = new FileOutputStream("fos.dat");
    //写出一组字节
    fos.write("helloworld".getBytes());
    fos.close();
  }
  ```

### 创建FOS对象(追加模式)

- 若想在文件的原有数据之后追加新数据，则需要以下构造方法创建FOS

  - 构造方法
    - FileOutputStream(File file, boolean append)
      - 创建一个向指定File对象表示的文件中写出数据的文件输出流
    - FileOutputStream(String filename, boolean append)
      - 创建一个向具有指定名称的文件中写出数据的文件输出流

- 若第二个参数为true，那么通过该FOS写出的数据都是在文件末尾追加的

  ```java
  public void testFos() throws Exception{
    //创建文件字节数处流
    FileOutputStream fos = new FileOutputStream("fos.dat",true);
    //写出一组字节
    fos.write("helloworld".getBytes());
    fos.close();
  }
  ```

### 创建FIS对象

- FileInputStream是文件的字节输入流，我们使用该流可以以字节为单位从文件中读取数据

- FileInputStream有两个常用的构造方法

  - FileInputStream(File file)
    - 创建一个从指定File对象表示的文件中读取数据的文件输入流
  - FileInputStream(String name)
    - 创建用于读取给定的文件系统中的路径名name所指定的文件的文件输入流

  ```java
  public void testFis() throws Exception{
    //根据给定的File对象创建文件输入流
    //FileInputStream fis = new FileInputStream(new File("raf.dat"));
    //根据给定的文件路径创建文件输入流
    FileInputStream fis = new FileInputStream("raf.dat");
    int d = -1;
    while((d=fis.read())!=-1){
      System.out.println((char)d +"");
    }
    fis.close();
  }
  ```

### read()和write(int d)方法

- FileInputStream继承自InputStream，其提供了以字节为单位读取文件数据的方法read
  - int read()
    - 从此输入流中读取一个数据字节，若返回-1则表示EOF(End Of File)
- FileOutputStream继承自OutputStream，其提供了以字节为单位向文件写数据的方法write
  - void write(int d)
    - 将指定字节写入此文件输入流
    - 这里只写给定的int值的"低八位"

### 实现文件复制

```java
public void testCopyFile1() throws Exception{
  FileInputStream fis = new FileInputStream("fos.dat");
  FileOutputStream fos = new FileOutputStream("fos_copy1.dat");
  int d = -1;
  while((d = fis.read())!=-1){
    fos.write(d);
  }
  System.out.println("复制完毕");
  fis.close();
  fos.close();
}
```

```java
public void testCopyFile2() throws Exception{
  FileInputStream fis = new FileInputStream("fos.dat");
  FileoutputStream fos = new FileOutputStream("fos_copy2.dat");
  int len = -1;
  byte[] buf = new byte[32];
  while((len = fis.read(buf))!=-1){
    fos.write(buf,0,len);
  }
  System.out.println("复制完毕");
  fis.close();
  fos.close();
}
```

### read(byte[ ] b)和write(byte b)方法

- FileInputStream也支持批量读取字节数据的方法
  - int read(byte[ ] b)
    - 从此输入流中将最多b.length个字节的数据读入到字节数组b中
- FileOutputSteam也支持批量写出字节数据的方法
  - void write(byte[ ] d)
    - 将b.length个字节从指定byte数组写入此文件输出流中
  - void write(byte[ ] dint offset,int len)
    - 将指定byte数组从偏移量off开始的len个字节写入此文件输出流

## 缓冲流

### BOS基本工作原理

- 在想硬件设备作出写操作时，增大写出次数会降低写出效率，为此我们可以使用缓冲输出流来一次性批量写出若干数据减少写出次数来提高写出效率
- BufferedOutputStream缓冲输出流内部维护着一个缓冲区，每当我们向该流写数据时，都会先将数据存入缓冲区，当缓冲区已满时，缓冲流会将数据一次性全部写出

### BOS实现输出缓冲

```java
public void testBos() throws Exception{
  FileOutputStream fos = new FileOutputStream("demo.dat");
  //创建缓冲字节输出流
  BufferedOutputStream bos = new BufferedOutputStream(fos);
  //所有字节被存入缓冲区，等待一次性写出
  bos.write("helloworld".getBytes());
  //关闭流之前，缓冲输出流会将缓冲区内容一次性写出
  bos.close();
}
```

### BOS的flush方法

- 使用缓冲输出流可以提高写出效率，但是也存在一个问题就是写出数据缺乏即时性
- 有时候我们需要在执行完某些写出操作后，就希望将这些数据确实写出，而非在缓冲区中保持直接到缓冲区满后才写出
- void flush()
  - 清空缓冲区，将缓冲区中的数据强制写出

### BIS基本工作原理

- 在读取数据时若以字节为单位读取数据，会导致读取次数过于频繁从而大大降低读取效率
- 为此我们可以通过提高一次读取的字节数量减少读写次数来提高读取的效率
- BufferedInputStream是缓冲字节输入流
  - 内部维护着一个缓冲区(字节数组),使用该流在读取一个字节时，该流会尽可能多的一次性读取若干字节并存入缓冲区
  - 然后逐一的将字节返回，直到缓冲区中的数据被全部读取完毕，会再次读取若干字节从而反复
  - 减少了读取的次数，从二提高了读取效率
- BIS是一个处理流，该流为我们提供了缓冲功能

### BIS实现输入缓冲

```java
public void testBis() throws Exception{
  //创建缓冲字节输入流
  FileInputStream fis = new FileInputSteam("demo.dat");
  BufferedInputStream bis = new BufferedInputStream(fis);
  int d = -1;
  //缓冲读入，实际上并非是一个字节一个字节从文件读取的
  while((d = bis.read())!=-1){
    System.out.print(d+"");
  }
  bis.close();
}
```

### 实现基于缓存区的文件复制

```java
public void testCopy() throws Exception{
  FileInputStream fis = new FileInputStream("fos.dat");
  //创建缓冲字节输入流
  BufferedInputStream bis = new BufferedInputStream(fis);
  FileOutputStream fos = new FileOutputStream("fos_copy3.dat");
  BufferedOutputStream bos = new BuffereddOutputStream(fos);
  int d = -1;
  while((d = bis.read())!=-1){
    bos.write(d);
  }
  System.out.println("复制完毕");
  bis.close();
  bos.close();
}
```

## 对象流

### 对象序列化概念

- 对象是存在于内存中的
- 有时候我们需要将对象保存到硬盘上，又有时候我们需要将对象传输到另一台计算机上等等这样的操作
- 这时我们需要将对象转换为一个字节序列，这个过程就称为对象序列化
- 相反，我们有这样一个字节序列就需要将其转换为对应的对象，这个过程就称为对象的反序列化

### 使用OOS实现对象序列化

- ObjectOutputStream
  - 用来对对象进行序列化的输出流
- 实现对象系列化的方法
  - void writeObject(Object o)
    - 该方法可以将给定的对象转换为一个字节序列后写出

### 使用OIS实现对象反序列化

- ObjectInputStream
  - 用来对对象进行反序列化的输入流
- 实现对象反序列化的方法
  - Object readObject()
    - 该方法可以从流中读取字节并转换为对应的对象

### Serializable接口

- ObjectOutputStream在对对象进行序列化时有一个要求
  - 需要序列化的对象所属的类**必须实现Serializable接口**
- 实现该接口不需要重写任何方法
- 只是作为可序列化的标志
- 通常实现该接口的类需要提供一个常量serialVersionUID，表明该类的版本。若不现实的声明，在对象序列化时也会根据当前类的各个方面计算该类的默认serialVersionUID，但不同平台编译器实现有所不同，所以若想跨平台，都应显示的声明版本号
- 如果声明的类的对象序列化存到硬盘上面，之后随着需求的变化更改了累的属性(增加或减少或改名)，那么当反序列化时就会出现InvalidClassException，这样就会造成不兼容性的问题
- 当serialVersionUID相同时，他就会将不一样的field以type的预设值反序列化，可避开不兼容性的问题

### Transient关键字

- 对象在序列化后得到的字节序列往往比较大，有时我们在对一个对象进行序列化时可以忽略某些不必要的属性，从而对序列化后得到的字节序列"瘦身"
- 关键字transient
- 被该关键字修饰的属性在序列化时其值将被忽略

### 实现Emp的序列化

```java
public void testOOS() throws Exception{
  FileOutputStream fos = new FileOutputStream("emp.obj");
  ObjectOutputStream oos = new ObjectOutputStream(fos);
  Emp emp = new Emp("张三",15,"男",4000);
  oos.writeObject(emp);
  System.out.println("序列化完毕");
  oos.close();
}
```

### 实现Emp的反序列化

```java
public void testOIS() throws Exception{
  FileInputStream fis = new FileInputStream("emp.obj");
  ObjectInputStream ois = new ObjectInputStream(fis);
  Emp emp = (Emp)ois.readObject();
  System.out.println("反序列化完毕");
  System.out.println(emp);
  ois.close();
}
```

