#### 1.Nginx编译报错:系统缺少PCRE库
```
./configure: error: the HTTP rewrite module requires the PCRE library.
You can either disable the module by using --without-http_rewrite_module
option, or install the PCRE library into the system, or build the PCRE library
statically from the source with nginx by using --with-pcre=<path> option.
```

#### 2.安装PCRE库
```
1.下载PCRE安装包：https://sourceforge.net/projects/pcre/files/
    wget https://sourceforge.net/projects/pcre/files/pcre/8.42/pcre-8.42.tar.gz/download
2.解压：tar -zxvf download
3.给解压后的文件夹授予读写权限:chmod -R 777 ./pcre-8.42
4.切换到/pcre-8.42目录下，运行 ./configure 进行pcre初始化配置: ./configure
5.执行make操作，进行编译: make
6.安装PCRE: make install
7.验证安装是否成功：./pcretest
  出现如下界面，表示安装成功：
      PCRE version 8.42 2018-03-20
        re>   
```

#### 3.安装zlib库
```
1.下载PCRE安装包：http://www.zlib.net/
    wget http://prdownloads.sourceforge.net/libpng/zlib-1.2.11.tar.gz?download
2.解压：tar -zxvf zlib-1.2.11.tar.gz\?download
3.进入解压后目录，执行：./configure
   注：./configure --prefix=/usr/local/3rdlib/zlib-1.2.11/data/zlib-1.2.11(此处尽量不要使用指定安装目录的方式安装)
4.编译：make
5.安装zlib: make install
6.添加到系统配置
　　　创建文件，命令：vim /etc/ld.so.conf.d/zlib.conf
　　　填入内容（为zlib的解压目录）：/usr/local/3rdlib/zlib-1.2.11
7.加载配置，执行命令：ldconfig
注：a).在第3步尽量不要使用指定安装目录的方式运行配置命令，否则后面安装Nginx时可能会报如下的错误：
    ----------------------------------------------------------
    fatal error: zlib.h: No such file or directory
    fatal error: zconf.h: No such file or directory
    No rule to make target `distclean'
    ----------------------------------------------------------
    b).在安装Nginx时，执行到运行 configure 命令结束，若在最后看到如下输出信息，表示zlib默认安装是正确的：
    ----------------------------------------------------------
    ......
    Configuration summary
       + using system PCRE library
       + OpenSSL library is not used
       + using system zlib library      (此行信息表明zlib默认安装是OK的)
    ----------------------------------------------------------
```

