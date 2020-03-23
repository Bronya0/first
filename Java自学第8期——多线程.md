## 1、多线程：
操作系统支持同时运行多个任务，一个任务通常是一个程序，所有运行中的程序就是一个**进程**()。程序内部包含多个顺序执行流，每个顺序执行流就是一个**线程**。

并发：两个或者多个事件在同一个时间段内交替发生；
并行：两个或者多个事件在同一时刻同时发生；

某时间段内宏观上有多个程序同时运行，在单cpu系统中，每一时刻只有单个程序在运行，但在微观上cpu是交替执行他们的。
多cpu系统中，这些多任务被分配到多个cpu上，这样多个任务就是同时运行。
### 1.1 线程与进程
进程：进程是并发执行程序在执行过程中资源分配和管理的基本单位（资源分配的最小单位）。进程可以理解为一个应用程序的执行过程，
应用程序一旦执行，就是一个进程。每个进程都有自己独立的地址空间，每启动一个进程，
系统就会为它分配地址空间，建立数据表来维护代码段、堆栈段和数据段。
线程：程序执行的最小单位。一个进程至少有一个线程，拥有多线程的应用程序被称为多线程程序。
### 1.2 线程调度
分时调度：所有线程轮流使用cpu，平均分配每个orffff线程占用cpu的时间
抢占式调度：优先让优先级高的线程使用cpu，优先级相同则随机选择。java使用抢占式调度。
设置线程优先级：任务管理器打开详细信息，右键设置线程优先级
## 1.3 创建线程类
java.lang.Thread类代表线程，所有线程类都必须是Thread类或者是其子类的实例，每个线程的作用是
完成一定的任务，即是执行一段程序流即一段顺序执行的代码，java使用线程执行体来代表这段程序流。
通过继承Thread类来创建并启动多线程：
1.定义Thread的子类，重写run()方法，其方法体代表线程执行的任务，该run()方法叫线程执行体、
2.创建子类的对象
3.调用线程对象的start()方法来启动该线程

```java
//定义测试类
public class Demo01 {
    public static void main(String[] args){
        Demo02 thread1 = new Demo02("线程1");
        thread1.run();
    }
}
```

```java
//定义自定义线程类
class Demo02 extends Thread{

    //定义指定线程名称的构造方法
    public Demo02(String name){
        //调用父类的String参数的构造方法，指定线程的名称
        super(name);
    }
    @Override
    public void run() {
        for (int i = 0; i <10 ; i++) {
            System.out.println(getName()+" is runnimg "+i);
        }
    }
}
```
## 2、了解Thread类
java.lang.Thread
构造方法：
public Thread() :分配一个新的线程对象。
public Thread(String name) :分配一个指定名字的新的线程对象。
public Thread(Runnable target) :分配一个带有指定目标新的线程对象。
public Thread(Runnable target,String name) :分配一个带有指定目标新的线程对象并指定名字。
常用方法：
public String ==getName==() :获取当前线程名称。
public void ==start==() :导致此线程开始执行; Java虚拟机调用此线程的run方法。
public void ==run==() :此线程要执行的任务在此处定义代码。
public static void ==sleep==(long millis) :使当前正在执行的线程以指定的毫秒数暂停（暂时停止执行）。
public static Thread ==currentThread==() :返回对当前正在执行的线程对象的引用。
## 2.1 创建线程的方式有两种：
一种是继承Thread类，另一种是实现Runnable接口方式
推荐实现Runnable接口：
1.定义该接口的实现类，并重写该接口的run()方法，即为线程的执行体
2.创建该实现类的实例，并以该例作为Thread的target来创建Thread线程对象，该对象为真正的线程对象
3.调用线程对象的start()方法启动线程
## 2.2 Thread类和Runnable类的区别
如果一个类继承Thread，则不适合资源共享。但是如果实现了Runable接口的话，则很容易的实现资源共享。
总结：
实现Runnable接口比继承Thread类所具有的优势：
1. 适合多个相同的程序代码的线程去共享同一个资源。
2. 可以避免java中的单继承的局限性。
3. 增加程序的健壮性，实现解耦操作，代码可以被多个线程共享，代码和线程独立。
4. 线程池只能放入实现Runable或Callable类线程，不能直接放入继承Thread的类。
扩充：在java中，每次程序运行至少启动2个线程。一个是main线程，一个是垃圾收集线程。因为每当使用
java命令执行一个类的时候，实际上都会启动一个JVM，每一个JVM其实在就是在操作系统中启动了一个进
程。

```java
public class Demo04 implements Runnable {//定义实现类

    //重写run方法
    @Override
    public void run() {
        for (int i = 0; i <3 ; i++) {
            System.out.println(Thread.currentThread().getName()+i);
        }
    }

}
```

```java
class test1 {
    public static void main(String[] args) {
        //创建实现类对象
        Demo04 obj = new Demo04();
        //将该实现类对象给创建的线程对象,并赋名。参考Thread类的构造方法
        Thread thread1 = new Thread(obj,"线程");
        thread1.start();
    }
}
```
### 2.3匿名内部类方式实现线程的创建
使用该方式，可以方便的实现每个线程执行不同的任务操作
使用该方式实现Runnable接口：

```java
public class Demo05 {
    public static void main(String[] args) {
//        new Runnable(){
//            @Override
//            public void run() {
//                for (int i = 0; i < 3; i++) {
//                    System.out.println(Thread.currentThread().getName()+i);
//                }
//            }
//        };
        Runnable r = new Runnable(){
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName());
            }
        };

        new Thread(r).start();

        for (int i = 0; i <4 ; i++) {
            System.out.println(Thread.currentThread().getName()+i);
        }
    }
}
```
后面学习使用lambda表达式优化写法。
## 3、线程安全问题
线程安全：多个线程同时运行，执行同一段代码时，如果程序每次运行结果和单线程运行的结果是一样的，同时其他变量的值和预期也是一样。
### 3.1 线程同步
要解决多线程并发访问同一资源可能出现的安全问题。可通过同步机制来解决(synchronized)
要求在某个线程修改共享资源的时候，其他线程不能修改该资源，等待修改完毕且同步后，
才能去抢夺cpu资源，完成对应的操作，保证了数据的同步性，从而保证线程安全。

有三种方法完成同步操作：
1、同步代码块
2、同步方法
3、锁机制

**同步代码块:**
synchronized 关键字可以用于方法中的某个区块中，表示只对这个区块的资源实行互斥访问
线程开始执行同步代码块之前，必须先获得同步监视器的锁定。任何时刻只能有一个线程可以获得对同步监视器的锁定，当同步代码块执行完成后，该线程会释放对该同步监视器的锁定。

格式：
synchronized(同步锁/同步监视器){
    需要同步操作的代码
}

**同步锁：**
理解为给对象上锁，
1、锁对象，可以是任意类型
2、多个线程对象，要使用同一把锁

注意：任何时候，最多允许一个线程拥有同步锁，谁拿到锁就进入代码块，其他线程只能在外等着。

```java
public class Demo07 implements Runnable{
    private int num = 10;
    //创建lock
    Object lock = new Object();

    @Override
    public void run() {
        while(true) {//一直执行
            //添加同步锁
            synchronized(lock) {
                if (num > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    String name = Thread.currentThread().getName();
                    System.out.println(name + "数字为：" + num--);
                }
            }
        }
    }
}
```

```java
class test2{
    public static void main(String[] args) {
        Demo07 x = new Demo07();
        Thread a = new Thread(x,"线程a");
        Thread b = new Thread(x,"线程b");
        Thread c = new Thread(x,"线程c");
        a.start();
        b.start();
        b.start();
    }
}
```
此时打印的内容已经没有重复的出现了，已经线程安全

**同步方法：**
概念：使用synchronized修饰的方法，叫做同步方法，保证a线程执行该方法的时候，其他方法只能在外面等着。

格式：
public synchronized void method(){
    可能会产生线程安全问题的代码
}

同步锁，
对于非static方法，同步锁就是this，
对于static方法，我们使用当前方法所在类的字节码对象（类名.class)
 

```java
public class Demo08 implements Runnable {
    private int num = 10;

    @Override
    public void run() {
        while(true){//一直执行
            method1();
        }
    }

```

```java
//同步方法
    public synchronized void method1(){
        if (num>0){
            try{
                Thread.sleep(100);
            }catch (InterruptedException e){
                e.printStackTrace();
            }
            //获取当前线程的名字
            String name = Thread.currentThread().getName();
            System.out.println(name+"数字为："+num--);
        }
    }
}
```
### 3.2 Lock锁
java.util.concurrent.locks.Lock机制提供了比synchronized代码块和synchronized方法更加广泛的锁定操作，同步代码块/同步方法具有的功能Lock都有，除此之外更强大，更体现面向对象。
Lock锁又称同步锁，加锁与释放锁方法化了，如下：

public void lock()：加同步锁。
public void unlock()：释放同步锁。

```java
public class Demo09 implements Runnable {
    private int num = 10;
    //创建lock
    Lock lock = new ReentrantLock();

    @Override
    public void run() {
        while (true) {//一直执行
            //添加同步锁
            lock.lock();
            if (num > 0) {
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }finally {
                    //释放同步锁,finally确保一定会执行
                    lock.unlock();
                }
                String name = Thread.currentThread().getName();
                System.out.println(name + "数字为：" + num--);
            }
        }
    }
}
```

```java
class test5{
    public static void main(String[] args) {
        Demo09 obj = new Demo09();
        Thread thread = new Thread(obj,"线程x");
        thread.start();
    }
}
```
## 4、线程状态
当线程被创建并启动以后，它既不是一启动就进入了执行状态，也不是一直处于执行状态。在线程的生命周期中，有几种状态呢？在API中java.lang.Thread.State 这个枚举中给出了六种线程状态：
Object中两个方法 wait()和notify()方法，

1.NEW(新建) :
线程刚被创建，但是并未启动。还没调用start方法。

2.Runnable(可运行):
线程可以在java虚拟机中运行的状态，可能正在运行自己代码，也可能没有，这取决于操
作系统处理器。

3.Blocked(锁阻塞):
当一个线程试图获取一个对象锁，而该对象锁被其他的线程持有，则该线程进入Blocked状
态；当该线程持有锁时，该线程将变成Runnable状态。

4.Waiting(无限等待):
一个线程在等待另一个线程执行一个（唤醒）动作时，该线程进入Waiting状态。进入这个
状态后是不能自动唤醒的，必须等待另一个线程调用notify或者notifyAll方法才能够唤醒。

5.Timed Waiting(计时等待):
同waiting状态，有几个方法有超时参数，调用他们将进入Timed Waiting状态。这一状态
将一直保持到超时期满或者接收到唤醒通知。带有超时参数的常用方法有Thread.sleep 、
Object.wait。

6.Teminated(被终止):
因为run方法正常退出而死亡，或者因为没有捕获的异常终止了run方法而死亡。

### 4.1 Timed Waiting(计时等待)
一个正在限时等待另一个线程执行一个（唤醒）动作的线程处于这一状态。在run方法中添加sleep语句，就强制当前正在执行的线程休眠（暂停执行），以减慢线程。
当我们调用了sleep方法之后，当前正在执行是线程进入休眠状态。

1.调用sleep方法，单独的线程可以调用，不一定非要有协作关系
2.为了让其他线程有机会执行，可以将Thread.sleep()调用放在线程run()之内。以保证
该线程执行过程中会睡眠。
3.sleep与锁无关，线程睡眠到期自动苏醒，并返回到Runnable（可运行状态）
4.sleep中指定的时间是线程暂停运行的最短时间，不能保证时间到期后立即就会执行

 ### 4.2 BLOCKED(锁阻塞)：
一个正在阻塞等待一个监视器锁（锁对象）的线程处于这一状态。
线程a和线程b使用同一个锁，如果线程a获取到锁，则进入Runnable状态，那么线程b进入
到Blocked锁阻塞状态（即b没有争取到锁对象）。

```java
public class Demo10 extends Thread {
    //实现一个计数器，计数到100，
    // 在每个数字之间暂停1秒，每隔10个数字输出一个字符串
    public void run(){
        for (int i = 0;i < 100;i++){
            if ((i) % 10 == 0){
                System.out.println("——————" + i);
            }
            System.out.println(i);
            try{
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    public static void main(String[] args){
        new Demo10().start();
    }
}
```
### 4.3 Waiting（无限等待）
一个正在无限期等待另一个线程执行一个特别的（唤醒）动作的线程处于这一状态。
Onject中的两个方法：

void wait():在其他线程调用此对象的 notify() 方法或 notifyAll() 方法前，让当前线程等待。

void notify():唤醒此对象监视器上等待的单线程,继续执行wait之后的代码

void notify():唤醒所有正在等待的线程

```java
public class Demo11 {
    //创建锁对象，保证唯一
    public static Object obj = new Object();

    public static void main(String[] args) {
        //演示Waiting
        //
        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    //添加同步锁,保证只有一个线程执行
                    synchronized (obj) {
                        try {
                            System.out.println(Thread.currentThread().getName() + "获取到锁对象，调用wait方法，进入waiting状态，释放锁对象");
                            obj.wait();//无限等待

                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                        System.out.println(Thread.currentThread().getName() + "===从waiting状态醒来，获取到锁对象，继续执行了");
                    }
                }
            }
        }, "等待线程").start();


        new Thread(new Runnable() {
            @Override
            public void run() {
//                while(true){

                try {
                    System.out.println(Thread.currentThread().getName() + "——等待3秒钟");
                    Thread.sleep(3000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

                synchronized (obj) {
                    System.out.println(Thread.currentThread().getName() + "获取到时锁对象，调用notify方法。释放锁对象");
                    obj.notify();
                }
            }
        }, "唤醒线程").start();
    }
}
```
## 5、线程池：
如果避免频繁启动和销毁线程（尤其是大量生命短的线程），造成系统性能下降，使用线程池的方法。

线程池创建大量空闲线程，当线程池接收创建的Runnable对象，便会将一个空闲的线程执行该对象的run()方法，执行结束后回归空闲状态，暂时不进行销毁。

线程池还能控制并发线程的数量，通过他的最大线程数目的参数。

java5后Executors工厂类生产线程池，静态方法：

newFixedThreadPool(int nThreads)：创建一个可重用的、具有固定线程数的线程池.
newScheduledThreadPool(int corePoolSize):创建一个线程池，指定延迟后执行。

java8新增：
==newWorkStealingPool==(in parallelism)：
创建持有足够的线程池来支持给定的并行级别，该方法还会使用多个队列减少竞争，可以无参数，自动匹配当前可支持最高并行级别，ExecutorService代表尽快执行的线程池，提交Runnable对象则会尽快执行该任务。静态方法：

Future<?> ==submit==(Runnable task):
提交Runnable对象，线程池将在有空闲时执行该任务。run()执行完后返回null。

<T> Future<>T ==submit==(Runnable task, T result)：
提交给一个Runnable对象给线程池，空闲时执行。run()方法结束后返回result。

用完后，调用该线程池的shutdown()方法,变不再接受新的任务，但会将之前提交的任务完成。全部完成后，池中所有线程会死亡。

调用shutdownNow()方法，则立刻停止所有正在执行的活动任务，暂停正在等待的任务，并返回等待执行的任务的列表。

**总步骤**：
1、调用Executors类的静态工厂方法创建一个ExecutorService对象，该对象代表一个线程池。
2、创建Runnable实现类，作为线程执行任务。
3、调用ExecutorService对象的submit()方法来提交Runnable实例。
4、当不想提交任何任务时，调用ExecutorService对象的submit()方法来结束线程池。

```java
public class Demo12_ThreadPool {
    public static void main(String[] args) throws Exception{
        //创建一个固定6个线程的线程池
        ExecutorService pool = Executors.newFixedThreadPool(6);
        Runnable obj1 = new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName() + "执行");
            }
        };
        Runnable obj2 = new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName() + "执行");
            }
        };
        pool.submit(obj1);
        pool.submit(obj2);
        //关闭线程池
        pool.shutdown();
    }
}
```
下一期记录lambda表达式的使用方法。
