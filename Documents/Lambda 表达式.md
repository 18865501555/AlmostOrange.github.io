# Lambda 表达式

- JDK8之后推出的一个新特性
- 可以用更精简的语法实现匿名内部类

---

[toc]



## 语法

```java
(实参列表)->{
  方法体
}
```

## 注

- lambda实现的接口只能有一个抽象方法
- 否则无法使用lambda

## 创建过滤条件

### 原始

```java
package file;
import java.io.File;
import java.io.FileFilter;
public class LambdaDemo{
  public static void main(String[] args){
    FileFilter filter = new FileFilter(){
      public boolean accept(File file){
        return file.getName().startWith("S");
      }
    };
  }
}
```

### 简化

```java
public class LambdaDemo{
  public static void main(String[] args){
    FileFilter filter = (File file)->{
      return file.getName().startWith("S");
    }
  }
}
```

### 再简化

- 方法类型的参数可以不写，编译器会自动分析出来

```java
public class LambdaDemo{
  public static void main(String[] args){
    FileFilter filter = (file)->{
      return file.getName().startWith("S");
    }
  }
}
```

### 最终

- 如果方法中只有一句代码，那么方法体的{ }可以不写
- 并且如果这句代码有return时，那么return也要一同省略

```java
public class LambdaDemo{
  public static void main(String[] args){
    FileFilter filter = (f)->file.getName().startWith("S");
  }
}
```

### 例

```java
public class LambdaDemo{
  public static void main(String[] args){
    File file = new File("./src/string");
    File[] files = file.listFiles(
      (f)->file.getName().startWith("S")
    );
    for(File f : files){
      System.out.println(f);
    }
  }
}
```

### 案例

#### 列出当前项目目录下所有的文本文件名字

- 要求
  - 使用Lambda表达式

```java
public class Test01{
  public static void main(String[] args){
    File file = new File(./);
    if(file.isDirectory()){
      File[] files = file.listFiles((f)->f.getName().endWith(".txt"));
      for(File f : files){
        System.out.pringln(f.getName());
      }
    }
      
  }
}
```

- boolean isFile()
  - 判断当前File表示的是否为一个文件
    - 若是则返回true
- boolean isDirectory()
  - 判断当前File表示的是否为一个目录
    - 若是则返回true