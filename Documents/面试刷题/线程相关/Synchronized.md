## 当一个线程进入一个对象的一个`synchronized`方法后，其他线程是否可以进入此对象的其他方法？

- 不可以，一个对象的一个`synchronized`方法只能由一个线程访问。

## 简述`synchronized`和`java.util.concurrent.locks.Lock`的异同

- 主要相同点
  - `Lock`能完成`synchronized`所实现的所有功能
- 主要不同点
  - `Lock`有比`synchronized`更精确的线程语义和更好的性能
  - `synchronized`会自动释放锁，而`Lock`一定要求程序员手工释放，并且必须在`finally`从句中释放

