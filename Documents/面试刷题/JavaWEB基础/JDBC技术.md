## 说下原生JDBC操作数据库流程

- Class.forName()加载数据库连接驱动
- DriverManager.getConnection()获取数据连接对象
- 根据SQL获取sql会话对象
  - Statement
  - PrepareStatement
- 执行SQL处理结果集，执行SQL前如果有参数值就设置参数值setXXX();
- 关闭结果集，关闭会话，关闭连接

---

## 为什么要使用PreparedStatement

- PreparedStatement接口继承Statement,PreparedStatement实例包含已编译的SQL语句，所以其执行速度快于Statement对象
- 作为Statement的子类，PreparedStatement继承了Statement的所有功能。三种方法
  - execute
  - executeQuery
  - executeUpdate
- 在JDBC应用中，在任何时候都不需要使用Statement
  - 代码的可读性和可维护性Statement需要不断的拼接，而PreparedStatement不会
  - PreparedStatement尽最大可能提高性能。DB有缓存机制。相同的预编译语句再次被调用不会再次需要编译
  - 最重要的一点是极大的提高了安全性。Statement容易被SQL注入，而PreparedStatement传入的内容不会和sql语句发生任何匹配关系

---

## 关系数据库中连接池的机制是什么

- 为数据库连接建立一个缓冲池
- 从连接池获取或创建可用连接
- 使用完毕之后，把连接返回给连接池
- 在系统关闭前，断开所有连接并释放连接占用的系统资源
- 能够处理无效连接，限制连接池中的连接总数不低于或者不超过某个限定值
  - 最小连接数
    - 是连接池一直保持的数据连接
    - 如果应用重续对数据库连接的使用量不大，将会有大量的数据库连接资源被浪费掉
  - 最大连接数
    - 是连接池能申请的最大连接数
    - 如果数据连接请求超过此数，后面的数据连接请求将被加入到等待队列中，这回影响之后的数据库操作
  - 如果最小连接数与最大连接数相差太大，那么最先的连接请求将会获利，之后超过最小连接数量的连接请求等价于建立一个新的数据库连接
  - 不过，这些大于最小连接数的数据库连接在使用完不会马上被释放，它将被放到连接池等待重复使用或是空闲超时后被释放
  - 以上，数据库连接数量一只保持一个不少于最小连接数的数量，当数量不够时，数据库会创建一些连接，直到一个最大连接数，之后连接数据就会等待