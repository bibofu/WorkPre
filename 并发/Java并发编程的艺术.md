

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

偏向锁在 Java 6 和 Java 7 里是默认启用的，但是它在应用程序启动几秒钟之后才激活，如有必要可以使用 JVM 参数来关闭延迟：-XX:BiasedLockingStartupDelay=0。



##### 轻量级锁

争夺锁导致的锁膨胀流程图：

![2022 02 16 19 25 19](https://s4.ax1x.com/2022/02/16/HhBxDf.png)



###### 轻量级锁加锁

线程在执行同步块之前，JVM 会先在当前线程的栈桢中创建用于存储锁记录的空间，并将对象头中的 Mark Word 复制到锁记录中，官方称为 Displaced Mark Word。然后线程尝试使用 CAS 将对象头中的 Mark Word 替换为指向锁记录的指针。如果成功，当前线程获得锁，如果失败，表示其他线程竞争锁，当前线程便尝试使用自旋来获取锁。

###### 轻量级锁解锁

轻量级解锁时，会使用原子的 CAS 操作将 Displaced Mark Word 替换回到对象头，如果成功，则表示没有竞争发生。如果失败，表示当前锁存在竞争，锁就会膨胀成重量级锁。
因为自旋会消耗 CPU，为了避免无用的自旋（比如获得锁的线程被阻塞住了），一旦锁升级成重量级锁，就不会再恢复到轻量级锁状态。当锁处于这个状态下，其他线程试图获取锁时，都会被阻塞住，当持有锁的线程释放锁之后会唤醒这些线程，被唤醒的线程就会进行新一轮的夺锁之争

![2022 02 16 19 32 27](https://s4.ax1x.com/2022/02/16/HhrGWj.png)



### 原子操作的实现原理

原子（atomic）本意是“不能被进一步分割的最小粒子”，而原子操作（atomic operation）意为“不可被中断的一个或一系列操作”。



#### 术语定义

![2022 02 16 19 35 06](https://s4.ax1x.com/2022/02/16/HhrIte.png)



#### 处理器如何实现原子操作

使用总线锁保证原子性

使用缓存锁保证原子性



#### Java 如何实现原子操作

在 Java 中可以通过锁和循环 CAS 的方式来实现原子操作

##### 使用循环 CAS 实现原子操作

JVM 中的 CAS 操作正是利用了处理器提供的 CMPXCHG 指令实现的。自旋 CAS 实现的基本思路就是循环进行 CAS 操作直到成功为止，以下代码实现了一个基于 CAS 线程安全的计数器方法 safeCount 和一个非线程安全的计数器 count。

```java
public class CASDemo {
    private AtomicInteger atomicI=new AtomicInteger(0);
    private int i=0;

    public static void main(String[] args) {

        final CASDemo cas=new CASDemo();
        List<Thread> ts=new ArrayList<Thread>(600);
        long start=System.currentTimeMillis();
        for(int i=0;i<100;i++){
            Thread t=new Thread(new Runnable() {
                @Override
                public void run() {
                    for(int j=0;j<10000;j++){
                        cas.count();
                        cas.safeCount();
                    }
                }

            });
            ts.add(t);
        }

        for (Thread t:ts){
            t.start();
        }

        for(Thread t:ts){
            try {
                t.join();
            }catch (Exception e){
                e.printStackTrace();
            }
        }

        System.out.println(cas.i);
        System.out.println(cas.atomicI.get());
        System.out.println(System.currentTimeMillis()-start);

    }

    /**
     *  使用CAS实现线程安全计数器
     */
    private void safeCount(){
        for(;;){
            int i=atomicI.get();
            boolean suc=atomicI.compareAndSet(i,++i);
            if(suc){
                break;
            }
        }
    }

    /**
     * 非线程安全计数器
     */
    private void count(){
        i++;
    }
}

```



###### CAS 实现原子操作的三大问题：

###### **ABA 问题**

因为 CAS 需要在操作值的时候，检查值有没有发生变化，如果没有发生变化则更新，但是如果一个值原来是 A，变成了 B，又变成了 A，那么使用 CAS 进行检查时会发现它的值没有发生变化，但是实际上却变化了。ABA 问题的解决思路就是使用版本号。在变量前面追加上版本号，每次变量更新的时候把版本号加 1，那么A→B→A 就会变成 1A→2B→3A。从 Java 1.5 开始，JDK 的 Atomic 包里提供了一个类
AtomicStampedReference 来解决 ABA 问题。



###### 循环时间长开销大

自旋 CAS 如果长时间不成功，会给 CPU 带来非常大的执行开销。



###### 只能保证一个共享变量的原子操作

why？

Java中的CAS操作只是对CPU的cmpxchgq指令的一层封装。它的功能就是一次只原子地修改一个变量。

当对一个共享变量执行操作时，我们可以使用循环 CAS 的方式来保证原子操作，但是对多个共享变量操作时，循环 CAS 就无法保证
操作的原子性，这个时候就可以用锁。还有一个取巧的办法，就是把多个共享变量合并成一个共享变量来操作。



##### 使用锁机制实现原子操作

锁机制保证了只有获得锁的线程才能够操作锁定的内存区域。JVM 内部实现了很多种锁机制，有偏向锁、轻量级锁和互斥锁。有意思的是除了偏向锁，JVM 实现锁的方式都用了循环 CAS，即当一个线程想进入同步块的时候使用循环 CAS 的方式来获取锁，当它退出同步块的时候使用循环 CAS 释放锁



### 本章小结

本章我们一起研究了 volatile、synchronized 和原子操作的实现原理。Java 中的大部分容器和框架都依赖于本章介绍的 volatile 和原子操作的实现原理，了解这些原理对我们进行并发编程会更有帮助



## 三：Java内存模型

Java 线程之间的通信对程序员完全透明，内存可见性问题很容易困扰 Java 程序员，本章将揭开 Java 内存模型神秘的面纱。本章大致分 4 部分：

**Java 内存模型的基础**，主要介绍内存模型相关的基本概念；

**Java 内存模型中的顺序一致性**，主要介绍重排序与顺序一致性内存模型；

**同步原语**，主要介绍 3 个同步原语（synchronized、volatile 和 final）的内存语义及重排序规则在处理器中的实现；

**Java 内存模型的设计**，主要介绍 Java 内存模型的设计原理，及其与处理器内存模型和顺序一致性内存模型的关系



### Java 内存模型的基础

#### 并发编程模型的两个关键问题

在并发编程中，需要处理两个关键问题：**线程之间如何通信**及**线程之间如何同步**（这里的线程是指并发执行的活动实体）。通信是指线程之间以何种机制来交换信息。在命令式编程中，线程之间的通信机制有两种：==共享内存==和==消息传递==

Java 的并发采用的是共享内存模型，Java 线程之间的通信总是隐式进行，整个通信过程对程序员完全透明



#### Java 内存模型的抽象结构

在 Java 中，所有实例域、静态域和数组元素都存储在堆内存中，堆内存在线程之间共享（本章用“共享变量”这个术语代指实例域，静态域和数组元素）。



Java 线程之间的通信由 Java 内存模型（本文简称为 JMM）控制，JMM 决定一个线程对共享变量的写入何时对另一个线程可见。从抽象的角度来看，JMM 定义了线程和主内存之间的抽象关系：线程之间的共享变量存储在主内存（Main Memory）中，每个线程都有一个私有的本地内存（Local Memory），本地内存中存储了该线程以读/写共享变量的副本。本地内存是 JMM 的一个抽象概念，并不真实存在。它涵盖了缓存、写缓冲区、寄存器以及其他的硬件和编译器优化。

![2022 02 16 20 21 58](https://s4.ax1x.com/2022/02/16/HhRH56.png)



如果线程 A 与线程 B 之间要通信的话，必须要经历下面 2 个步骤。

1.  线程 A 把本地内存 A 中更新过的共享变量刷新到主内存中去。
2.  线程 B 到主内存中去读取线程 A 之前已更新过的共享变量



![2022 02 16 20 24 16](https://s4.ax1x.com/2022/02/16/HhWiPf.png)



JMM 通过控制主内存与每个线程的本地内存之间的交互，来为 Java程序员提供内存可见性保证



#### 从源代码到指令序列的重排序

在执行程序时，为了提高性能，编译器和处理器常常会对指令做重排序。重排序分3 种类型。
1. 编译器优化的重排序。编译器在不改变单线程程序语义的前提下，可以重新安排语句的执行顺序。
2. 指令级并行的重排序。现代处理器采用了指令级并行技术（InstructionLevelParallelism，ILP）来将多条指令重叠执行。如果不存在数据依赖性，处理器可以改变语句对应机器指令的执行顺序。
3. 内存系统的重排序。由于处理器使用缓存和读/写缓冲区，这使得加载和存储操作看上去可能是在乱序执行



![2022 02 16 20 29 21](https://s4.ax1x.com/2022/02/16/HhWhJf.png)



#### 并发编程模型的分类

虽然写缓冲区有这么多好处，但每个处理器上的写缓冲区，仅仅对它所在的处理器可见。这个特性会对内存操作的执行顺序产生重要的影响：处理器对内存的读/写操作的执行顺序，不一定与内存实际发生的读/写操作顺序一致！



![2022 02 16 20 35 22](https://s4.ax1x.com/2022/02/16/Hhfc1U.png)

![2022 02 16 20 35 35](https://s4.ax1x.com/2022/02/16/HhffB9.png)

为了保证内存可见性，Java 编译器在生成指令序列的适当位置会插入内存屏障指令来禁止特定类型的处理器重排序。



#### happens-before 简介

从 JDK 5 开始，Java 使用新的 JSR-133 内存模型（除非特别说明，本文针对的都是JSR-133 内存模型）。JSR-133 使用 happens-before 的概念来阐述操作之间的内存可见性。在 JMM 中，如果一个操作执行的结果需要对另一个操作可见，那么这两个操作之间必须要存在 happens-before 关系。



### 重排序

#### 数据依赖性

![2022 02 16 20 39 35](https://s4.ax1x.com/2022/02/16/HhhPgS.png)

编译器和处理器在重排序时，会遵守数据依赖性，编译器和处理器不会改变存在数据依赖关系的两个操作的执行顺序。这
里所说的数据依赖性仅针对单个处理器中执行的指令序列和单个线程中执行的操作，不同处理器之间和不同线程之间的数据依赖性不被编译器和处理器考虑



#### as-if-serial 语义



#### 程序顺序规则



#### 重排序对多线程的影响

在单线程程序中，对存在控制依赖的操作重排序，不会改变执行结果（这也是 as-ifserial 语义允许对存在控制依赖的操作做重排序的原因）；但在多线程程序中，对存在控制依赖的操作重排序，可能会改变程序的执行结果



### 顺序一致性

顺序一致性内存模型是一个理论参考模型，在设计的时候，处理器的内存模型和编程语言的内存模型都会以顺序一致性内存模型作为参照



#### 数据竞争与顺序一致性

#### 顺序一致性内存模型

#### 同步程序的顺序一致性效果

#### 未同步程序的执行特性



### volatile 的内存语义

当声明共享变量为 volatile 后，对这个变量的读/写将会很特别。为了揭开 volatile 的神秘面纱，下面将介绍 volatile 的内存语义及 volatile 内存语义的实现

#### volatile 的特性

![2022 02 16 21 25 46](https://s4.ax1x.com/2022/02/16/Hh78bD.png)



![2022 02 16 21 26 01](https://s4.ax1x.com/2022/02/16/Hh7t5d.png)



对一个 volatile 变量的读，总是能看到（任意线程）对这个 volatile 变量最后的写入。锁的语义决定了临界区代码的执行具有原子性。这意味着，即使是 64 位的 long 型和 double 型变量，只要它是 volatile 变量，对该变量的读/写就具有原子性。如果是多个volatile 操作或类似于 volatile++这种复合操作，这些操作整体上不具有原子性



简而言之，volatile 变量自身具有下列特性。

1.   可见性。对一个 volatile 变量的读，总是能看到（任意线程）对这个 volatile 变量最后的写入。
2.   原子性：对任意单个 volatile 变量的读/写具有原子性，但类似于 volatile++这种复合操作不具有原子性。



#### volatile 写-读建立的 happens-before 关系

#### volatile 写-读的内存语义

![2022 02 16 21 35 56](https://s4.ax1x.com/2022/02/16/HhHT6P.png)

#### volatile 内存语义的实现

X86 处理器仅会对写-读操作做重排序。

![2022 02 16 21 34 53](https://s4.ax1x.com/2022/02/16/HhHsQx.png)



#### JSR-133 为什么要增强 volatile 的内存语义



### 锁的内存语义

众所周知，锁可以让临界区互斥执行。这里将介绍锁的另一个同样重要，但常常被忽视的功能：锁的内存语义



#### 锁的释放-获取建立的 happens-before 关系

锁是 Java 并发编程中最重要的同步机制。锁除了让临界区互斥执行外，还可以让释放锁的线程向获取同一个锁的线程发送消息。



#### 锁的释放和获取的内存语义

当线程释放锁时，JMM 会把该线程对应的本地内存中的共享变量刷新到主内存中

![2022 02 16 23 04 58](https://s4.ax1x.com/2022/02/16/H4eYCj.png)





当线程获取锁时，JMM 会把该线程对应的本地内存置为无效。从而使得被监视器保护的临界区代码必须从主内存中读取共享变量。

![2022 02 16 23 08 28](https://s4.ax1x.com/2022/02/16/H4e5VO.png)



![2022 02 16 23 08 51](https://s4.ax1x.com/2022/02/16/H4ebRA.png)



#### 锁内存语义的实现