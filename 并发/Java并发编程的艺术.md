

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



先来看下利用 synchronized 实现同步的基础：Java 中的每一个对象都可以作为锁。具体表现为以下 3 种形式。

1.  对于普通同步方法，锁是当前实例对象。
2.  对于静态同步方法，锁是当前类的 Class 对象。
3.  对于同步方法块，锁是 Synchonized 括号里配置的对象



当一个线程试图访问同步代码块时，它首先必须得到锁，退出或抛出异常时必须释放锁。那么锁到底存在哪里呢？锁里面会存储什么信息呢？



从 JVM 规范中可以看到 Synchonized 在 JVM 里的实现原理，JVM 基于进入和退出Monitor 对象来实现方法同步和代码块同步，但两者的实现细节不一样。代码块同步是使用 monitorenter 和 monitorexit 指令实现的，而方法同步是使用另外一种方式实现的，细
节在 JVM 规范里并没有详细说明。但是，方法的同步同样可以使用这两个指令来实现。

monitorenter 指令是在编译后插入到同步代码块的开始位置，而 monitorexit 是插入到方法结束处和异常处，JVM 要保证每个 monitorenter 必须有对应的 monitorexit 与之配对。任何对象都有一个 monitor 与之关联，当且一个 monitor 被持有后，它将处于锁定状态。线程执行到 monitorenter 指令时，将会尝试获取对象所对应的 monitor 的所有权，即尝试获得对象的锁



#### Java 对象头

在HotSpot虚拟机中，对象在堆内存中的存储布局可以划分为三个部分：对象头(Header)，实例数据(Instance Data) 和对齐填充(padding)。

![2022 02 16 16 34 34](https://s4.ax1x.com/2022/02/16/Hh9IG8.png)



![2022 02 16 16 38 28](https://s4.ax1x.com/2022/02/16/HhC6YV.png)



![2022 02 16 16 37 03](https://s4.ax1x.com/2022/02/16/HhC4m9.png)



![2022 02 16 16 37 41](https://s4.ax1x.com/2022/02/16/HhPCff.png)



#### 锁的升级与对比

Java SE 1.6 为了减少获得锁和释放锁带来的性能消耗，引入了“偏向锁”和“轻量级锁”，在 Java SE 1.6 中，锁一共有 4 种状态，级别从低到高依次是：无锁状态、偏向锁状态、轻量级锁状态和重量级锁状态，这几个状态会随着竞争情况逐渐升级。锁可以升级但不能降级，意味着偏向锁升级成轻量级锁后不能降级成偏向锁。



##### 偏向锁

当一个线程访问同步块并获取锁时，会在对象头和栈帧中的锁记录里存储锁偏向的线程 ID，以后该线程在进入和退出同步块时不需要进行 CAS 操作来加锁和解锁，只需简单地测试一下对象头的 Mark Word 里是否存储着指向当前线程的偏向锁。如果测试成功，表示线程已经获得了锁。如果测试失败，则需要再测试一下 Mark Word 中偏向锁的标识是否设置成 1（表示当前是偏向锁）：如果没有设置，则使用 CAS 竞争锁；如果设置了，则尝试使用CAS 将对象头的偏向锁指向当前线程。



###### 偏向锁的撤销

偏向锁使用了一种等到竞争出现才释放锁的机制，所以当其他线程尝试竞争偏向锁时，持有偏向锁的线程才会释放锁。偏向锁的撤销，需要等待全局安全点（在这个时间点上没有正在执行的字节码）。它会首先暂停拥有偏向锁的线程，然后检查持有偏向锁的线程是否活着，如果线程不处于活动状态，则将对象头设置成无锁状态；如果线程仍然活着，拥有偏向锁的栈会被执行，遍历偏向对象的锁记录，栈中的锁记录和对象头的Mark Word 要么重新偏向于其他线程，要么恢复到无锁或者标记对象不适合作为偏向锁，最后唤醒暂停的线程。

![2022 02 16 16 53 49](https://s4.ax1x.com/2022/02/16/HhAUqe.png)



###### 关闭偏向锁