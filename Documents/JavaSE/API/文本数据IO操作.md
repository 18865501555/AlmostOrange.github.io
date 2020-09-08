![](https://tva1.sinaimg.cn/large/007S8ZIlly1gij7t6dujzj31580u0jx9.jpg)

## Reader和Writer

### 字符流原理

- Reader
  - 字符输入流的父类
- Writer
  - 字符输出流的父类
- 字符流是以字符(char)为单位读写数据的。一次处理一个unicode
- 字符流的底层仍然是基本的字节流
- 字符流封装了字符的编码解码算法

### 常用方法

- Reader
  - int read()
    - 读取一个字符，返回的int值"低十六"位有效
  - int read(char[ ] chs)
    - 从该流中读取的一个字符数组的length个字符并存入该数组，返回值为实际读取到的字符量
- Writer
  - void write(int c)
    - 写出一个字符，写出给定int值"低十六"位表示的字符
  - void write(char[ ] chs)
    - 将给定字符数组中所有字符写出
  - void write(String str)
    - 将给定的字符串写出
  - void write(char[ ] chs,int offset,int len)
    - 将给定的字符数组中从offset处开始连续的len个字符写出

## 转换流

### 字符转换流原理

- InputStreamReader
  - 字符输入流
  - 使用该流可以设置字符集，并按照指定的字符集从流中按照该编码将字节数据转换为字符并读取
- OutputSteamWriter
  - 字符输出流
  - 使用该流可以设置字符集，并按照指定的字符集将字符转换为对应字节后通过该流写出

### 指定字符编码

- InputStreamReader的构造方法允许我们设置字符集
  - InputStreamReader(InputStream in,String charsetName)
    - 基于给定的字节输入流以及字符编码创建ISR
  - InputStreamReader(InputStream in)
    - 该构造方法会根据系统默认字符集创建ISR
- OutputStreamWriter的构造方法
  - OutputStreamWriter(OutputStream out,String charsetName)
    - 基于给定的字节输出流以及字符编码创建OSW
  - OutputStreamWriter(OutputStream out)
    - 该构造方法会根据系统默认字符集创建OSW

### 使用OSW

```java
public void testOutput() throws IOException{
  FileOutputStream fos = new FileOutputStream("demo.txt");
  OutputStreamWriter writer  = new OutputStreamWriter(fos,"UTF-8");
  String str = "XXX";
  writer.write(str);
  writer.close();
}
```

### 按照指定编码将文本写入文件

```java
public void testOSW() throws Exception{
  FileOutputStream fos = new FileOutputStream("osw.txt");
  //根据指定编码写出字符串，编码名称忽略大小写
  OutputStreamWriter osw = new OutputStreamWriter(fos,"GBK");
  osw.write("XXX");
  osw.close();
}
```

### 使用ISR

```java
public void testInput() throws IOException{
  FileInputStream fis = new FileInputStream("demo.txt");
  InputStreamReader reader = new InputStreamReader(fis,"UTF-8");
  int c = -1;
  while((c = reader.read()) !=-1){
    System.out.println((char)c);
  }
  reader.close();
}
```

### 读出特定编码的文本文件

```java
public void testISR() throws Exception{
  FileInputStream fis = new FileInputStream("osw.txt");
  //根据指定编码读取字符串，编码名称忽略大小写
  InputStreamReader isr = new InputStreamReader(fis,"GBK");
  int chs = -1;
  while((chs = isr.read()) != -1){
    System.out.println((char)chs);
  }
  isr.close();
}
```

## PrintWriter

### 构建PW对象

- PrintWriter是具有自动行刷新的缓冲字符输出流
- 构造方法
  - PrintWriter(File file)
  - PrintWriter(String fileName)
  - PrintWriter(OutputStream out)
  - PrintWriter(OutputStream out, boolean autoFlush)
  - PrintWriter(Writer writer)
  - PrintWriter(Writer writer, boolean autoFlush)
- 其中参数为OutputStream于Writer的构造方法提供了一个可以传入boolean值参数，该参数用于表示PrintWriter是否具有自动行刷新

### print和prinln方法

- PrintWriter提供了丰富的重载print与println方法
- println方法在于输出目标数据后自动输出一个系统支持的换行符
- 若该流是具有自动行刷新的，那么每当通过println方法写出的内容都会被实际写出，而不是进行缓存
- 常用方法
  - void print(int i)
    - 打印整数
  - void print(char c)
    - 打印字符
  - void print(boolean b)
    - 打印boolean值
  - void print(char[ ] c)
    - 打印字符数组
  - void print(double d)
    - 打印double值
  - void print(float f)
    - 打印float值
  - void print(long l)
    - 打印long值
  - void print(String str)
    - 打印字符串

### 使用PW输出字符数据

```java
public void testPrintWriter() throws IOException{
  FIleOutputStream fos = new FileOutputStream("demo.txt");
  OutputStreamWriter osw = new OutputStreamWriter(fos,"UTF-8");
  //创建带有自动行刷新的PW
  PrintWriter pw = new PrintWriter(osw,true);
  //立即写出该字符串
  pw.println("xxx");
  //关闭时，pw也会先进行flush
  pw.close();
}
```

### 将日志信息写入文本文件

```java
public void testPrintWrite() throws Exception{
  PrintWriter pw = new PrintWriter({pw.txt});
  pw.println("xxx");
  pw.println("XXX");
  pw.close();
}
```

## BufferedReader

### 构建BufferedReader对象

- BufferedReader是缓冲字符输入流，其内部提供了缓冲区，可以提高读取效率
- BufferedReader的常用构造方法
  - BufferedReader(Reader reader)

### 创建BufferedReader对象

```java
public void testBufferedReader() throws IOException{
  FileInputStream fis = new FileInputStream("demo.txt");
  InputStreamReader isr = new InputStreamReader(fis);//这里可以指定编码
  BufferedReader br = new BufferedReader(isr);
  ...
}
```

### 使用BR读取字符串

- BufferedReader提供了一个可以便于读取一行字符串的方法
  - String readLine()
    - 该方法连续读取一行字符串
    - 直到读取到换行符位置
    - 返回的字符串中不包含该换行符

### 读取一个文本文件的所有行输出到控制台

```java
public void testBufferedReader() throws Exception{
  FileInputStream fis = new FileInputStream("pw.txt");
  InputStreamReader isr = new InputStreamReader(fis);
  BufferedReader br = new BufferedReader(isr);
  String line = null;
  while((line = br.readLine())!=null){
    System.out.println(line);
  }
  br.close();
}
```

