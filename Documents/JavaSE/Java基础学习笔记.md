## Java开发环境

### Java编译运行过程

- 编译期
  - `Java`源文件，经过编译，生成`.class`字节码文件
- 运行期
  - `JVM`加载`.class`并运行`.class`

- 特点
  - 跨平台、一次编译到处运行

### JVM、JRE、JDK名词解释

- JVM
  - `Java`虚拟机
  - 加载`.class`并运行`.class`
- JRE
  - 除了包含JVM以外，还包含了运行Java程序所必须的环境
  - JRE=JVM+Java系统类库
- JDK
  - 除了包含JRE以外，还包含了开发Java程序所必须的命令工具
  - JDK=JRE+编译、运行等命令工具
- 说明
  - 运行Java程序的最小环境为JRE
  - 开发Java程序的最小环境为JDK
  - JDK= JVM+java系统类库+编译、运行等命令工具
  - 配置环境变量
    - JAVA_HOME:指向jdk的安装路径
    - CLASSPATH:表示类的搜索路径，一般简写为`.`(当前目录)
    - PATH:指向jdk下的bin目录

### 注释

- 解释性文本
  - 单行注释：`//`
  - 多行注释：`/* */`
  - 文档注释：`/** */`

---

## 变量

