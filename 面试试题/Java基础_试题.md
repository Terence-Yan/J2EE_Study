##### 1.List和Set的区别。
* List中的元素有序且可重复，常用的实现类有：
```
ArrayList: 内部是通过一个数组实现的；
LinkedList：内部是通过一个链表实现的。
```
* Set中的元素无序且不可重复，常见的实现类有：
```
HashSet: 内部是通过一个HashMap对象实现的；
TreeSet：内部是通过一个SortedMap(Map + Comparator)对象实现的，该集合中元素的顺序是通过比较器定义的.
```

##### 2.HashSet是如何保证元素的互异性？
HashSet的内部使用的是一个HashMap实例存放元素的，元素被存放在HashMap的键中，所以它是通过HashMap键值的唯一特性来保证元素的互异性的。
```
HashSet类的部分源码：
...
private transient HashMap<E,Object> map;
...
public boolean add(E e) {
    return map.put(e, PRESENT)==null;
}    
```

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
```
--Throwable
     --Exception
         --RuntimeException(unchecked exceptions，非检查异常，编码时非必需处理(即不用显式catch或throw)的异常)
     --Error
```

##### 12.wait和sleep的区别。
* **wait是Object类的方法，而sleep是Thread类的方法。**
* **wait与sleep方法调用后，都会暂停当前线程并让出CPU的执行权，但wait方法会释放持有的锁资源，而sleep方法不会释放。**
```
wait()方法源码中的注解：
...The thread releases ownership of this monitor...
sleep()方法源码中的注解：
...The thread does not lose ownership of any monitors...
```
* **wait方法必须在synchronized同步代码块或同步方法中使用(即必须持有锁)，且需要使用notify方法进行“唤醒”，而sleep在达到“休眠时间”后会继续执行。**
```
...The current thread must own this object's monitor...
     synchronized (obj) {
        obj.wait();
     }
```
###### 参考文章
* <a href="https://mp.weixin.qq.com/s/gvaksKy2ss90bsybCnajpQ" target="_blank">sleep( )和 wait( )的这5个区别，你知道几个？</a>

##### 13.数组在内存中如何分配。
* 数组在内存中会被分配一块连续的内存空间。

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

##### 15.说说你对static的理解。

##### 16.简述集合框架类的层次关系。
```
--Collection
     --List(ArrayList、LinkedList、Vector)
     --Set(HashSet)
        --SortedSet(TreeSet)
     --Queue
        --PriorityQueue
       
--Map
     --AbstractMap(HashMap、LinkedHashMap)
     --SortedMap(TreeMap)
     --Hashtable
```

##### 17.谈谈hashcode()与equals()的关系。
```
1.hashcode()是用来计算给定参数的hash值，主要用于hash表中数组索引的计算;
2.equals()是用来比较两个引用变量(对象)的值是否相等。
3.本来这两个方法之间是没有多少关系的，将它们联系在一起的是采用数组+链表(或红黑树等)作为元素存储数据结构的HashMap一类的集合类。
其特点是作为一级存储结构的数组的分配方式(即如何获取数组的索引)是采用hash函数进行的。如果一个自定义的数据对象，有被用作HashMap
类的key值可能，则它必须重写hashcode()方法，否则在HashMap类型的集合存取元素时可能会出现与预期不一致的现象，而又因为在数组的同
一个位置上可能不止存放了一个元素(当元素多余一个时，会采用链表等数据结构作为二级存储结构存放元素，这是我们也称这个二级存储单元为桶)，
此时要在桶中查找指定元素，就需要使用equals方法进行逐一比较了。因此，当一个自定义的类的实例有作为HashMap类型集合的key可能时，
在定义此类时，就必须同时重写hashcode()与equals()。
4.(1).hash值相同的两个对象，不一定相等；(2).相同的两个对象，其hash值一定相等。
```
###### 参考文章
* <a href="https://mp.weixin.qq.com/s/dnyEc6-WWOm_Ijg7MfSmWw" target="_blank">基础：为什么要重写 hashcode 和 equals 方法？</a>

##### 18.谈谈NoSuchMethodError的常见原因及解决方法。
###### 参考文章
* <a href="https://mp.weixin.qq.com/s/IMfWFxAyWwOxvU5MUYPTtA" target="_blank">NoSuchMethodError 常见原因及解决方法</a>

##### 19.关键字finally相关问题。
```
1.finally块中的代码一定会执行吗？
答：不一定，只有当执行流进入其对应的try块时，finally块中的代码才一定会执行。如：
    public static void main(String[] args) {
        try{
            System.out.println("第一个try块...");
            System.out.println("" + (1 / 0));
        } catch (NullPointerException e) {
            System.out.println("第一个try块对应的catch块...");
        }

        try{
            System.out.println("第二个try块...");
        } finally {
            System.out.println("finally块执行了...");
        }
    }
的执行结果是：
“第一个try块...
Exception in thread "main" java.lang.ArithmeticException: / by zero
	at com.sm.test.FinallyTest.main(FinallyTest.java:7)”
    
2.对try块中包含return语句的finally块，finally中的代码是在return语句之前还是之后执行？
答：在return语句执行之后，该方法返回之前执行。
如：
   例1：
   public static void main(String[] args) {
        System.out.println(test());
    }
   private static String test(){
        String result = "初始值";
        try{
            System.out.println("第一个try块...");
            result = "return返回的值";
            return result;
        } finally {
            System.out.println("finally块执行了...");
            result = "finally返回的值";
            System.out.println("返回值在finally块中被修改了...");
        }
    }
上面代码的执行结果是：
“第一个try块...
finally块执行了...
返回值在finally块中被修改了...
return返回的值”

   例2：
   public static void main(String[] args) {
        System.out.println(test());
    }
   private static Person test(){
        Person p = null;
        try{
            System.out.println("第一个try块...");
            p = new Person("Tom", 20);
            return p;
        } finally {
            System.out.println("finally块执行了...");
            p.setAge(25);
            System.out.println("返回值在finally块中被修改了...");
        }
    }......

class Person{
    private String name;
    private int age;
    public Person(String name, int age){
        this.name = name;
        this.age = age;
    }
    // 省略了setter与getter方法
    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }  
}
上面代码的执行结果是：
“第一个try块...
finally块执行了...
返回值在finally块中被修改了...
Person{name='Tom', age=25}”

通过上面两个例子的分析可以得出：
1.一个方法的栈中有多个return语句时，只会执行第一个(这个也就解释了“为什么try块与finally块中都
含有return语句时，try块中的返回值会被覆盖”);
2.对于返回值是引用类型的，返回的是该引用的地址.
3.return操作不是一个原子操作。
```

##### 20.简单介绍一下Arrays、Collections和Objects类。
```
Arrays:数组的工具类，其公共的静态工具方法的参数都是Java中的数组。
Collections：集合的工具类，其公共的静态工具方法的参数都是List、Set和Collection类型的。
Objects：Object类型的工具类，其公共的静态工具方法的参数都是Object类型，主要包括一些公共的顶级方法，如：
         equals：比较两个对象是否相等，封装了判空处理，以及比较效率的提升——对于两个对象直接先比较地址；
	 hashCode：获取给定对象的hash值，封装了判空处理；
	 toString：返回给定对象的字符串表达式，并且可以设置null时的默认值。
```






