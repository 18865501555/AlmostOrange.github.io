## java.util.Timer和java.util.TimerTask基本介绍

### Timer

- 是一种工具
- 线程用其安排以后在后台线程中执行的任务
- 可安排任务执行一次，或者定期重复执行
- 实际上是个线程，定时调度所拥有的TimerTasks

### TimerTask

- 是一个抽象类
- 它的子类由Timer安排为一次执行或重复执行的任务
- 实际上就是一个拥有run方法的类
- 需要定时执行的代码放到run方法体内

## Java定时任务的基本方法

### Thread.sleep

- 创建一个Thread，然后让它在while循环里一直循环，通过sleep方法来达到定时任务的效果

```java
public class TimerTests {
    public static void main(String[] args) {
        final long timeInterval = 1000;
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                while (true) {
                    System.err.println("aaa");
                    try {
                        Thread.sleep(timeInterval);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        };
        Thread thread = new Thread(runnable);
        thread.start();
    }
}
```

### Timer和TimerTask

- 用Timer和TimerTask与第一种方法相比有如下好处
  - 当启动和取消任务时可以控制
  - 第一次执行任务可以指定你想要的delap时间

```java
import java.util.Timer;
import java.util.TimerTask;

public class TimerTests {
    public static void main(String[] args) {
        TimerTask timerTask = new TimerTask() {
            @Override
            public void run() {
                System.err.println("bbb");
            }
        };
        Timer timer = new Timer();
        long delay = 0;
        long period = 2*1000;
        timer.scheduleAtFixedRate(timerTask,delay,period);
    }
}
```

### ScheduledExecutorService

- 用ScheduledExecutorService是从java.util.concurrent里做为并发工具类被引起的，这是最理想的定时任务实现方式，相对比于上两个方法，它有以下好处
  - 相比于Timer的单线程，它是通过线程池的方式来执行任务的
  - 可以很灵活的去设定第一次执行任务delay时间
  - 提供了良好的约定，以便设定执行的时间间隔

```java
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

public class TimerTests {
    public static void main(String[] args) {
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                System.err.println("ccc");
            }
        };
        ScheduledExecutorService scheduledExecutorService = Executors.newSingleThreadScheduledExecutor();
        scheduledExecutorService.scheduleAtFixedRate(runnable, 0, 5, TimeUnit.SECONDS);
    }
}
```



