#### 1.通过maven构建springboot项目的“根包”
```
通过maven构建springboot项目，会产生项目的pom文件，如：
"""
......
   <parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.19.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>springboot</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>app1</name>
	<description>Demo project for Spring Boot</description>
......
"""
pom文件中项目所对应的<groupId>与<artifactId>联合构成了项目的“根”包，即只能在该包的目录下创建子包或java类文件，或者说项目的所有java代码
只能位于该包的下面，不在该包下面的代码是无法访问的。如，创建"com.example.controller"包，此时虽然文件夹"controller"与"springboot"位于
同一目录下，但是"controller"下的java代码确是无法访问的。
```
