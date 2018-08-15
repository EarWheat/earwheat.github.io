---
layout: post
title: Ant Design Pro页面加载原理及过程
date: 2018-08-15 
tag: 前端
---

# Ant Design Pro页面加载原理及过程

背景:业务使用ant design。写的一脸懵逼。记录一些相关知识。


## 过程图
废话少说，直接上图。看完图能理解一大半。
![image](https://images2018.cnblogs.com/blog/292888/201805/292888-20180508171202839-375319417.png)


## @connect 装饰器
组件写法中调用了 dva 所封装的 react-redux 的 @connect 装饰器，用来接收绑定的 list 这个 model 对应的 redux store。注意到这里的装饰器实际除了 app.state.list 以外还实际接收 app.state.loading 作为参数，这个 loading 的来源是 src/index.js 中调用的 dva-loading这个插件


```
/*
* src/index.js
*/
import createLoading from 'dva-loading';
app.use(createLoading());
```


View中使用

```
@connect(({ list, loading }) => ({
  list,//①
  loading: loading.models.list,//②
}))
```

说明：

　　　　1、connect 有两个参数,mapStateToProps以及mapDispatchToProps,一个将状态绑定到组件的props一个将方法绑定到组件的props

　　　　2、代码①：将实体list中的state数据绑定到props，注意绑定的是实体list整体，使用时需要list.[state中的具体变量]

　　　　3、代码②：通过loading将上文“数据map一”中的models的list的key对应的value读取出来。赋值给loading，以方便使用，如表格是否有加载图标

　　　　　　当然代码②也可以通过key value编写：loading.effects["list/fetch"]
　　　　　　
变量获取
因，在src/models/list.js

```

export default {
  namespace: 'list',
 
  state: {
    list: [],
  },
```

view中

```
  render() {
    const { list: { list }, loading } = this.props;

```

说明：

　　定义使用时：list: { list }  ，含义实体list下的state类型的list变量