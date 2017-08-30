---
layout: post
title: Linux下编译安装nginx到指定目录
date: 2017-08-23
tag: nginx
---


## Linux下编译安装nginx到指定目录

---
因工作需要在服务器搭建nginx，特此博客记录搭建过程。

---

nginx所需的依赖有

- gcc && g++
- pcre
- zlib
- openssl


### 1.下载编译pcre

去官网下载相关的.tar.gz包
http://www.pcre.[org](http://www.pcre.org)。

放到相关目录（我放在/software/source/pcre/pcre-8.41）
![img](http://img.blog.csdn.net/20170823141348360?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbDU1NDcyNTcyMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
解压文件,并进入解压后的文件目录。


```
tar -zxvf pcre-8.41.tar.gz
cd pcre-8.41/
```

编译到指定目录(我指定的目录是/software/execRes/pcre/pcre-8.41)


```
./configure --prefix=/software/execRes/pcre/pcre-8.41
```

之后就是make && make install



```
make
make install
```


### 2.下载编译zlib

步骤基本同上，第一步就是去官网下载[http://www.zlib.net](http://www.zlib.net)

目录我就不列出来了，参考上面。解压并进入相关目录


```
tar -zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11/
```

编译到指定目录


```
./configure --prefix=/software/execRes/zlib/zlib-1.2.11
```

安装

```
make
make install
```

### 3.下载编译openssl

所有过程同上，不在赘述。


### 4.安装nginx

终于到了重点nginx了，其实nginx的编译安装与前面的方法基本一样，不同的是编译的时候需要配置上nginx的依赖。


去官网下载nginx的包[http://nginx.org](http://note.youdao.com/)并放到指定目录,我的目录在/software/source/nginx/nginx-1.13.1


![img](http://img.blog.csdn.net/20170823143513547?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbDU1NDcyNTcyMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

解压并进入相关目录


```
tar -zxvf nginx-1.13.1.tar.gz
cd nginx-1.13.1/
```
编译
接下来就比较关键了，注意所配置的依赖是源码包所在的目录，并不是编译后安装的位置，

---
例：--with-zlib=/software/source/zlib/zlib-1.2.11

---
通用的配置是--with-***="依赖位置"

所有的编译名字如下，我是指定安装到/software/execRes/nginx/nginx-1.13.1目录


```
./configure --prefix=/software/execRes/nginx/nginx-1.13.1 --with-pcre=/software/source/pcre/pcre-8.41 --with-zlib=/software/source/zlib/zlib-1.2.11 --with-openssl=/software/execRes/openssl/openssl-1.0.2h
```
![img](http://img.blog.csdn.net/20170823144113675?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbDU1NDcyNTcyMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
编译通过，之后安装即可


```
make
make install
```

### 4.启动nginx

进入安装完后的nginx目录


```
cd /software/execRes/nginx/nginx-1.13.1/
```

目录结构如下

![img](http://img.blog.csdn.net/20170823144832421?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbDU1NDcyNTcyMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

其中
- conf是配置目录
- html是项目目录
- logs是日志目录
- sbin是启动目录

接下来启动nginx


```
cd sbin
./nginx
```
![img](http://img.blog.csdn.net/20170823145207757?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbDU1NDcyNTcyMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

如果端口被占用，（比如我的80端口被其他服务器使用，只需修改配置文件端口号即可)

进入conf目录，并修改nginx.conf文件


```
cd conf
vi nginx.conf
```
![img](http://img.blog.csdn.net/20170823145355815?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbDU1NDcyNTcyMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

修改此处80为你想要的端口号即可。之后重启nginx

```
 /software/execRes/nginx/nginx-1.13.1/sbin/nginx
```
目录因你装的位置不同而不一样。启动之后去你的浏览器输入127.0.0.1:80（后面为你的端口号 ）

![img](http://img.blog.csdn.net/20170823145823643?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbDU1NDcyNTcyMg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



至此，ngxin搭建完毕。么么哒！