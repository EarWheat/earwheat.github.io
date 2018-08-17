---
layout: post
title: redux在react中的应用
date: 2018-08-17 
tag: 前端
---

# redux在react中的应用

背景：反正就是在用react中需要理解的内容

## 简介
Redux是JavaScript状态容器，提供可预测化的状态管理。redux的集成是非必选的。本文主要介绍的大纲如下：
- store
- action
- reducer
- dispatch
- middleware

我们知道在react中数据是单向流动的。所谓的数据单向流动，顾名思义，就是数据是从一个方向传播下去的意思。来一个有趣的例子，
我们假设数据中心是一个负责发行身份证的地方政府，每个人都可以到这里来领取自己的身份证，但是如果你对自己的身份证头像不满意，你不能自己去村口拍个十块钱的大头贴贴上去，也不能自己修改上面的个人信息，这样的身份证是不被承认的（弄不好可能还得坐牢），正确的操作方式应该是，带着你的修改意见，去到地方政府，找相关工作人员帮你重新办理，办理完之后再发放给你。

这个例子当中，数据中心我们视为父组件，身份证就是子组件通过父组件接收到的props，你不能直接修改自己的身份证，也就是你不能直接
```
this.props.name = '葬爱'
```
，只能通知父组件去修改这个props，即调用父组件的提供的changeName方法来操作：this.props.changeName('葬爱')，这个过程事实上是修改了父组件的state，从而使得子元素接收到的数据也发生了改变。所以数据由始至终都是从父元素流向子元素，我们称为数据单向流动。


数据的传递是单向的，子组件的数据只能就近获取，但是真正的数据源却离得太远，没有捷径可以直接通知数据源更新状态。

redux的出现改变了react的这种窘迫处境，它提供了整个应用的唯一数据源store，这个数据源是随处可以访问的，不需要靠父子相传，并且还提供了（间接）更新这个数据源的方法，并且是随处可使用的！


## store

store 是redux提供的唯一数据源，它存储了整个应用的state，并且提供了获取state的方法，即store.getState()。==store是只读的。==

redux没有提供直接修改数据的方法，改变state的唯一方法就是触发（dispatch） action 。


## action

action 是一个用于描述已发生事件的普通对象。

简单来说，就是“你干了一件什么事情”。

但是单单讲了你干的事情，我们并不知道你干的这件事产生了什么牛逼效果，于是有了一个专门负责描述某个行动对应产生某种效果的机构，叫做 reducer 。

## reducer
reducer 只是一个接收state和action，并返回新的state的函数。

它就像是一部法典，根据你所做的事情，提供对应的后果，这个后果直接对数据源起作用。

ok，现在我们知道怎么修改数据源了，首先必须先定义好我们即将做的事情，也就是定义一个action，跟着，我们需要相对应地补充我们做的这件事要怎么影响数据源，于是我们根据这个action补充了一个reducer，最后我们触发这个action：store.dispatch(action)，数据源就会根据reducer定义好的规则来更新自己了。

## 举例说明
我们假设整个应用只有两个组件，一个身份证组件，一个弹窗组件，那么应用的state树应该是这样子的：

```
// store.getState()

{
  card: {
    name: 'Jack',
    picture: 'a.jpg'
  },
  dialog: {
    status: false
  }
}
```
action本质上是一个普通的js对象，因为它只是一个用来描述事件的对象，先来两个现成的action看看：

```
{
  type: 'CHANGE_NAME',
  name: '葬爱'
}

{
  type: 'CHANGE_PICTURE',
  picture: 'b.jpg'
}
```
action都会带一个type属性，这个属性是必选的，而其他的内容，比如name、picture等等，都是可选的，它们是由action携带，最后传递给reducer的内容，就好比我说我要改名字，这是事件，但是我没有说我要改成什么名字，这个操作就不完整，所以我还需要补充说，我要改名叫“葬爱”，所以我还需要提供一个name给你，这才能完整实现一个动作。这些附属的参数，我们称为 payload（载荷）。

我们说过payload是可选的，也就是说，有些动作的触发是不需要其他信息的，比如“激活弹窗”、“关闭弹窗”等等，这类动作只需要一个type就可以传达意思了：

```
{
  type: 'SHOW_DIALOG'
}

{
  type: 'CLOSE_DIALOG'
}
```

于是，一个完整的触发动作是这样的：

```
// 修改名字
dispatch({
  type: 'CHANGE_NAME',
  name: '葬爱'
})
// 激活弹窗
dispatch({
  type: 'SHOW_DIALOG'
})
```

我们已经知道如何触发一个动作，现在我们来了解如何接收并处理这个动作，也就是补充一个reducer。

可以认为，reducer就是根据传入的各种action不同，相对应对state进行处理，最后返回一个新state的函数。

那么可以推断出这个函数需要的参数至少是：当前的state，以及一个action。

事实也正是如此，下面是一个真实的reducer：

```
function reducer(state, action) {
  switch (action.type) {
    case 'CHANGE_NAME':
    return {
      card: {
        name: action.name, // 使用action携带的新name
        picture: state.card.picture  // 不需要修改，使用旧state的值
      },
      dialog: state.dialog  // 不需要修改，使用旧state的值
    }

    case 'SHOW_DIALOG':
    return {
      card: state.card,  // 不需要修改，使用旧state的值
      dialog: {
        status: true
      }
    }

    case 'CLOSE_DIALOG':
    return {
      card: state.card,  // 不需要修改，使用旧state的值
      dialog: {
        status: false
      }
    }

    default:
    return state  // 没有匹配的action type，返回原来的state
  }
}

```
如上，reducer接收一个修改前的state和一个action，然后通过判断actionType的方式来进行不同操作（没有匹配的actionType则默认返回原state），而这个操作的最终目的就是 拼装 一个新的state，并最终return，这样就达到更新state的目的了！

看到这里你可能会有疑问，为什么不直接拿state，然后修改它state.card.name = action.name，最后return state不就好了吗？

这是一个不好的实践，因为state是一个对象，直接修改state是会对其他引用了state的地方产生影响的，这种影响我们称为 副作用 ，而redux规定reducer必须是 纯函数 ，纯函数是没有副作用的。

```
reducer 一定要保持纯净，只要传入参数相同，返回计算得到的下一个 state 就一定相同。没有特殊情况、没有副作用，没有 API 请求、没有变量修改，单纯执行计算。 ——redux官方文档
```
我们回到上面的示例代码来，仔细观察这个state，我们发现它由两部分构成，分别是card和dialog，但是这两者之间并没有关联，那么我们可不可以把它们拆分出来，分别管理呢？

```
function cardReducer(state, action) {
  switch (action.type) {
    case 'CHANGE_NAME':
    return {
      name: action.name, // 使用action携带的新name
      picture: state.card.picture  // 不需要修改，使用旧state的值
    }

    default:
    return state  // 没有匹配的action type，返回原来的state
  }
}

function dialogReducer(state, action) {
  switch (action.type) {
    case 'SHOW_DIALOG':
    return {
      status: true
    }

    case 'CLOSE_DIALOG':
    return {
      status: false
    }

    default:
    return state  // 没有匹配的action type，返回原来的state
  }
}

function reducer(state, action) {
  return {
    card: cardReducer(state.card, action),
    dialog: dialogReducer(state.dialog, action)
  }
}

export default reducer

```

这个由两个reducer组成的大reducer所实现的功能，跟之前只有一个reducer实现的功能是没有差别的！我们成功地将原本比较笼统的reducer拆分成了多个专注于不同功能的reducer，如果你愿意的话，还可以继续拆下去，但目前来看没有这个必要。

这是个很重要的思想！强调一遍，拆分reducer是一个很重要的思想，这是我们之后编写reducer的最基本方式。

值得一提的是，redux对reducer的合并提供了一些便捷的方法，我们可以这么写：
```
function card(state, action) {
  switch (action.type) {
    case 'CHANGE_NAME':
    return {
      name: action.name, // 使用action携带的新name
      picture: state.card.picture  // 不需要修改，使用旧state的值
    }

    default:
    return state  // 没有匹配的action type，返回原来的state
  }
}

function dialog(state, action) {
  switch (action.type) {
    case 'SHOW_DIALOG':
    return {
      status: true
    }

    case 'CLOSE_DIALOG':
    return {
      status: false
    }

    default:
    return state  // 没有匹配的action type，返回原来的state
  }
}

export default combineReducers({
  card,
  dialog
})

```
需要特别注意，使用combineReducers来合并reducer，需要子reducer的名字跟对应要接收的state的key一致，所以你看到上面我们把原来的子reducer名字分别从cardReducer和dialogReducer，改为了card和dialog。

作者：Jack_Lo
链接：https://www.jianshu.com/p/7a71181a7aa0
來源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。
