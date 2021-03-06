### File定义

- file 文件
  - 代表文件、文件夹，用于操作文件、文件夹。 

- Java提供API，可以用于对文件、文件进行操作：
  - 创建文件夹、文件夹改名、创建文件、文件改名、删除文件、删除文件夹

- file.getName() 获取当前文件夹、文件的名字
- Absolute 绝对 Path 路径，从根目录到当前目录的绝对位置信息

### 创建文件夹

- ./代表当前目录，当前项目文件夹
- File 提供了创建文件夹的方法 mkdir() 执行这个方法，就会在磁盘上创建一个文件夹
  - 如果文件夹不存在，就会创建文件夹，返回true
  - 如果文件夹已经存在，就不做任何操作，返回false，表示没有创建
  - 如果磁盘有问题，有故障，会出现“异常” 出现红字
- mkdirs() 创建系列文件夹，可快速创建一系列文件夹

### 删除文件或者文件夹

- delete()
  - 文件就直接删除，返回true
  - 空文件夹直接删除，返回true
  - 非空文件夹不删除，返回false， 为了安全，避免意外删除

### 创建空文件

- createNewFile() 调用方法会创建文件
  - 如果返回true表示文件创建成功
  - 如果返回false表示文件创建失败
  - 在一个文件夹中不能同时存在相同的文件、文件夹，这里的同名是文件名可扩展名都相同
  - 文件夹和文件的名字都可以包括主文件名和扩展名

### 检查文件、文件夹是否存在

- exists() 检查当前file对象，对应的磁盘文件、文件夹是否已经存在
  - 如果存在就返回true
  - 如果不存在就返回false

### 检查是文件还是文件夹的方法

- File 提供了检查是文件还是文件夹的方法
- isFile() 检查是否是文件
  - 如果是文件就返回true
- isDirectory() 检查是否文件夹
  - 如果是文件夹就是true
- 如果根本不存在的文件
  - 这个两个方法返回值都是false

### File 提供的API可以检查相关的属性

- 检查文件的属性
  - 文件长度、文件名称、文件的父目录、是否可以读取
- getCanonicalFile() 
  - 获取规范名称

### 文件改名

- rename 改名 
- 原文件.renameTo(新文件) 
  - 返回true，表示成功

### 列文件夹的内容（列目录）

- listFiles() 获取文件夹file的全部内容
  - 内容就是子文件夹和文件。
- 在文件上调用 listFiles 会得到 null！

### 有条件的列目录内容

- 是一个重载的listFiles
- 例子
  - 将首字母为 “S”文件列出来
  - 过滤条件 = name.startsWith("S");
  - file.listFiles(过滤条件);
- 创建过滤条件: file.getName().startsWith("I")