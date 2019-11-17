##### 1.List和Set的区别。

##### 2.HashSet是如何保证元素的互异性？

##### 3.HashMap是线程安全的吗？请简述理由。

##### 4.简述HashMap的扩容过程。

##### 5.HashMap1.7版本与1.8版本的区别，1.8版本做了哪些优化，如何优化的？

##### 6.简述final、finally与finalize的用法与含义。

##### 7.简述强引用、软引用、弱引用与虚引用的含义。

###### 参考文章
* <a href="https://mp.weixin.qq.com/s/p3Z-iqDCXCiVbf4r5Y_FCA" target="_blank">引用</a>
##### 8.简述Arrays.sort的实现原理。

##### 9.简述LinkedHashMap的用法。

##### 10.简述colneable接口的实现原理。

##### 11.简述异常分类以及相应的处理机制。

##### 12.wait和sleep的区别。

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
