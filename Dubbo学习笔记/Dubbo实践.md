#### 1.本地使用 maven 编译安装dubbo源码报错
```
报错信息：
1.Failed to execute goal on project dubbo-serialization-protobuf: Could not resolve dependencies for project 
org.apache.dubbo:dubbo-serialization-protobuf:jar:2.7.7: Failed to collect dependencies at org.apache.dubbo:
dubbo-serialization-api:jar:2.7.7: Failed to read artifact descriptor for org.apache.dubbo:dubbo-serialization-api:
jar:2.7.7: Could not find artifact org.apache.dubbo:dubbo-serialization:pom:${revision} in central 
(https://repo.maven.apache.org/maven2) -> [Help 1]
2....Failed to execute goal org.xolstice.maven.plugins:protobuf-maven-plugin:0.5.1:compile (default) on project 
message: protoc did not exit cleanly...
问题原因：
编译的源码所在本地路径中包含中文字符
解决方案：
修改本地路径，去掉中文字符，重新执行即可
```
参考文章：https://www.cnblogs.com/Unicron/p/13068259.html
