---
layout: post
title: Vue自学笔记（1）—入门
date: 2018-07-17 
tag: Vue
---

# Vue.js自学笔记（1）—入门

背景：因为公司业务采用的是angluarJs，单个人感觉angularJs并不是特别好用顺手，经过对前端同学的一番询问咨询，决定自学Vue.js，于是又了这篇笔记。

## 从Hello world开始
基本上任何语文都是从经典的Hello World开始，这次学习Vue也不例外


```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
<script src="https://cdn.bootcss.com/vue/2.4.2/vue.min.js"></script>
</head>
<body>
<div id="app">
  <p>{{ message }}</p>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!'
  }
})
</script>
</body>
</html>
```


## Vue的安装

我使用的是NPM的安装方法，具体代码如下：
```
# 最新稳定版
$ cnpm install vue
# 全局安装 vue-cli
$ cnpm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
进入项目，安装并运行：
$ cd my-project
$ cnpm install
$ cnpm run dev
```

## Vue起步

==每个 Vue 应用都需要通过实例化 Vue 来实现。==


```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
	<script src="https://cdn.bootcss.com/vue/2.4.2/vue.min.js"></script>
</head>
<body>
	<div id="vue_det">
		<h1>site : {{site}}</h1>
		<h1>url : {{url}}</h1>
		<h1>{{details()}}</h1>
	</div>
	<script type="text/javascript">
		var vm = new Vue({
			el: '#vue_det',
			data: {
				site: "菜鸟教程",
				url: "www.runoob.com",
				alexa: "10000"
			},
			methods: {
				details: function() {
					return  this.site + " - 学的不仅是技术，更是梦想！";
				}
			}
		})
	</script>
</body>
</html>
```

接下来看看Vue构造器中有哪些参数，且这些参数的功能作用是什么。

- el：它是 DOM 元素中的 id。在上面实例中 id 为 vue_det，这意味着我们接下来的改动全部在以上指定的 div 内，div 外部不受影响。
- data：用于定义属性，实例中有三个属性分别为：site、url、alexa。
- methods：用于定义的函数，可以通过 return 来返回函数值。
- {{ }}：用于输出对象属性和函数返回值。


## Vue.js 模板语法

Vue.js 使用了基于 HTML 的模版语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。

1. 使用 v-html 指令用于输出 html 代码：
```
<div id="app">
    <div v-html="message"></div>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    message: '<h1>EarWheat</h1>'
  }
})
</script>
```
2. HTML 属性中的值应使用 v-bind 指令。

```
<div id="app">
  <label for="r1">修改颜色</label><input type="checkbox" v-model="class1" id="r1">
  <br><br>
  <div v-bind:class="{'class1': class1}">
    directiva v-bind:class
  </div>
</div>
    
<script>
new Vue({
    el: '#app',
  data:{
      class1: false
  }
});
</script>
```

==3.表达式
Vue.js 都提供了完全的 JavaScript 表达式支持。==

```
<div id="app">
    {{5+5}}<br>
    {{ ok ? 'YES' : 'NO' }}<br>
    {{ message.split('').reverse().join('') }}
    <div v-bind:id="'list-' + id">EarWheat</div>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    ok: true,
    message: 'Gloria',
    id : 1
  }
})
</script>
```

4. 指令
指令是带有 v- 前缀的特殊属性。

指令用于在表达式的值改变时，将某些行为应用到 DOM 上。如下例子：

```
<div id="app">
    <p v-if="seen">现在你看到我了</p>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    seen: true
  }
})
</script>
```

5. 参数
参数在指令后以冒号指明。例如， v-bind 指令被用来响应地更新 HTML 属性：


```
<div id="app">
    <pre><a v-bind:href="url">菜鸟教程</a></pre>
</div>
    
<script>
new Vue({
  el: '#app',
  data: {
    url: 'http://www.runoob.com'
  }
})
</script>

另一个例子是 v-on 指令，它用于监听 DOM 事件：
<a v-on:click="doSomething">
```

