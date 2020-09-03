你所知道的设计模式有哪些

- Java中一般认为有23种设计模式

- 总体来说设计模式分为三大类

  ![](https://tva1.sinaimg.cn/large/007S8ZIlly1gidjamlcv2j31750u0gt3.jpg)

---

## 单例模式

- 最好理解的一种设计模式

  - 饿汉式

    ```java
    public class Singleton{
      //直接创建对象
      public static Singleton instance = new Singleton();
      //私有化构造函数
      private Singleton(){ }
      //返回对象实例
      public static Singleton getInstance(){
        return instance;
      }
    }
    ```

  - 懒汉式

    ```java
    public class Singleton{
      //声明变量
      private static volatile Singleton singleton = null;
      //私有构造函数
      private Singleton(){ }
      //提供对外方法
      public static Singleton getInstance(){
        if(singleton == null){
          synchronize(Singleton.class){
            if(singleton == null){
              singleton = new Singleton();
            }
          }
        }
        return singleton;
      }
    }
    ```

---

## 工厂设计模式

- 工厂模式分为

  - 工厂方法模式

    - 普通工厂模式

      - 建立了一个工厂类，对实现了同一接口的一些类进行实例的创建

        ```java
        public interface Sender{
          public void Send();
        }
        public class MailSender implements Sender{
          @Override
          public void Send(){
            System.out.println("this is mail sender!");
          }
        }
        public class SmsSender implements Sender{
          @Override
          public void Send(){
            System.out.println("this is sms sender!");
          }
        }
        public class SendFactory{
          public Sender produce(String type){
            if("mail".equals(type)){
              return new MailSender();
            }else if("sms".euqals(type)){
              return new SmsSender();
            }else{
              System.out.println("请输入正确的类型!");
              return null;
            }
          }
        }
        ```

    - 多个工厂方法模式

      - 对普通工厂方法模式的改进，在普通工厂方法模式中，如果传递的字符串出错，则不能正确创建对象

      - 多个工厂方法模式是提供多个工厂方法，分别创建对象

        ```java
        public class SendFactory{
          public Sender produceMail(){
            return new MailSender();
          }
          public Sender produceSms(){
            return new SmsSender();
          }
        }
        public class FactoryTest{
          pubic static void main(String[] args){
            SendFactory factory = new SendFactory();
            Sender sender = factory.produceMail();
            sender.send();
          }
        }
        ```

    - 静态工厂方法模式

      - 将多个工厂方法模式里的方法设置为静态的，不需要创建实例，直接调用即可

        ```java
        public class SendFactory{
          public static Sender produceMail(){
            return new MailSender();
          }
          public static Sender produceSms(){
            return new SmsSender();
          }
        }
        public class FactoryTest{
          public static void main(String[] args){
            Sender sender = SendFactory.produceMail();
            sender.send();
          }
        }
        ```

  - 抽象工厂模式

    - 工厂方法模式有一个问题就是，累的创建依赖工厂类，也就是说，如果想要拓展程序，必须对工厂类进行修改，这违背了闭包原则

    - 抽象工厂模式，创建多个工厂类，这样一旦需要增加新的功能，直接增加新的工厂类就可以了，不需要修改之前的代码

      ```java
      public interface Provider{
        public Sender produce();
      }
      --------------------------------------------------
      public interface Sender{
        public void send();
      }
      --------------------------------------------------
      public class MailSender implements Sender{
        @Override
        public void send(){
          System.out.println("this is mail sender!");
        }
      }
      --------------------------------------------------
      public class SmsSender implements Sender{
        @Override
        public void send(){
          System.out.println("this is sms sender!");
        }
      }
      --------------------------------------------------
      public class SendSmsFactory implements Provider{
        @Override
        public Sender produce(){
          return new SmsSender();
        }
      }
      --------------------------------------------------
      public class SendMailFactory implements Provider{
        @Override
        public Sender produce(){
          return new MailSender();
        }
      }
      --------------------------------------------------
      public class Test{
        public static void main(String[] args){
          Provider provider = new SendMailFactory();
          Sender sender = provider.produce();
          sender.send();
        }
      }
      ```

---

## 建造者模式(Builder)

- 工厂类模式提供的是创建单个类的模式，而建造者模式则是将各种产品集中起来进行管理，用来创建复合对象

- 所谓复合对象就是指某个类具有不同的属性，其实建造者模式就是前面抽象工厂模式和最后的Test结合起来得到的

  ```java
  public void produceMailSender(int count) { 
    for (int i = 0; i < count; i++) {
  		list.add(new MailSender()); 
    }
  }
  public void produceSmsSender(int count) { 
      for (int i = 0; i < count; i++) {
        list.add(new SmsSender()); 
      }
  }
  public class Builder {
  	private List<Sender> list = new ArrayList<Sender>();
  	public void produceMailSender(int count) { 
      for (int i = 0; i < count; i++) {
  			list.add(new MailSender()); 
      }
  	}
    public void produceSmsSender(int count) { 
      for (int i = 0; i < count; i++) {
        list.add(new SmsSender()); 
      }
    } 
  }
  public class TestBuilder {
  	public static void main(String[] args) {
      Builder builder = new Builder();
  		builder.produceMailSender(10);
    }
  }
  ```

---

## 适配器设计模式

- 适配器模式将某个类的接口转换成客户端期望的另一个接口表示，目的是消除由于接口不匹配所造成的类的兼容性问题。

- 主要分为三类

  - 类的适配器模式

    ```java
    public class Source {
    	public void method1() {
    		System.out.println("this is original method!"); 
      }
    }
    ----------------------------------------------------- public interface Targetable {
      /* 与原类中的方法相同 */
      public void method1();
      /* 新类的方法 */
      public void method2();
      }
    public class Adapter extends Source implements Targetable {
      @Override
      public void method2() {
      System.out.println("this is the targetable method!"); 
      }
    }
    public class AdapterTest {
      public static void main(String[] args) { 
        Targetable target = new Adapter();
        target.method1();
      	target.method2();
    	} 
    }
    ```

  - 对象的适配器模式

    - 基本思路和类的适配器模式相同，只是将 Adapter 类作修改
    - 这次不继承 Source 类，而是持有 Source 类的实例，以达到解决兼容性的问题。

    ```java
    public class Wrapper implements Targetable {	
      private Source source;
    	public Wrapper(Source source) {
      	super();
    		this.source = source; 
      }
    	@Override
    	public void method2() {
    	System.out.println("this is the targetable method!"); 
      }
    	@Override
    	public void method1() {
    	source.method1(); 
      }
    } 
    -----------------------------------------------------public class AdapterTest {
    	public static void main(String[] args) { 
        Source source = new Source(); 
        Targetable target = new Wrapper(source); 
        target.method1();
    		target.method2(); 
      }
    }
    ```

  - 接口的适配器模式

    - 有时我们写的一个接口中有多个抽象方法，当我们写该接口的实现类时，必须实现该接口的所有方法，这明显有时比较浪费
    - 因为并不是所有的方法都是我们需要的，有时只需要某一些，此处为了解决这个问题，我们引入了接口的适配器模式
    - 借助于一个抽象类，该抽象类实现了该接口，实现了所有的方法，而我们不和原始的接口打交道,只和该抽象类取得联系，所以我们写一个类，继承该抽象类，重写我们需要的方法就行。

---

## 装饰模式(Decorator)

