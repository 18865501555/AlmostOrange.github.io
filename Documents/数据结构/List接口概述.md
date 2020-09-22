## 使用场景

- 如果涉及到"栈"、"队列"、"链表"等操作，优先考虑用List
  - 对于需要快速插入、删除元素-----LinkedList
  - 对于需要快速访问元素--------------ArrayList
  - 对于"单线程环境"或者"多线程环境,但是List仅被一个线程操作",考虑使用同步的类(如Vector)

## Collection

- Collection层次结构中的根接口
- 表示一组对象，这些对象也称为collection元素
- 对于Collection而言，它不提供任何直接的实现，所有的实现全部由它的子类负责

## AbstractCollection

- 提供Collection接口的骨干实现，以最大限度的减少了实现此接口所需的工作
- 对于我们而言，要实现一个不可修改的collection，只需扩展此类，并提供iterator和size方法的实现
- 但要实现可修改的collection，就必须另外重写此类add方法(否则会抛出UnsupportedOperationException),iterator方法返回的迭代器还必须另外实现其remove方法

## Iterator

- 迭代器

## ListIterator

- 系列表迭代器，允许程序员按任一方向遍历列表、迭代期间修改列表，并获得迭代器在列表中的当前位置

## List

- 继承于Collection的接口
- 代表着有序的队列

## AbstractList

- List接口的骨干实现，以最大限度的减少实现"随机访问"数据存储(如数组)支持的该接口所需的工作

## Queue

- 队列
- 提供队列基本的插入、获取、检查操作

## Deque

- 一个线性collection
- 支持在两端插入和移除元素
- 大多数Deque实现对于它们能够包含的元素数没有固定限制，但此接口既支持有容量限制的双端队列，也支持没有固定大小限制的双端队列

## AbstractSequentialList

- 提供了List接口的骨干实现，从而最大限度的减少了实现受"连续访问"数据存储(如链接列表)支持的此接口所需的工作
- 从某种意义上说，此类在列表的列表迭代器上实现"随机访问"方法

## LinkedList

- List接口的链接列表实现
- 它实现所有可选的列表操作

## ArrayList

- List接口的大小可变数组的实现
- 它实现了所有可选列表操作，并允许包括null在哪的所有元素
- 除了实现List接口外，此类还提供一些方法来操作内部用来存储列表的数组的大小

## Vector

- 实现可增长的对象数组
- 与数组一样，它包含可以使用整数索引进行访问的组件

## Stack

- 后进先出(LIFO)的对象堆栈
- 它通过五个操作对类Vector进行了扩展
- 允许将向量视为堆栈

## Enumeration

- 枚举
- 实现了该接口的对象，它生成一系列元素，一次生成一个
- 连续调用nextElement方法将返回一系列的连续元素