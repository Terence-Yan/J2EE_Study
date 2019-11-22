##### 1.List和Set的区别。

##### 2.HashSet是如何保证元素的互异性？

##### 3.HashMap是线程安全的吗？请简述理由。

##### 4.简述HashMap的扩容过程。

##### 5.HashMap1.7版本与1.8版本的区别，1.8版本做了哪些优化，如何优化的？

##### 6.简述final、finally与finalize的用法与含义。

##### 7.简述强引用、软引用、弱引用与虚引用的含义。
```
在Java语言中，程序员几乎是“无法”手动的去管理对象的生命周期的。在某些场景下，为了便于对java对象生命周期更“精细化”的管理，
Java语言中引入了这四种引用。
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
引用类型  |     实现类         |   生命周期    
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
强引用    |      无           |  对象存在强引用，JVM不会回收这个对象，即使抛出OOM异常
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
软引用    | SoftReference     |  对象只存在软引用，JVM会在内存不足时回收这个对象
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
弱引用    | WeakReference     |  对象只存在弱引用，JVM进行垃圾回收时就会回收这个对象(不论内存是否充足)
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
虚引用    | PhantomReference  |  对象只存在虚引用，这个对象任何时候都可能被垃圾回收器回收
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
```
###### 参考文章
* <a href="https://mp.weixin.qq.com/s/p3Z-iqDCXCiVbf4r5Y_FCA" target="_blank">Java 如何有效地避免OOM：善于利用软引用和弱引用</a>
* <a href="https://mp.weixin.qq.com/s/fXkkjz7k_vQmMIwx9x1eiA" target="_blank">如何有效的避免OOM，温故Java中的引用</a>
##### 8.简述Arrays.sort的实现原理。

##### 9.简述LinkedHashMap的用法。

##### 10.简述colneable接口的实现原理。

##### 11.简述异常分类以及相应的处理机制。

##### 12.wait和sleep的区别。
* ##wait是Object类的方法，而sleep是Thread的方法。##
* ##wait方法会释放持有的锁资源，而sleep方法不会释放。##
```
wait()方法源码中的注解：
...The thread releases ownership of this monitor...
sleep()方法源码中的注解：
...The thread does not lose ownership of any monitors...
```
* ##wait方法必须与synchronized代码块同时使用，且需要notify方法进行“唤醒”。##
```
...The current thread must own this object's monitor...
     synchronized (obj) {
        obj.wait();
     }
```

##### 13.数组在内存中如何分配。

##### 14.Void 与 void的区别。
```
1.Void是一个不能被继承不能被实例化的类,是void关键字在类层面上的表示；
2.方法返回值声明为Void,必须以null作为返回值：
    private static Void outputWord(){
        System.out.println("Hello World!");
        return null;
    }
3.Void一般常用于反射场景下，获取方法的返回值类型。
4.void是Java语言的一个关键字，用于方法返回值的声明，表示该方法无返回值。在语法上，可以以一个return语句结束方法执行并返回:
    private static void printName(Person p){
        if(null == p){
            return;
        }
        System.out.println(p.getName());
    }
```
