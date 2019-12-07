#### Java内存模型(JMM)中与并发处理相关的三个特征：可见性，原子性，有序性(指令重排序问题)
#### 线程安全(并发)与性能是一对矛盾的存在。
##### 1.简述synchronized的实现原理。

##### 2.简述volatile的实现原理。
```
关键字volatile是JVM提供的最轻量级的同步机制，但是它只能保证可见性与有序性，不能保证原子性。
```
###### 参看文章
* <a href="https://mp.weixin.qq.com/s/nzJdqJkXl1Dj4-JNzJHGEQ" target="_blank">既生synchronized，何生volatile？！</a>
##### 3.谈谈对Java的Semaphore(信号灯、信号量)的理解。
* Semaphore是一个用来控制访问(资源)的线程的数量的协调器。

##### 4.synchronized修饰静态方法与普通方法的区别。
```
synchronized修饰静态方法时，持有的是使用当前类对象作为资源的锁(如 A.class);
synchronized修饰普通方法时，持有的是this锁。
```

##### 5.如何实现所有线程在等待某个事件发生后才会去执行？

##### 6.CAS的含义，CAS有什么缺陷，如何解决？
```
CAS全称 Compare And Swap（比较与交换），是一种无锁算法。在不使用锁（没有线程被阻塞）的情况下实现多线程之间的变量同步。
java.util.concurrent包中的原子类就是通过CAS来实现了乐观锁。
CAS算法涉及到三个操作数：
   --需要读写的内存值 V
   --进行比较的值 A
   --要写入的新值 B
```

##### 7.synchronized和lock的区别？

##### 8.Hashtable是如何加锁的？

##### 9.HashMap的并发问题。

##### 10.简述ConcurrentHashMap，1.8版本中为什么采用红黑树实现？

##### 11.AQS

##### 12.如何检测死锁？怎么预防死锁？

##### 13.简述Java内存模型。

##### 14.如何保证多线程下i++结果正确？

##### 15.线程池的种类，区别和使用场景。

##### 16.简述线程池的实现原理和线程的调度过程。

##### 17.线程池如何调优，最大数目如何确认？

##### 18.ThreadLocal的实现原理，使用时有哪些需要注意的问题？

##### 19.CountDownLacth 和 CyclicBarrier的用法，以及相互之间的差别？
* CountDownLacth是一个同步(线程安全的)状态指示器，采用计数器的方式实现，是以“观察者模式”在线程之间进行状态信息的通信.它的含义是：
```
源码注释：
A synchronization aid that allows one or more threads to wait until a set of operations 
being performed in other threads completes.
--------------------------------------------------------------------------------------------------------------
它可以作为一个多线程任务下的“状态标识符”来使用，主要的应用场景有以下两种模式：
1.假如有事务T需要分为A,B,C三个有序步骤来完成，而步骤B又是一个执行相同子任务的批量操作，此时可以通过使用CountDownLacth
对象(countDown方法)，让步骤B采用并发进行处理，而执行步骤C的线程通过调用await方法就可以保证只有在步骤B的所有并发任务执
行完之后才会执行。
2.假如有事务T可以分为 N 个子任务来完成，这 N 个子任务又可以分为有序的三段来执行--“1,(2,3,...,N-1),N”，其中中间的
多个子任务没有顺序要求，此时可以使用CountDownLacth对象，对“(2,3,...,N-1),N”这些子任务采用并发处理(前面的子任务使用
countDown方法，最后一个使用await方法即可)。
```
* CountDownLacth 与 CyclicBarrier的作用是一样的，都是一种用来保持多线程任务完成状态一致性检验的协调器。不同的是：
```
1.CountDownLacth对象只能使用一次，CyclicBarrier可以循环重复使用；
2.CountDownLacth的结果需要额外的线程来判断，而CyclicBarrier不需要，它是通过多线程之间相互“通信”来进行
完成状态的确认与协调的。
```

##### 20.LockSupport工具

##### 21.Condition接口及其实现原理。
```
Condition是一个集合了线程间通信的锁方法的接口。其中的方法(await、signal...)在效果上等同于Object类
中对应的相应方法(wait、notify...)。
简单地讲，synchronized + Object.wait() 等价于 Lock + Condition。
参见源码中注释：
 * {@code Condition} factors out the {@code Object} monitor
 * methods ({@link Object#wait() wait}, {@link Object#notify notify}
 * and {@link Object#notifyAll notifyAll}) into distinct objects to
 * give the effect of having multiple wait-sets per object, by
 * combining them with the use of arbitrary {@link Lock} implementations.
 * Where a {@code Lock} replaces the use of {@code synchronized} methods
 * and statements, a {@code Condition} replaces the use of the Object
 * monitor methods.
```

##### 22.简述你对Fork/Join框架的理解。

##### 23.分段锁的原理，谈谈对锁力度减小的认识。

##### 24.八种阻塞队列以及各个阻塞队列的特性。

##### 25.说说你对Exchanger的理解。
```
Exchanger是一个同步的数据交换器，用于两个线程之间进行数据的交换，交换的是“原始”数据，而不是数据的拷贝。
交换数据的操作可以重复进行。
```

##### 26.说说你对ReentrantReadWriteLock的理解。
```
ReentrantReadWriteLock是一个可重入的读写分离锁，它的含义是：
假如有A，B两个线程同时对一个资源进行读写操作，则
________________________________________________________
      线程A      |     线程B       |  是否可以并发执行
————————————————————————————————————————————————————————
       写        |       写        |       否
————————————————————————————————————————————————————————
       写        |       读        |       否
————————————————————————————————————————————————————————
       读        |       写        |       否
————————————————————————————————————————————————————————
       读        |       读        |       是
————————————————————————————————————————————————————————
```

##### 27.谈谈你对AtomicReference与AtomicStampedReference的理解。
```
AtomicStampedReference是一个带有“时间戳”标记的原子引用类，可以避免CAS操作产生的ABA问题。
```

##### 28.谈谈你对StampedLock的理解。
###### 参看文章
* <a href="http://ifeve.com/java-8-stampedlocks-vs-readwritelocks-and-synchronized/" target="_blank">Java 8：StampedLock、ReadWriteLock以及synchronized的比较</a>

##### 29.Java中锁的分类有哪些？
* 根据线程是否要锁住同步资源，可分为：悲观锁(锁住)与乐观锁(不锁住)。
* 根据锁住同步资源失败时，线程是否要阻塞，可分为：阻塞与不阻塞(自旋锁、适应性自旋锁)。
* 根据多个线程竞争同步资源的流程细节的不同，可分为：
```
无锁：不锁住资源，多个线程中只有一个能修改资源成功，其他线程会重试；
偏向锁：同一个线程执行同步资源时自动获取资源；
轻量级锁：多个线程竞争同步资源时，没有获取到资源的线程自旋等待锁释放；
重量级锁：多个线程竞争同步资源时，没有获取到资源的线程阻塞等待唤醒。
```
* 根据多个线程竞争锁时是否要排队，可分为：公平锁(排队)与非公平锁(先尝试插队，插队失败再排队)。
* 根据一个线程中的多个流程是否能获取同一把锁，可分为：可重入锁(能)与非可重入锁(不能)。
* 根据多个线程是否能共享同一把锁，可分为：共享锁(能)与排他锁(不能)。

###### 参看文章
* <a href="https://mp.weixin.qq.com/s/E2fOUHOabm10k_EVugX08g" target="_blank">不可不说的Java“锁”事</a>
