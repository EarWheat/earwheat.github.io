---
layout: post
title: React自学笔记——记一个setState的坑
date: 2018-08-07 
tag: 前端
---

# React自学笔记——记一个setState的坑

背景介绍：研发使用Ant Design，由于一个==setState==的坑调试了一天，现在记录一下这个坑。

## ==setState==不会立即改变数据

举个例子：

```
// name is "hello world!"
this.setState({
    name: "hello EarWheat!"
})
console.log(`name is ${this.state.name}`)
```

代码并不会输出"hello EarWheat!",因为setState并不是同步的，所以set之后并不会立即改变state的值。

## 解决办法，使用回调函数


```
this.setState({
    name: "hello EarWheat!"
}, () => {
  console.log(`name is ${this.state.name}`)
})
```
这样才会如你所预料那般的输出.

对于上面的这个问题倒是还好.因为setState是放在一个队列里异步去处理的,是一个异步的batch操作。 所以在没有成功改变之前,输出的就是之前的值. 
在回调里显示正确的数据,那是因为callback的原因。[详情请戳这里。](https://reactjs.org/docs/react-component.html#setstate)


## setState多次,re-render一次
原本以为每一次setState都会re-render一次，然而事实并非如此。

```
componentDidMount(){
  this.setState((prevState, props) => ({count: this.state.count + 1})) // 1
  this.setState((prevState, props) => ({count: this.state.count + 1})) // 2
  this.setState((prevState, props) => ({count: this.state.count + 1})) // 3
  this.setState({name: "EarWheat"}) // 4
}

render(){
  console.log("render")
  return(<h1>SetState</h1>)
}
```
可以发现,这里只会出现render两次,而不是想象中的4+1(初始化的render)。原因在于setState塞进一个队列里去做异步的处理的,就是说他是把我们这4个setState操作放到了一个队列里,然后batch操作。如果将代码换成异步就能解决。

```
componentDidMount(){
    setTimeout(() => {
        this.setState(count: this.state.count + 1)
        this.setState(count: this.state.count + 1)
    })
}
```

可以发现,如果改成这样,也会触发re-render. 可是这是为啥setTimeout里的两次this.state.count会成功,而不是像上面一样,浅合并成一个呢? 还得继续摸索。

## 总结

1. setState操作,默认情况下是每次调用, 都会re-render一次,除非你手动shouldComponentUpdate为false. 
react为了减少re-render的次数,会进行一个浅合并.将多次re-render减少到一次re-render.

2. setState之后,无法立即获取到this.state的值,是因为在setState的时候,他只会把操作放到队列里.

