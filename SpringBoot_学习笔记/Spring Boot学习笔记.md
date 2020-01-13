#### 1.Spring Boot的主导思想——约定优于配置(自动配置)、注解驱动开发、模块化(各种starter)

#### 2.Spring Boot的自定义配置文件
* src/main/resources/application.properties
* 配置文件有两种格式：properties文件与yml文件

#### 3.Spring Boot的配置项参考文档
* <a href="https://docs.spring.io/spring-boot/docs/1.5.21.RELEASE/reference/htmlsingle/#common-application-properties" target="_blank">配置项参考文档</a>

#### 4.视图解析器(ViewResolver)
* Spring MVC的视图解析器的作用主要是定位视图，也就是当控制器只是返回一个逻辑名称的时候，是没有办法直接找到对应视图的，这就需要视图解析器进行解析了。
* 视图前后缀配置：spring.mvc.view.prefix 与 spring.mvc.view.suffix 是我们与Spring Boot约定的视图前缀与后缀，含义是找到文件夹`/WEB-INF/jsp/`下
以`.jsp`为后缀的JSP文件，那么前缀和后缀之间显然缺了一个文件名称，在Spring MVC机制中，这个名称是由控制器给出的。
```
如：
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp
```

#### 5.Spring Boot的优点
* 快速创建独立运行的spring项目以及与主流框架集成
* 使用嵌入式的Servlet容器，应用无须打成WAR包
* starters(启动器)自动依赖与版本控制
* 大量的自动配置，简化开发，也可修改默认值
* 无须配置XML，无须代码生成，开箱即用
* 准生产环境的运行时应用监控
* 与云计算的天然集成

#### 6.什么是微服务

###### 参考文章
* <a href="https://martinfowler.com/microservices/" target="_blank">Microservices Guide</a>

#### 7.SpringBoot项目的构建与部署插件
```
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```
* <a href="https://docs.spring.io/spring-boot/docs/2.2.2.RELEASE/reference/html/howto.html#howto-build" target="_blank">部署插件参考文档</a>
