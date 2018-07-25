---
layout: post
title: Ant Design Pro自学笔记（1）—入门
date: 2018-07-21 
tag: Ant Design
---

# Ant Design Pro自学笔记（1）—入门

背景：业务需要，传统的angular开发已经难以适用于潮流，所以转型Ant Design

## 安装
本文直接采用git clone的方式安装：

```
$ git clone --depth=1 https://github.com/ant-design/ant-design-pro.git my-project
$ cd my-project
```

进入项目我们可以看到完整的目录结构

```
├── mock                     # 本地模拟数据
├── public
│   └── favicon.ico          # Favicon
├── src
│   ├── assets               # 本地静态资源
│   ├── common               # 应用公用配置，如导航信息
│   ├── components           # 业务通用组件
│   ├── e2e                  # 集成测试用例
│   ├── layouts              # 通用布局
│   ├── models               # dva model
│   ├── routes               # 业务页面入口和常用模板
│   ├── services             # 后台接口服务
│   ├── utils                # 工具库
│   ├── g2.js                # 可视化图形配置
│   ├── theme.js             # 主题配置
│   ├── index.ejs            # HTML 入口模板
│   ├── index.js             # 应用入口
│   ├── index.less           # 全局样式
│   └── router.js            # 路由入口
├── tests                    # 测试工具
├── README.md
└── package.json
```

## 本地开发


```
$ npm install
$ npm start
```


## 谈谈如何开发

首先，我们来简单了解一下整个项目的目录结构及其中每个文件的功能。 

```
├── mock                     # 本地模拟数据
├── public
│   └── favicon.ico          # Favicon
├── src
│   ├── assets               # 本地静态资源
│   ├── common               # 应用公用配置，如导航信息
│   ├── components           # 业务通用组件
│   ├── e2e                  # 集成测试用例
│   ├── layouts              # 通用布局
│   ├── models               # dva model
│   ├── routes               # 业务页面入口和常用模板
│   ├── services             # 后台接口服务
│   ├── utils                # 工具库
│   ├── g2.js                # 可视化图形配置
│   ├── theme.js             # 主题配置
│   ├── index.ejs            # HTML 入口模板
│   ├── index.js             # 应用入口
│   ├── index.less           # 全局样式
│   └── router.js            # 路由入口
├── tests                    # 测试工具
├── README.md
└── package.json
```

其中，关于业务功能的代码都在==src==文件夹下。 
而==src==中本身又包含着很多的子文件夹。 
其中，我们最常用的应该了==routes，services，models==以及==components。 ==
下面，我们来依次了解一下这四个文件夹的作用。

1. routes：每个路由对应的页面组件文件。主要定义具体页面的基本结构和内容。
2. services：用于与后台交互、发送请求等。
3. models：用于组件的数据存储，接收请求返回的数据等。
4. components：组件文件夹。每个页面可能是由一些组件组成的，对于一些相对通用的组件，建议将该组件写入components文件夹中，并在routes文件夹中的文件引入来使用。


