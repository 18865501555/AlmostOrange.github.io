![](https://tva1.sinaimg.cn/large/007S8ZIlly1gij0bnoutfj30q00n2abm.jpg)

## 文件指针操作

### getFilePointer()方法

- RandomAccessFile的读写操作都是基于指针的

- 总是在指针当前所指向的位置进行读写操作

- 方法

  - long getFilePointer()
    - 该方法用于获取当前RandomAccessFile的指针位置

  ```java
  RandomAccessFile raf = new RandomAccessFile(file,"rw");
  System.out.println(raf.getFilePointer());//0
  raf.write('A');
  System.out.println(raf.getFilePointer());//1
  raf.writeInt(3);
  System.out.println(raf.getFilePointer());//5
  raf.close();
  ```

### seek()方法

- RandomAccessFile提供了一个方法用于移动指针位置

- 方法

  - void seek(long pos)
    - 该方法用于移动当前RandomAccessFile的指针位置

  ```java
  RandomAccessFile raf = new RandomAccessFile(file,"rw");
  System.out.println(raf.getFilePointer());//0
  raf.write('A');//指针位置1
  raf.writeInt(3);//指针位置5
  //将指针移动到文件开始处(第一个字节的位置)
  raf.seek(0);
  System.out.println(raf.getFilePointer());//0
  raf.close();
  ```

### skipBytes()方法

- RandomAccessFile提供了一个方法可以尝试跳过输入的n个字节以丢弃跳过的字节
- 方法
  - int skipBytes(int n)
    - 该方法可能跳过一些较少数量的字节(可能包括零)
    - 这可能是由任意数量的条件引起
    - 在跳过n个字节之前已达到文件的末尾只是其中的一种可能
    - 此方法不抛出EOFException
    - 返回跳过的实际字节数
    - 如果n为负数，则不跳过任何字节

### 测试文件指针的相关方法

```java
public void testPointer() throws Exception{
  RandomAccessFile raf = new RandomAccessFile("raf.dat","r");
  //输出指针位置，默认从0开始(文件的第一个字节位置)
  System.out.println("指针位置:"+raf.getFilePointer());
  //读取world，需要将hello这5个字节跳过
  raf.skipBytes(5);
  System.out.println("指针位置:"+raf.getFilePointer());
	//读取world这5个字节
  byte[] buf = new byte[5];
  raf.read(buf);
  System.out.println(new String(buf));
  System.out.println("指针位置:"+raf.getFilePointer());
	//将有表移动到文件开始
  raf.seek(0);
  System.out.println("指针位置:"+raf.getFilePointer());
	raf.close();
}
```

