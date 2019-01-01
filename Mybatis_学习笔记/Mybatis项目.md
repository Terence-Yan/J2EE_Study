#### 1.mybatis项目的日志配置
```
a).在mybatis的配置文件的<settings>中添加如下配置：
  <setting name="logImpl" value="LOG4J"/>
b).在类路径下创建log4j.properties文件,并进行相应的配置
c).导入所依赖的日志组件包：
  log4j-1.2.17.jar,
  commons-logging-1.2.jar,
  log4j-core-2.3.jar,
  log4j-api-2.3.jar
  slf4j-api-1.7.21.jar
  slf4j-log4j12-1.7.21.jar
```
