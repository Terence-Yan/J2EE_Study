#### 1.Nginx适用于哪些场景？
* 静态资源服务
```
通过本地文件系统提供服务
```
* 反向代理服务
```
* Nginx的强大性能
* 缓存
* 负载均衡
```
* API服务
```
OpenResty
```
#### 2.Nginx出现的历史背景
* 互联网的数据量快速增长
```
互联网的快速普及、全球化、物联网
```
* 摩尔定律：性能提升
```
软件如何利用多核CPU
```
* 低效的Apache
```
一个连接对应一个进程
```
#### 3.为什么使用Nginx？
* 高并发、高性能
* 可扩展性好
* 高可靠性
* 热部署
* BSD许可证
```
Nginx虽然是开源免费的，但是对其进行定制化开发后，可将其使用于商业场景下。
```
#### 4.Nginx的主要组成部分
* Nginx的二进制可执行文件
```
由各模块源码编译出的一个文件
```
* Nginx.conf配置文件
```
控制Nginx的行为
```
* access.log访问日志
```
记录每一个http请求信息
```
* error.log错误日志
```
定位问题
```
#### 5.Nginx的主要版本
* 开源免费的Nginx(nginx.org)
* 商业版的Nginx Plus(nginx.com)
* 阿里巴巴的Tengine
* 开源免费的OpenResty(openresty.org)
* 商业版的OpenResty(openresty.com)
#### 6.编译属于自己的Nginx
* 下载Nginx
```
Mainline version、Stable version(稳定版本)
```
* 目录简介
```
auto、conf(示例文件)、configure脚本文件、contrib(在vim中增加nginx的配置语法功能的脚本文件)、html、man(帮助文件)、src
```
* configure
* 中间文件介绍
* 编译Nginx
```
1.执行命令：./configure --prefix={nginx的安装目录}
2.执行编译：make
```
* 安装Nginx
```
执行安装(第一次)：make install
```
#### 7.Nginx配置文件的通用语法
```
1.配置文件由指令与指令块构成；
2.每条指令以分号(;)结尾，指令与参数间以空格符号分割；
3.指令块通过大括号({})将多条指令组织在一起；
4.include语句允许组合多个配置文件以提升可维护性；
5.使用 # 添加注释，提高可读性；
6.通过 $ 获取并使用变量；
7.部分指令的参数支持正则表达式。
8.配置参数的时间单位：ms(毫秒)、s(秒)、m(分钟)、h(小时)、d(天)、w(周)、M(月,30天)、y(年,365天)
9.配置参数的空间单位：不加(默认为bytes，字节)、k/K(千字节)、m/M(兆字节)、g/G(吉字节)
```












