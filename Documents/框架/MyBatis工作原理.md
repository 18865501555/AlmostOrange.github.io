- 读取核心配置文件并返回`InputStream`流对象
- 根据`InputStream`流对象解析出`Configuration`对象，然后创建`SqlSessionFactory`工厂对象
- 根据一系列属性从`SqlSessionFactory`工厂中创建`SqlSession`
- 从`SqlSession`中调用`Executor`执行数据库操作&&生成具体SQL指令
- 对执行结果进行二次封装
- 提交与事务

