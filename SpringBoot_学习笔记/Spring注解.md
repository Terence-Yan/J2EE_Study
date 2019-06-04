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

#### 3.@Conditional——条件装配Bean
```
问题提出：有时候某些因素会导致一些Bean无法进行初始化，例如，在数据库连接池的配置中漏掉一些配置会造成数据源不能连接上。在这种情况下，IoC容器如果
还进行数据源的装配，则系统将会抛出异常，导致应用无法继续。这是倒是希望IoC容器不去装配数据源。
解决方案：为了处理这样的场景，Spring提供了@Conditional注解，配合另外一个接口Condition就可以完成相应的功能了。
例：
@Bean(name="dataSource")
@Conditional(DatabaseConditional.class)
public DataSource getDataSource(...){...}
上面的代码中，加入了@Conditional注解，并且配置了DatabaseConditional类，那么这个类就必须实现Condition接口。对于Condition接口则要实现matches方法。
如果matches方法返回true，则IoC容器就会装配数据库相关的这个Bean，否则就不装配。
```

#### 4.@Scope——Bean的作用域
```
在IoC容器最顶级接口BeanFactory中，包括isSingleton 和 isPrototype两个方法。其中，isSingleton方法如果返回true，则Bean在IoC容器中以单例存在，这也是
Spring IoC容器的默认值；如果isPrototype方法返回true，则当我们每次获取Bean的时候，IoC容器都会创建一个新的Bean，这显然存在很大的不同，这便是Spring 
Bean的作用域问题。在一般的容器中，Bean都会存在单例(Singleton)和原型(Prototype)两种作用域。JavaEE广泛地使用在互联网中，而在Web容器中，则存在
页面(page)、请求(request)、会话(session)和应用(application)4种作用域。对于页面(page)，是针对JSP当前页面的作用域，所以Spring是无法支持的。为了
满足各类的作用域，在Spring的作用域中就存在如下的几种类型：
    作用域类型              使用范围                               作用域描述
    singleton             所有Spring应用
    
```






















