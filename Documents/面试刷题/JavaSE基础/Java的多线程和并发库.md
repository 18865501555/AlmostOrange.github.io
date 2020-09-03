## 请模拟写出双重定时器

- 要求:使用定时器，间隔4秒执行一次，再间隔2秒执行一次，以此类推执行

  ```java
  class TimerTastCus extends TimerTask{
    @Override
    public void run(){
      count = (count + 1) % 2;
      System.err.println("Boob boom");
      new Timer().schedule(new TimerTastCus(),2000+2000*count);
    }
  }
  Timer timer = new Timer();
  timer.schedule(new TimerTastCus(),2000+2000*count);
  while(true){
    System.out.println(new Date().getSeconds());
    try{
      Thread.sleep(1000);
    }catch(InterruptedException e){
      e.printStackTrace();
    }
  }
  /*
   *	下面的代码中的count变量中
   *	此参数要使用再匿名内部类中，使用final修饰就无法对其值进行修改
   *	只能改为静态变量
   */	
  private static volatile int count = 0;
  ```

---

