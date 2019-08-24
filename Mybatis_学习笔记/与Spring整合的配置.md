#### 1.Mybatis与Spring整合的配置文件
```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:jdbc="http://www.springframework.org/schema/jdbc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mybatis="http://mybatis.org/schema/mybatis-spring"
       xsi:schemaLocation="http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
     http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
     http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc.xsd
     http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
     http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring.xsd">
     ......
    <!-- 事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource" /> // 指定事务管理器管理的数据源
    </bean>

    <!-- enable component scanning and autowire (beware that this does not enable mapper scanning!) -->    
    <context:component-scan base-package="org.mybatis.jpetstore.service" />

    <!-- enable transaction demarcation with annotations -->
    <tx:annotation-driven />

    <!-- 通过 Spring 创建 SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" /> // 指定数据源
        // 指定mybatis全局配置文件的位置
        <property name="configLocation" value="classpath:mybatis-config.xml" /> 
        // 指定SQL映射文件的位置
        <property name="mapperLocations" value="classpath:mybatis/mapper/*.xml" /> 
    </bean>

    <!-- scan for mappers and let them be autowired -->
    <!-- 扫描所有mapper接口的实现，让其可以自动注入 -->
    // base-package:指定mapper接口所在的包名
    <mybatis:scan base-package="org.mybatis.jpetstore.mapper" />
    <!-- 或者
    <bean id="transactionManager" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="org.mybatis.jpetstore.mapper" /> 
    </bean>
    -->
</beans>
```
