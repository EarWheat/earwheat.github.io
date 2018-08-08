---
layout: post
title: React自学笔记（1）——入门
date: 2018-08-08 
tag: React
---

# React自学笔记（1）——入门

背景介绍：公司准备使用Ant Design，但是学习了Ant才发现，Ant是基于React的，于是又调回头来学习React。

## Hello world
入门还是从Hello World入门吧，毕竟都成了传统了。

```
<div id="example"></div>
<script type="text/babel">
  ReactDOM.render(
    <h1>Hello, world!</h1>,
    document.getElementById('example')
  );
</script>
```

## React的特点
1. 声明式设计 −React采用声明范式，可以轻松描述应用。
2. 高效 −React通过对DOM的模拟，最大限度地减少与DOM的交互。
3. 灵活 −React可以与已知的库或框架很好地配合。
4. JSX − JSX 是 JavaScript 语法的扩展。React 开发不一定使用 JSX ，但我们建议使用它。
5. 组件 − 通过 React 构建组件，使得代码更加容易得到复用，能够很好的应用在大项目的开发中。
6. 单向响应的数据流 − React 实现了单向响应的数据流，从而减少了重复代码，这也是它为什么比传统数据绑定更简单。

## React元素渲染
首先我们在一个 HTML 页面中添加一个 id="example" 的 <div>:
```
<div id="example"></div>
```
在此 div 中的所有内容都将由 React DOM 来管理，所以我们将其称之为 "根" DOM 节点。

我们用 React 开发应用时一般只会定义一个根节点。但如果你是在一个已有的项目当中引入 React 的话，你可能会需要在不同的部分单独定义 React 根节点。

要将React元素渲染到根DOM节点中，我们通过把它们都传递给 ==ReactDOM.render()== 的方法来将其渲染到页面上：


```
const element = <h1>Hello, world!</h1>;
ReactDOM.render(
    element,
    document.getElementById('example')
);
```

## React JSX
React 使用 JSX 来替代常规的 JavaScript。

JSX 是一个看起来很像 XML 的 JavaScript 语法扩展。

我们可以在代码中嵌套多个 HTML 标签，需要使用一个 div 元素包裹它，实例中的 p 元素添加了自定义属性 data-myattribute，添加自定义属性需要使用 data- 前缀。

```
ReactDOM.render(
    <div>
    <h1>菜鸟教程</h1>
    <h2>欢迎学习 React</h2>
    <p data-myattribute = "somevalue">这是一个很不错的 JavaScript 库!</p>
    </div>
    ,
    document.getElementById('example')
);
```

### JavaScript 表达式

我们可以在 JSX 中使用 JavaScript 表达式。表达式写在花括号 {} 中。实例如下：

```
ReactDOM.render(
    <div>
      <h1>{1+1}</h1>
    </div>
    ,
    document.getElementById('example')
);
```

在 JSX 中不能使用 if else 语句，但可以使用 conditional (三元运算) 表达式来替代。以下实例中如果变量 i 等于 1 浏览器将输出 true, 如果修改 i 的值，则会输出 false.

```

ReactDOMReactDOM.render(
    <div>
      <h1>{i == 1 ? 'True!' : 'False'}</h1>
    </div>
    ,
    document.getElementById('example')
);
```

