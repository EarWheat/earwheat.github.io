---
layout: post
title: Ant Design Pro自学笔记（2）——路由与菜单
date: 2018-07-21 
tag: Ant Design
---

# Ant Design Pro自学笔记（1）—入门

## 配置新的路由和菜单

在Ant Design Pro中，前端路由是通过react-router4.0进行路由管理的。 
下面，我们将会讲解如下添加一个新的路由，并在前端页面中增加一个菜单来对应该路由。 
在Ant Design Pro中，路由的配置文件统一由==src/common/router.js==文件进行管理。 
示例内容如下：

```
const routerConfig = {
    '/': {
      component: dynamicWrapper(app, [], () => import('../layouts/BasicLayout')),
    },
    '/dashboard/monitor': {
      component: dynamicWrapper(app, ['monitor'], () => import('../routes/Dashboard/NgMonitor')),
    },
    '/dashboard/workplace': {
      component: dynamicWrapper(app, ['monitor'], () => import('../routes/Dashboard/NgWorkSpace')),
    }
}
```
其中，包含了三个路由：/，/dashboard/monitor，/dashboard/workplace。 
同时，指定了这三个路由分别对应的页面文件为layouts/BasicLayout.js，routes/Dashboard/NgMonitor.js以及Dashboard/NgWorkSpace.js文件。

下面，我们可以在侧边栏中填写一项来对应到我们添加的路由中： 
示例如下：


```
const menuData = [{
  name: 'dashboard',
  icon: 'dashboard',  // https://demo.com/icon.png or <icon type="dashboard">
  path: 'dashboard',
  children: [{
    name: '分析页',
    path: 'analysis',
  }, {
    name: '监控页',
    path: 'monitor',
  }, {
    name: '工作台',
    path: 'workplace',
  }, {
    name: '测试页',
    path: 'test',
  }],
}, {
  // 更多配置
}];
```

## 创建一个页面
下面，我们继续来创建一个页面，对应到我们添加的路由中。 
在==src/routes==下创建对应的js文件即可。 


```
import React, { PureComponent } from 'react';
export default class Workplace extends PureComponent {
  render() {
    return (
      <h1>Hello World!</h1>
    );
  }
}
```


## 新增一个组件

假设该页面相对复杂，我们需要引入一个组件呢？ 
我们可以在components文件夹下创建一个组件，并在newPage.js引入并使用。 
示例如下： 
==components/ImageWrapper/index.js：==

```
import React from 'react';
import styles from './index.less';    // 按照 CSS Modules 的方式引入样式文件。
export default ({ src, desc, style }) => (
  <div style="{style}" classname="{styles.imageWrapper}">
    <img classname="{styles.img}" src="{src}" alt="{desc}">
    {desc &amp;&amp; <div classname="{styles.desc}">{desc}</div>}
  </div>
);
```
