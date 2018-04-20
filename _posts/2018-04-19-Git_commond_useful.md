---
layout: post
title: Git常用命令
date: 2018-04-19 
tag: git
---

 Git常用命令
---
随着Git使用的频繁，对Git的操作也越来越熟悉，所熟知的命令越来越多，在此记录常用的Git命令
---

## 1.初始化本地git目录（项目）

```
git init
```
此命令用于初始化本地git目录（项目）是使用git的第一步，也是最常用的命令之一

## 2.关联远方仓库
初始化本地git之后，就要将本地与远程git仓库相关联，前提是远程仓库要存在，使用下面命令关联远程仓库（项目）

```
git add remote origin 'git项目地址'
```
## 3.获取项目分支
因为大多数情况下，我们是不操作master分支的，所以我们要获取／创建分支。

```
git fetch
```
该命令会拉取项目的分支情况，并使用

```
git checkout -b 本地分支 origin/远程分支
```
该命令会创建本地分支并且拉取远程仓库某分支。

当我们修改好代码之后，就需要上传代码到远程仓库
```
git branch
```
可以使用该命令查看当前分支

```
git checkout 本地分支名
```
可以使用该命令切换本地分支。
## 4.推送代码至仓库
推送本地分支至远程分支

```
git push origin 本地分支名:远程分支名
```
## 5.合并(merge)分支

```
git checkout targetBranch  //切换到targetBranch
git merge --no-ff sourceBranch  //建议merge的时候总是用 --no-ff 选项
git push origin targetBranch   //把sourceBranch的修改merge到targetBranch之后，push到远程的targetBranch
```
## 6.查看git状态

```
git status
```
