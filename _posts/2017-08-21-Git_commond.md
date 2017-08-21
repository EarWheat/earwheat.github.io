---
layout: post
title: Git命令行提交代码
date: 2017-08-21 
tag: git
---
# Git命令行提交代码
## 1.下载Git
   win下去官网下载(https://git-scm.com/),Linux系统可以使用命令行下载
```
sudo apt-get install git
```
## 2.注册Github和创建代码库
   怎么创建git和代码仓库自行百度解决
## 3.设置Git全局用户名
   在Git工作之前，为了能够跟踪到是谁对这段代码进行了修改，需要做一个配置信息，主要是用户名和邮箱，如下：
```
git config --global user.name "your_username"  
git config --global user.email your_email@domain.com  
```
## 4.添加文件交于Git管理
   当我们配置完信息之后，需要初始化Git工作区并将我们的文件交于Git来管理，命令为：

```
git init
git add .
```
## 5.提交文件到本地仓库
   我们提交文件的时候，需要填写有意义的代码注释，以便于区别每次提交的作用以及意义。命令如下：
    
```
    git commit -m "first commit"  
```
## 6.推送至远程仓库
   我们需要将代码提交到远程服务器上面去，首先第一步是关联远方仓库

```
    git remote add origin https://your_username@bitbucket.org/your_username/name_of_remote_repository.git  
```
之后就是推送到远程仓库了，如果推送到主分支则是master，其余分支输入分支名即可。

```
    git push origin master
```
### 自此使用命令行推送git到远程仓库就已经全部完成了。参考了网上部分博客，此篇博客内容为自己经验手打。

