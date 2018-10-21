#### 1.Spring诞生的背景
A(nswer)：在诞生之初，创建Spring的主要目的是用来替代更加重量级的企业级Java技术，尤其是EJB（Enterprise JavaBean）。相对于EJB来说，
Spring提供了更加轻量级和简单的编程模型。它增强了简单老式Java对象(Plain Old Java Object，POJO)的功能，使其具备了之前只有EJB和其他企
业级Java规范才具有的功能。

#### 2.Spring的核心特性
A：Spring可以做很多事情，它为企业级开发提供了丰富的功能，但是这些功能的底层都依赖于它的两个核心特性——依赖注入(dependency injection，DI)
和面向切面编程(aspect-oriented programming, AOP)。

#### 3.Spring简介
A：Spring是一个开源框架，最早由Rod Johnson创建，并在《Expert One-On-One:J2EE Design and Development》这本著作中进行了介绍。Spring
是为了解决企业级应用开发的复杂性而创建的，使用Spring可以让简单的JavaBean实现之前只有EJB才能完成的事情。但Spring不仅仅局限于服务器端开发，
任何Java应用都能在简单性、可测试性和松耦合等方面从Spring中获益。

#### 4.Spring的最根本使命或者其基本理念是什么？
A：简化Java开发

#### 5.Spring是如何简化Java开发的？
A：为了降低Java开发的复杂性，Spring采取了以下4种关键策略：
   (1).基于POJO的轻量级和最小侵入性编程；
   (2).通过依赖注入和面向接口实现松耦合；
   (3).基于切面和惯例进行声明式编程；
   (4).通过切面和模板减少样板式代码。

#### 6.与同类型的Java开发框架相比，Spring做了怎样的改变？
A：很多的Java开发框架通过强迫应用继承它们的类或实现它们的接口从而导致应用与框架绑死。Spring竭力避免因自身的API而弄乱你的应用代码。Spring不会
强迫你实现Spring规范的接口或继承Spring规范的类，相反，在基于Spring构建的应用中，它的类通常没有任何痕迹表明你使用了Spring。最坏的场景是，一个
类或许会使用Spring注解，但它依旧是POJO。

#### 7.Spring中，一个简单JavaBean的例子：
  ```
  public class HelloWorldBean{
      public String sayHello(){
          return "Hello World!";
      }
  }
  ```
  这是一个简单普通的Java类——POJO。没有任何地方表明它是一个Spring组件。Spring的非侵入式编程模型意味着这个类在Spring应用和非Spring应用中都可以
  发挥相同的作用。尽管形式看起来很简单，但POJO一样可以具有魔力。Spring赋予POJO魔力的方式之一就是通过DI来装配它们。
  
#### 8.耦合的两面性是指什么？
A：在软件的开发中，一方面，紧密耦合的代码难以测试、难以复用、难以理解，并且典型地表现出“打地鼠”式的bug特性(修复一个bug，将会出现一个或更多新的bug)；另一方面，一定程度的耦合又是必须的————完全没有耦合的代码什么也做不了。为了完成有实际意义的功能，不同的类必须以适当的方式进行交互。总而言之，耦合是必须的，但同时耦合又应当被小心谨慎地管理。

#### 9.什么是DI？
A：通过DI，对象的依赖关系将由系统中负责协调各对象的第三方组件在创建对象的时候进行设定。对象无须自行创建或管理它们的依赖关系，依赖关系将被自动注入到
需要它们的对象当中去。

#### 10.一个改进代码耦合性的例子
  改进之前的代码：
  ```
  public class DamselRescuingKnight implements Knight{
      private RescueDamselQuest quest;
      
      public DamselRescuingKnight(){
          this.quest = new RescueDamselQuest();
      }
      
      public void embarkOnQuest(){
          quest.embark();
      }
  
  }
  ```
  
  可以看到，DamselRescuingKnight 在它的构造函数中自行创建了RescueDamselQuest。这使得DamselRescuingKnight紧密地和RescueDamselQuest耦合到
  一起，因此极大地限制了这个骑士执行探险的能力。如果一位少女需要救援，这个骑士能够召之即来。但是如果一条恶龙需要杀掉，或者一个圆桌……额……需要滚动起
  来，那么这个骑士就爱莫能助了。
  
  注：knight——骑士，武士；damsel——少女，年轻女子；quest——探索，寻找；rescue——救援，营救
  
  改进之后的代码：
  ```
  public class BraveKnight implements Knight{
      private Quest quest;
      
      public BraveKnight(Quest quest){    <----------- Quest被注入进来
          this.quest = quest;
      }
      
      public void embarkOnQuest(){
          quest.embark();
      }
  
  }
  ```
  可以看到，不同于之前的DamselRescuingKnight，BraveKnight没有自己创建探险任务，而是在构造的时候把探险任务作为构造器参数传入。这是
  依赖注入的方式之一，即构造器注入(constructor injection)。
  更重要的是，传入的探险类型是Quest，也就是所有探险任务都必须实现的一个借口。所以，BraveKnight 能够响应RescueDamselQuest、SlayDragonQuest、
  MakeTableRounderQuest等任意的Quest实现。
  这里的要点是BraveKnight没有与任何特定的Quest实现发生耦合(注：这一点也就是设计模式中谈到的，要面向接口编程，不要面向具体实现编程.)。对它来说，
  被要求挑战的探险任务只要实现了Quest接口，那么具体是哪种类型的探险就无关紧要了。这就是DI所带来的最大收益——松耦合。如果一个对象只通过接口(而不是
  具体实现或初始化过程)来表明依赖关系，那么这种依赖就能够在对象本身毫不知情的情况下，用不同的具体实现进行替换。
  
#### 11.DI与AOP能产生怎样的收益？
A：DI 能够让相互协作的软件组织保持松散耦合，而面向切面编程(AOP)允许你把遍布应用各处的功能分离出来形成可重用的组件。

#### 12.AOP 产生的背景
A：面向切面编程往往被定义为促使软件系统实现关注点分离的一项技术。系统由许多不同的组件组成，每一个组件各负责一块特定功能。除了实现自身核心的功能之外，
这些组件还经常承担着额外的职责。诸如日志、事务管理和安全这样的系统服务经常融入到自身具有核心业务逻辑的组件中去，这些系统服务通常被称为横切关注点，因
为它们会跨越系统的多个组件。
  如果将这些关注点分散到多个组件中去，你的代码将会带来双重的复杂性：
  (1).实现系统关注点功能的代码将会重复出现在多个组件中。这意味着如果你要改变这些关注点的逻辑，必须修改各个模块中的相关实现。即使你把这些关注点
   抽象为一个独立的模块，其他模块只是调用它的方法，但方法的调用还是会重复出现在各个模块中。
  (2).组件会因为那些与自身核心业务无关的代码而变得混乱。一个向地址簿增加地址条目的方法应该只关注如何添加地址，而不应该关注它是不是安全的或者是
   否需要支持事务。
 而 AOP 能够使这些组件模块化，并以声明的方式将它们应用到它们需要影响的组件中去。所造成的结果就是这些组件会具有更高的内聚性并且会更加关注自身的业务，
 完全不需要了解涉及系统服务所带来的复杂性。总之，AOP能够确保POJO的简单性。
 
#### 13.一个关于AOP的例子
  应用背景：每一个人都熟知骑士所做的任何事情，这是因为吟游诗人用诗歌记载了骑士的事迹并将其进行传唱。假设我们需要使用吟游诗人这个服务类来记载骑士的
          所有事迹。
          
     ```
     public class Minstrel{
         
         private PrintStream stream；
         
         public Minstrel(PrintStream stream){
             this.stream = stream;
         }
         
         public void singBeforeQuest(){            <----------- 探险之前调用
             stream.println("Fa la la...the knight is so brave!");
         }
         
         public void singAfterQuest(){             <----------- 探险之后调用
             stream.println("Tee hee hee...the brave knight did embark on a quest!");
         }
     }
     ```
     
     正如所看到的那样，Minstrel 是只有两个方法的简单类。在骑士执行每一个探险任务之前，singBeforeQuest() 方法会被调用；在骑士完成探险任务之后，
     singAfterQuest() 方法会被调用。在这两种情况下，Minstrel 都会通过一个 PrintStream 类来歌颂骑士的事迹，这个类是通过构造器注入进来的。
     注：minstrel--吟游诗人，旅行音乐师
     
     ```
     public class BraveKnight implements Knight{
         private Quest quest;
      
         public BraveKnight(Quest quest， Minstrel minstrel){   
             this.quest = quest;
             this.minstrel = minstrel;
         }
      
         public void embarkOnQuest(){
             minstrel.singBeforeQuest();    <----------- Knight应该管理它的Minstrel吗？
             quest.embark();
             minstrel.singAfterQuest();
         }
  
     }
     ```
     要将Minstrel 抽象为一个切面，你所需要做的事情就是在一个Spring 配置文件中声明它。下面的knight.xml文件中，Minstrel被声明为一个切面：
     
     ```
     <? xml version="1.0" encoding="UTF-8"?>
       <beans xmlns="……">
           <bean id="knight" class="com.springApp.knights.BraveKnight">
               <constructor-arg ref="quest"/>
           </bean>
           <bean id="quest" class="com.springApp.knights.SlayDragonQuest">
               <constructor-arg value="#{T (System).out}"/>
           </bean>
           <bean id="minstrel" class="com.springApp.knights.Minstrel">
               <constructor-arg value="#{T (System).out}"/>
           </bean>
           
           <aop:config>
               <aop:aspect ref="minstrel">
                   <aop:pointcut id="embrak" expression="execution(* *.embrakOnQuest(..))"/>  <------ 定义切点
                   <aop:before pointcut-ref="embrak" method="singBeforeQuest"/>  <------ 声明前置通知
                   <aop:after  pointcut-ref="embrak" method="singAfterQuest"/>  <------ 声明后置通知
               </aop:aspect>
           </aop:config>
       </beans>
       ```
#### 14.Spring 通过模板封装来消除样板式代码。

#### 15.Spring 通过面向POJO编程、DI、切面和模板技术来简化Java开发中的复杂性。
     
     
     
     
  
  


















