#### 1.事务传播行为的含义
```
(1).事务传播问题产生的背景：对于一个(数据库)批处理任务，它要处理多个任务，如果在一些任务中发生了异常，这个时候采用什么回滚策略
    处理所有的任务，即当有任务发生异常时，是立即停止批处理任务，回滚所有的任务处理操作，从而结束批处理任务，还是回滚当前发生异
    常的任务处理操作，然后继续下一个任务处理。
(2).事务的传播行为是指当一个方法调用另一个方法时，采用配置的方式控制事务特性的传播策略的行为。
```

#### 2.事务传播行为的7种策略
```
(1).REQUIRED: 当方法调用时，如果不存在当前事务，那么就创建事务；如果之前的方法已经存在事务了，那么就沿用之前的事务。
              这是Spring默认的传播行为。
(2).REQUIRES_NEW: 无论是否存在当前事务，方法都会在新的事务中运行，也就是说事务管理器会打开新的事务运行该方法。该方法
                  每次被调用都会开启一个新事务。
(3).NESTED: 嵌套事务，也就是该被调用方法如果发生异常只回滚自己执行的SQL，而不回滚调用方法的SQL。它的实现存在两种情况，
            如果当前数据库支持保存点(savepoint)，那么它就会在当前事务上使用保存点技术：如果发生异常则将方法内执行的
            SQL回滚到保存点上，而不是全部回滚，否则就等同于 REQUIRES_NEW 创建新的事务运行方法。亦即当数据库支持保存点
            技术时，它是在一个事务中进行批处理任务的，只是通过
(4).SUPPORTS: 当方法调用时，如果不存在当前事务，那么就不启用事务；如果存在当前事务，那么就沿用当前事务。
(5).MANDATORY: 方法必须在事务内运行。如果不存在当前事务，那么就抛出异常。
(6).NOT_SUPPORTED: 不支持事务，如果不存在当前事务也不会创建事务；如果存在当前事务，则挂起它，直至该方法结束后才恢复当前事务。
(7).NEVER: 不支持事务，只有在不存在事务的环境才能运行；如果方法存在当前事务，则抛出异常。
(8).常用的传播策略：REQUIRED 与 REQUIRES_NEW。
```

#### 3.NESTED 与 REQUIRED、REQUIRES_NEW 的异同
```
(1).当数据库支持保存点技术时，NESTED 与 REQUIRED 策略相同，都是在一个事务中进行批处理任务的，不同的是它可以通过保存点技术回滚
    发生异常的单个任务；
(2).当数据库不支持保存点技术时，NESTED 与 REQUIRES_NEW 策略相同。   
```

#### 4.REQUIRED、REQUIRES_NEW 的含义
```
举例说明：
(1). public class RoleListImpl {
        
        ...
        @Autowired
        private RoleService roleService;
        ...
        
        @Transactional(propagation = Propagation.REQUIRED)
        public int insertRoleList(List<Role> roles){
            int count == 0;
            for(Role role : roles){
                try{
                    count += roleService.insertRole(role);
                } catch (Exception e) {
                    log.error(e);
                }
            }
            return count;
        }
        ...
     }
(2). public class RoleServiceImpl implements RoleService {
        
        ...
        @Autowired
        private RoleMapper roleMapper;
        ...
        
        @Transactional(propagation = Propagation.REQUIRES_NEW)
        @Override
        public int insertRole(Role role) {
            return roleMapper.insertRole(role);
        }
        ...
     }
(3). 由于 insertRoleList 会调用 insertRole，而 insertRole 标注了 REQUIRES_NEW，所以每次调用都会产生新的事务：
         insertRoleList开启事务 -> insertRole事务1(提交或回滚) -> insertRole事务2(提交或回滚) -> ... 
         -> insertRoleList提交(回滚)事务。
     数据库中的结果：insert成功的事务被提交，insert异常的事务被回滚，只有insert成功了的数据被持久化到了数据库。亦即
     在一个数据库批处理任务中，不是所有的数据都被持久化到了数据库。
(4). 由于 insertRoleList 会调用 insertRole，而如果 insertRole 标注了 REQUIRED，则每次调用都不会产生新的事务，而是
     沿用 insertRoleList 的事务：
         insertRoleList开启事务 -> insertRole任务1加入当前事务 -> insertRole任务2加入当前事务 -> ... 
         -> insertRole任务n加入当前事务(异常则立即回滚当前事务) -> insertRoleList提交(回滚)事务。
     数据库中的结果：insert的数据要么全部持久化到了数据库，要么数据库没有任何新增数据。即insert操作要么全部成功，要么全部失败。
```

#### 5.@Transactional的自调用失效问题
```
有时候被调用方法配置了@Transactional注解，却没有生效。造成失效的主要原因是：注解@Transactional的底层实现是 Spring AOP 技术，
而Spring AOP 技术使用的是动态代理。这就意味着对于静态(static)方法和非public方法，注解@Transactional是无效的。还有一种情形
更为隐秘，而且在使用的过程中也极容易出现——自调用。所谓自调用，就是一个对象的一个方法去调用自身的另一个方法的行为。如：
     public class RoleListImpl {
        
        ...
        @Autowired
        private RoleMapper roleMapper;
        ...
        
        @Transactional(propagation = Propagation.REQUIRED)
        public int insertRoleList(List<Role> roles){
            int count == 0;
            for(Role role : roles){
                try{
                    // 调用自身的方法，产生自调用问题，insertRole的@Transactional注解失效
                    count += insertRole(role);
                } catch (Exception e) {
                    log.error(e);
                }
            }
            return count;
        }
        
        @Transactional(propagation = Propagation.REQUIRES_NEW)
        public int insertRole(Role role) {
            return roleMapper.insertRole(role);
        }
        ...
     }
```





















