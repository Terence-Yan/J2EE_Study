#### 1.@Autowired
* 自动注入注解：首先它会根据属性类型查找对应的唯一的Bean，如果对应类型的Bean不是唯一的，那么它会根据其属性名称和Bean的名称进行匹配。如果匹配得上，就
会使用该Bean；如果还无法匹配，就会抛出异常。
* @Autowired是一个默认必须找到对应Bean的注解，如果不能确定其标注属性一定会存在并且允许这个被标注的属性为null，那么可以将@Autowired的属性required设
置为false，如`@Autowired(required = false)`。

#### 2.@Primary和@Quelifier——消除歧义性
* @Primary：在注入Bean时，当Spring IoC容器发现有多个同样类型的Bean时，可以使用该注解指定优先使用某一个Bean进行注入。
* @Quelifier：有时候@Primary也可以使用在多个类上，其结果是IoC容器还是无法区分采用哪个Bean的实例进行注入，又或者说我们需要更加灵活的机制来实现注入，
那么@Quelifier注解就是解决这个问题的。它将与@Autowired组合在一起，通过类型和名称一起找到Bean。我们知道Bean名称在Spring IoC容器中是唯一的标识，
通过这个就可以消除歧义性了。如：
```
@Autowired
@Quelifier("manager")   //表示将容器中类型为User且名称为manager的Bean对其进行注入赋值
private User user = null;
```
