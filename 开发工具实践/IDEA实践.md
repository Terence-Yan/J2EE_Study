#### 1.IDEA中无法创建java类
```
操作系统：centos7 64位
IDEA版本：2016
Java版本：1.8
Maven版本：3.2.3
在IDEA中创建了java的maven工程，在创建java类的时候，遇到了如下的错误提示信息：
...Unable to parse template "Class" Error message: This template did not produce a Java class or an interface...
通过搜索找到了解决办法：
(1).修改安装目录下的bin文件夹下idea.vmoptions文件，在最后添加如下的一行参数：-Djdk.util.zip.ensureTrailingSlash=false；
(2).重启IDEA即可。
注意：若是64位的机器，则需要在bin目录下再拷贝一份该文件，并重命名为idea64.vmoptions。(修改以后进行该操作，该文件也可以通过点击IDEA中
Help --> Edit Custom VM Options编辑生成。)
```
