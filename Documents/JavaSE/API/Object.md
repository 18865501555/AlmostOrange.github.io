## Object

- 对象、东西

- Object类是所有类的父类型，Sun公司设计，将所有子类都应该有的属性和方法定义到Object类型上，所有子类型都继承了这些方法。 

- Object类型上属性和方法所有子类型都有继承。 
  - 这些包括 toString equals 等

### toString()

- 用于返回对象的文字描述信息

- Object默认的toString()返回了：类名@散列值
  - 散列值，是一个随机序号，不要当做对象的内存地址
  - 这个返回值没有用途!

- java建议对这个方法进行重写，实现用户希望的结果
  - 开发工具提供自动重写功能，利用即可

- 很多Java提供的API等都会自动调用toString, 自动调用toString可以大大节省代码
  - 字符串连接时候会自动调用
  - println() 输出对象时候
  - 等 ... 
- 默认情况下子类继承了Object 的toString, 默认值无意义！
- toString() 方法经常默认被调用， 可以节省代码
- 字符串连接时候会自动调用 toString

- 注意
  - 程序输出了 “类名@散列值” 这样的丑陋结果， 一定是你忘记了重写对象的toString！ 

### equals

- Object 定义了用于比较对象相等的方法 equals

- Java中 == 不能用于比较两个对象的“属性值”是否相等

- Object提供的默认 equals， 其实现也是 == 比较，和==功能一样。无法使用！建议在子类中进行重写！

- 开发工具提供了自动重写 equals 方法。

- Java提供类，几乎都重写了 equals 和 toString，