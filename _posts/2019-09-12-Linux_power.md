---
layout: post
title: Linux权限详解
date: 2019-09-12 
tag: Linux
---

## 1.背景
刷题的时候，Linux专栏考得比较多的就是权限系统，因此特对权限系统作一个了解介绍。

## 2.Linux文件类型
 文件列表信息分为：文件类型、权限、链接数、所属用户、所属用户组、文件大小、最后修改时间、文件名。
 
 linux一共有7种文件类型,分别如下:
 - 
             -：普通文件

         　　d：目录文件
         　　l： 软链接（类似Windows的快捷方式）
        　　(下面四种是特殊文件)
         　　b：块设备文件（例如硬盘、光驱等）
        　　 p：管道文件
         　　c：字符设备文件（例如猫等串口设备）
         　　s：套接口文件/数据接口文件（例如启动一个MySql服务器时会产生一个mysql.sock文件）
         　　
         　　
文件又分为三种权限

- r:对应数字4，可读
- w:对应数字2，可写
- x:对应数字1，可执行
 

文件权限信息每三位一组

rwxr-xr-x  5 root root  94 Jun 27 2017 xdg

　　rwx：代表文件所有者(u表示)权限，这里是root，root对该文件拥有读写执行权限。

　　r-x :  代表所属组(g表示)的权限，这里所属组拥有对该文件读和执行的权限。

　　r-x :  代表其他人(o表示)的权限，这里和上面权限一样。

 

 　　当然我们也可以修改文件的权限。

　　 linux中chmod命令就可以用来变更文件或目录的权限。

 　　语法:  chmod （选项） 模式  文件名
 　　
## 3.总结
总结下，权限对文件及目录的作用

  　　对于文件：

  　　　　r ： 可以读取文件内容(比如命令 cat more head tail)。

 　　　　w ： 可以编辑文件(比如命令 vim echo )，但是不能删除文件，因为文件名没有放在自己的文件空间，而是放在了上一级的目录空间下。

 　　　　x ： 可以执行。

　　 对于目录：

              r：可以查询目录下的文件（比如命令ls  ll）。

             w：具有修改目录结构的权限，比如新建文件和目录，删除此目录下文件和目录，重命名此目录下文件和目录，剪切和复制（比如命令cp mv touch rm）。

　　      x：目录有执行权限但是不能运行，可以进入目录（cd命令）。

     对文件来说最高权限是x，对于目录来说最高权限是w，一般给目录赋予权限0 ，5（rx），7（rxw），赋予4 ，1， 6都是没有意义的。对于文件能否删除，首先要对目录具有执行权限，同时对文件也具有执行权限。