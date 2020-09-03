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
  - 字符流
    - 继承于InputStreamReader和OutputStreamWriter

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gidaygc9orj31lw0u00ww.jpg)

![](https://tva1.sinaimg.cn/large/007S8ZIlly1gidb3h8pdoj31e40ridjq.jpg)

