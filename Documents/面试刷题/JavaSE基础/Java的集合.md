## HashMap排序

- 已知一个HashMap<Integer,User>集合，User有name(String)和age(int)属性。请写一个方法实现对HashMap的排序功能，该方法接收Hashmap<Integer,User>为形参，返回类型为HashMap<Integer,User>要求对HashMap中的User的age倒序进行排序。排序时key=value键值对不得拆散

  ```java
  public class HashMapTest{
    public static void main(String[] args){
      HashMap<Integer,User> users = new HashMap<>();
      user.put(1,new User("张三",25));
      user.put(3,new User("李四",22));
      user.put(2,new User("王五",28));
      System.out.println(users);
      HashMap<Integer,User> sortHashMap = sortHashMap(users);
      System.out.println(sortHashMap);
      /**
      	*	控制台输出内容
        *	(1=User [name=张三,age=25],2=User [name=王五,age=28],3=User [name=李四,age=22])
        *	(2=User [name=王五,age=28],1=User [name=张三,age=25],3=User [name=李四,age=22])
        */
      public static Hashmap<Integer,User> sortHashMap(HashMap<Integer,User>map){
        //	首先拿到map的键值对集合
        Set<Entry<Integer,User>> entrySet - map.entrySet();
        //	将set集合转为List集合，为了使用工具类的排序方法
        List<Entry<Integer,User>> list = new ArrayList<Entry<Integer,User>>(entrySet);
        //	使用Collections集合工具类对list进行排序，排序规则使用匿名内部类来实现
        Collections.sort(list,new Comparator<Entry<Integer,User>>(){
          @Override
          public int compare(Entry<Integer,User> o1,Entry<Integer,User> o2){
            //按照要求根据User的age的倒序进行排序
            return o2.getValue().getAge()-o1.getValue().getAge();
          }
        });
        //创建一个新的有序的HashMap子类的集合
        LinkedHashMap<Integer,User> linkedHashMap = new LinkedHashMap<Integer,User>();
        //将List中的数据存储在LinkedHashMap中
        for(Entry<Integer,User> entry : list){
          linkedHashMap.put(entry.getKey(),entry.getValue());
        }
        //返回结果
        return linkedHashMap;
      }
    }
  }
  ```

  - HashMap本身就是不可排序的，但是该题偏偏让给HashMap排序，那我们就得想在API中有没有这样的Map结构时有序的
  - LinkedHashMap，是Map结构也是链表结构，有序的，是HashMap的子类，我们返回LinkedHashMap<Integer,User>即可，符合面向接口(父类编程的思想)
  - 但凡是对集合的操作，我们应该保持一个原则就是能用JDK中的API就用，比如排序算法我们不应该去用冒泡或者选择，而是首先想到用Collections集合工具类

---

## ArrayList、HashSet、HashMap是线程安全的吗？如果不是，我想要线程安全的集合怎么办？

- ArrayList、HashSet、HashMap的源码，每个方法都没有加锁，所以都是线程不安全的

- 在集合中Vector和HashTable是线程安全的，打开源码会发现其实就是把各自核心方法添加上了synchronized关键字

- Collections工具类提供了相关的API，可以让上面3个不安全的集合变为安全的

  ```java
  Collections.synchronizedCollection(c);
  Collections.synchronizedList(list);
  Collections.synchronizedMap(m);
  Collections.synchronizedSet(s);
  ```

  - 上面几个函数都有对应的返回值类型，传入什么类型返回什么类型

---

## ArrayList内部用什么实现的

- ArrayList内部是用Object[]实现的
  - 空参构造
    - array是一个Object[]类型
    - 当我们new一个空参构造时系统调用了EmptyArray.OBJECT属性，也就是使用了一个new Object[0]数组
  - 有参构造ArrayList(int capacity)
    - 该构造函数传入一个int值作为数组的长度值
    - 如果值小于0，则抛出一个运行时异常
    - 如果值等于0，则使用一个空数组
    - 如果值大于0，则创建一个长度为该值的新数组
  - 有参构造ArrayList(Collection<? extends E> collection)
    - 该构造函数传入一个Collection子类
    - 先判断该集合是否为null
      - 为null，抛出空指针异常
      - 不为null，将该集合转换为数组a，然后将数组赋值为成员变量array，将该数组的长度作为成员变量size
  - toArray方法
    - 是Collection接口定义的，因此其所有的子类都有这样的方法，list集合的toArray和Set集合的toArray返回的都是Object[]数组
  - add方法
  - remove方法
  - clear方法

---

## List的三个子类的特点

- ArrayList
  - 底层结构是数组
  - 底层查询快，增删慢
- LinkedList
  - 底层结构是链表型
  - 增删快，查询慢
- voctor
  - 底层结构是数组，线程安全的
  - 增删慢，查询慢

---

## List和Map、Set的区别

- 结构特点
  - List和Set是存储单列数据的集合
  - Map是存储键值对的双列数据的集合
  - List中存储的数据有序并且允许重复
  - Map中存储的数据无序其键不能重复，值可以重复
  - Set中存储的数据是无序的不许有重复，但元素在集合中的位置由元素的hashcode决定，位置是固定的
  - Set集合根据hashcode来进行数据的存储，所以位置是固定的，但是位置不是用户可以控制的，所以对于用户来说set中的元素还是无序的
- 实现类
  - List
    - 接口有三个实现类
    - LinkedList
      - 基于链表实现，链表内存是散乱的，每一个元素存储本身内存地址的同时还存储下一个元素的地址
      - 链表增删快，查找慢
    - ArrayList
      - 基于数组实现
      - 非线程安全的，效率高，便于索引，但不便于插入删除
    - Vector
      - 基于数组实现，线程安全的，效率低
  - Map
    - 接口有三个实现类
    - Hashmap
      - 基于hash表的Map接口实现，非线程安全
      - 高效，支持null值和null键
    - HashTable
      - 线程安全
      - 低效，不支持null值和null键
    - LinkedHashmap
      - HashMap的一个子类，保存了记录的插入顺序
      - SortMap接口
        - TreeMap能够把它保存的记录根据键排序，默认是键值的升序排序
  - Set
    - 接口两个实现类
    - HashSet
      - 底层是由HashMap实现
      - 不允许集合中有重复的值，使用时需重写equals()和hashCode()方法
    - LinkedHashSet
      - 继承于HashSet
      - 同时又基于LinkedHashMap来进行实现
      - 底层使用的是LinkedHashMap
- 区别
  - List
    - 对象按照索引位置排序
    - 可以有重复对象
    - 允许按照对象在集合中的索引位置检索对象
      - 例如通过list.get(i)方法来获取集合中的元素
  - Map
    - 每一个元素包含一个键和一个值，成对出现
    - 键对象不可重复，值对象可以重复
  - Set
    - 对象不按照特定的方式排序，并且没有重复对象
    - 实现类能对集合中的对象按照特定的方式排序
      - TreeSet类，可以按照默认顺序
      - 实现Java.util.Comparator<Type>接口来自定义排序方式

---

## HashMap和HashTable有什么区别

- HashMap
  - 线程不安全,效率比HashTable效率高
  - 是一个接口
  - 是Map的一个子接口
  - 是将键映射到值的对象
  - 不允许键值重复
  - 允许空键和空值
  - 被多个线程访问时需要自己为它的方法实现同步
- HashTable
  - 线程安全的一个集合
  - 不允许null值作为一个key值或者value值
  - 是sychronize，多个线程访问时不需要自己为它的方法实现同步

---

## 数组和链表的区别

- 数组
  - 将元素在内存中连续存储的
  - 优点
    - 因为数据是连续存储的，内存地址连续，所以在查找数据的时候效率比较高
  - 缺点
    - 在存储之前，我们需要申请一块连续的内存空间，并且在编译的时候就必须缺点好它的空间的大小
    - 在运行的时候空间的大小是无法随着你的需要进行增加和减少而改变的
    - 当数据量比较大的时候，有可能会出现越界的情况
    - 当数据量比较小的时候，有可能会浪费掉内存空间
    - 在改变数据个数时，增加插入删除数据效率比较低
- 链表
  - 是动态申请内存空间，不需要像数组需要提前申请好内存的大小
  - 链表只需要在用的时候申请就可以，根据需要来动态申请或者删除内存空间
  - 对于数据增加和删除以及插入比数组灵活
  - 链表中数据在内存中可以在任意的位置，通过应用来关联数据(通过存在元素的指针来联系)

---

## 链表和数组使用场景

- 数组应用场景
  - 数据比较少
  - 经常做的运算是按序号访问数据元素
  - 数组更容易实现，任何高级语言都支持
  - 构建的线性表较稳定
- 链表应用场景
  - 对线性表的长度或者规模难以估计
  - 频繁做插入删除操作
  - 构建动态性比较强的线性表

---

## Java中ArrayList和LinkedList区别

- ArrayList
  - ArrayList和Vector使用了数组的实现
  - 可以认为ArrayList或者Vector风中了对内部数组的操作
    - 比如向数组中添加，删除，插入新的元素或者数据的扩展和重定向
- LinkedList
  - 使用了循环双向链表数据结构
  - 与基于数组的ArrayList相比，这是两种截然不同的实现技术
  - 由一系列列表项连接而成
    - 一个表项总是包含3个部分
      - 元素内容
      - 前驱表
      - 后驱表
  - 无论LinkedList是否为空，链表内部都有一个heder表项，它既表示链表的开始，也表示链表的结尾
    - 表项header的后驱表项便是链表中第一个元素
    - 表项header的前驱表项便是链表中最后一个元素

---

## List a=new ArrayList()和ArrayList a = new ArrayList()的区别

- List a = new ArrayList()

  - 创建了一个ArrayList的对象后把上溯到了List

  - 此时它是一个List对象，有些ArrayList有但是List没有的属性和方法，它就不能再用了

    ```java
    List list = new ArrayList();
    list.trimToSize();//错误，没有该方法
    ```

- ArrayList a = new ArrayList()

  - 创建一个对象则保留了ArrayList的所有属性

  - 所以需要用到ArrayList独有的方法的时候不能用前者

    ```java
    ArrayList list = new ArrayList();
    list.trimToSize();//ArrayList里有该方法
    ```

---

## 要对集合更新操作时，ArrayList和LinkedList哪个更适合

- ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构
- 如果集合数据是对于集合随机访问get和set，ArrayList绝对优于LinkedList，因为LinkedList要移动指针
- 如果集合数据是对于集合新增和删除操作add和remove，LinkedList比较占优势，因为ArrayList要移动数据
- ArrayList和LinkedList是两个集合类，用于存储一系列的对象引用(references)
- 总结
  - 对 ArrayList 和 LinkedList 而言，在列表末尾增加一个元素所花的开销都是固定的。对 ArrayList 而言，主要是在内部数组中增加一项，指向所添加的元素，偶 尔可能会导致对数组重新进行分配;而对 LinkedList 而言，这 个开销是统一的，分配一个内部 Entry 对象
  - 在 ArrayList 的中间插入或删除一个元素意味着这个列表中剩余的元素都会被移动;而在 LinkedList 的中间 插入或删除一个元素的开销是固定的。
  - LinkedList 不支持高效的随机元素访问。
  - ArrayList 的空间浪费主要体现在在 list 列表的结尾预留一定的容量空间，而 LinkedList 的空间花费则体现在 它的每一个元素都需要消耗相当的空间
  - 当操作是在一列数据的后面添加数据而不是在前面或中间,并且需要随机地访问其中的元素时
    - 使用ArrayList 会提供比较好的性能
  - 当你的操作是在一列数据的前面或中间添加或删除数据,并且按照顺序访问其中的元 素时
    - 就应该使用 LinkedList 了

---

## 请用两个队列模拟堆栈结构

- 队列是先进先出
- 堆栈式先进后出
- 队列a和b
  - 入栈
    - a队列为空，b为空
    - 例将"a,b,c,d,e"需要入栈的元素先放a中，a入栈为"a,b,c,d,e"
  - 出栈
    - a队列目前的元素为"a,b,c,d,e"
    - 将a队列依次加入ArrayList集合a中
    - 以倒序的方法将a中的集合取出，放入b队列中，再将b队列出列

```java
//a队列
Queue<String> queue = new linkedList<String>();
//b队列
Queue<String> queue2 = new LinkedList<String>();
//arraylist集合是中间参数
ArrayList<String> a = new ArrayList<String>();
//往a队列添加元素
queue.offer("a");
queue.offer("b");
queue.offer("c");
queue.offer("d");
queue.offer("e");
System.out.print("进栈:");//进栈:a b c d e 
//将a队列依次加入list集合中
for(String q : queue){
  a.add(q);
  System.out.print(q);
}
//以倒序的方法取出(a队列依次加入list集合)之中的值，加入b队列
for(int i=a.size()-1;i>=0;i++){
  queue2.offer(a.get(i));
}
//打印出栈队列
System.out.println("");
System.out.println("出栈:");//出栈:e d c b a 
for(String q : queue2){
  System.out.print(q);
}
```

---

