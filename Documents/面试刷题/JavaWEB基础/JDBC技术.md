## 说下原生JDBC操作数据库流程

- Class.forName()加载数据库连接驱动
- DriverManager.getConnection()获取数据连接对象
- 根据SQL获取sql会话对象
  - Statement
  - PrepareStatement
- 执行SQL处理结果集，执行SQL前如果有参数值就设置参数值setXXX();
- 关闭结果集，关闭会话，关闭连接

