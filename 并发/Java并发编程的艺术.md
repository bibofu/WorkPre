

## 一：并发编程的挑战

并发编程的目的是为了让程序运行得更快，但是，并不是启动更多的线程就能让程序最大限度地并发执行。在进行并发编程时，如果希望通过多线程执行任务让程序运行得更快，会面临非常多的挑战，比如上下文切换的问题、死锁的问题，以及受限于硬件和软件的资源限制问题



### 上下文切换

即使是单核处理器也支持多线程执行代码，CPU 通过给每个线程分配 CPU 时间片来实现这个机制。时间片是 CPU 分配给各个线程的时间，因为时间片非常短，所以 CPU通过不停地切换线程执行，让我们感觉多个线程是同时执行的，时间片一般是几十毫秒（ms）。CPU 通过时间片分配算法来循环执行任务，当前任务执行一个时间片后会切换到下一个任务。但是，在切换前会保存上一个任务的状态，以便下次切换回这个任务时，可以再加载这个任务的状态。所以任务从保存到再加载的过程就是一次上下文切换



#### 多线程一定快吗？

```java
public class Demo4 {
    public static final long count=100000L;

    public static void main(String[] args) throws InterruptedException {

        concurrency();
        serial();

    }
    private static void concurrency() throws InterruptedException{
        long start=System.currentTimeMillis();
        Thread thread=new Thread(new Runnable() {
            @Override
            public void run() {
                int a=0;
                for(long i=0;i<count;i++){
                    a+=5;
                }
            }
        });
        thread.start();

        int b=0;
        for(long i=0;i<count;i++){
            b--;
        }
        long time=System.currentTimeMillis()-start;
        thread.join();

        System.out.println("concurrency: "+time+"ms,b="+b);
    }

    private static void serial(){
        long start=System.currentTimeMillis();
        int a=0;
        for(long i=0;i<count;i++){
            a+=5;
        }
        int b=0;
        for(long i=0;i<count;i++){
            b--;
        }
        long time =System.currentTimeMillis()-start;
        System.out.println("serial: "+time+"ms,b="+b+",a="+a);
    }
}
```

![2022 02 15 14 11 28](https://s4.ax1x.com/2022/02/15/Hg4jH0.png)



为什么并发执行的速度会比串行慢呢？这是因为线程有创建和上下文切换的开销



#### 测试上下文切换次数和时长

下面我们来看看有什么工具可以度量上下文切换带来的消耗。
      使用 Lmbench3可以测量上下文切换的时长。
      使用 vmstat 可以测量上下文切换的次数。



#### 如何减少上下文切换

减少上下文切换的方法有无锁并发编程、CAS 算法、使用最少线程和使用协程。

1.  无锁并发编程。多线程竞争锁时，会引起上下文切换，所以多线程处理数据时，可以用一些办法来避免使用锁，如将数据的 ID 按照 Hash 算法取模分段，不同的线程处理不同段的数据。
2.  CAS 算法。Java 的 Atomic 包使用 CAS 算法来更新数据，而不需要加锁。
3.  使用最少线程。避免创建不需要的线程，比如任务很少，但是创建了很多线程来处理，这样会造成大量线程都处于等待状态。
4.  协程：在单线程里实现多任务的调度，并在单线程里维持多个任务间的切换。



#### 减少上下文切换实战



### 死锁

一旦产生死锁，就会造成系统功能不可用。下面这段代码会引起死锁

```java
public class DeadLock {
    private static String A="A";
    private static String B="B";

    public static void main(String[] args) throws Exception{

        deadLock();

    }

    private static void deadLock() throws InterruptedException {

        Thread t1=new Thread(new Runnable() {
            @Override
            public void run() {
                //得到A的锁
                synchronized (A){
                    try {
                        //因为暂停,导致t2先得到B的锁
                        Thread.currentThread().sleep(2000);

                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                    //得到B的锁
                    synchronized (B){
                        System.out.println("1");
                    }
                }
            }
        });

        Thread t2=new Thread(new Runnable() {
            @Override
            public void run() {
                //得到B的锁
                synchronized (B){
                    //得到A的锁
                    synchronized (A){
                        System.out.println("2");
                    }
                }
            }
        });
        t1.start();
        t2.start();

    }
}
```



现在我们介绍避免死锁的几个常见方法。

1.  避免一个线程同时获取多个锁。
2.   避免一个线程在锁内同时占用多个资源，尽量保证每个锁只占用一个资源。
3.   尝试使用定时锁，使用 lock.tryLock（timeout）来替代使用内部锁机制。
4.  对于数据库锁，加锁和解锁必须在一个数据库连接里，否则会出现解锁失败的情况。



### 资源限制的挑战

资源限制是指在进行并发编程时，程序的执行速度受限于计算机硬件资源或软件资源。例如，服务器的带宽只有 2Mb/s，某个资源的下载速度是 1Mb/s 每秒，系统启动 10个线程下载资源，下载速度不会变成 10Mb/s，所以在进行并发编程时，要考虑这些资源的限制。硬件资源限制有带宽的上传/下载速度、硬盘读写速度和 CPU 的处理速度。软件资源限制有数据库的连接数和 socket 连接数等



## 二：Java 并发机制的底层实现原理

Java 代码在编译后会变成 Java 字节码，字节码被类加载器加载到 JVM 里，JVM 执行字节码，最终需要转化为汇编指令在 CPU 上执行，Java 中所使用的并发机制依赖于JVM 的实现和 CPU 的指令。



### volatile 的应用

在多线程并发编程中 synchronized 和 volatile 都扮演着重要的角色，volatile 是轻量级的 synchronized，它在多处理器开发中保证了共享变量的“可见性”。可见性的意思是当一个线程修改一个共享变量时，另外一个线程能读到这个修改的值。如果 volatile 变量修饰
符使用恰当的话，它比 synchronized 的使用和执行成本更低，因为它不会引起线程上下文的切换和调度。



#### volatile 的定义与实现原理

Java 编程语言允许线程访问共享变量，为了确保共享变量能被准确和一致地更新，线程应该确保通过排他锁单独获得这个变量。Java 语言提供了 volatile，在某些情况下比锁要更加方便。如果一个字段被声明成volatile，Java 线程内存模型确保所有线程看到这个变量的值是一致的。



CPU 术语的定义：

![2022 02 15 15 34 07](https://s4.ax1x.com/2022/02/15/H2Vr9K.png)



![2022 02 15 15 37 32](https://s4.ax1x.com/2022/02/15/H2ZG5t.png)



#### volatile 的使用优化



### synchronized 的实现原理与应用

在多线程并发编程中 synchronized 一直是元老级角色，很多人都会称呼它为重量级锁。但是，随着 Java SE 1.6 对 synchronized 进行了各种优化之后，有些情况下它就并不那么重了。本文详细介绍 Java SE 1.6 中为了减少获得锁和释放锁带来的性能消耗而引入的偏向锁和轻量级锁，以及锁的存储结构和升级过程



