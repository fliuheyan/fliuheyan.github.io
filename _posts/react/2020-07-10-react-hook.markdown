---
layout: post
title:  "react"
date:   2020-07-10 11:11:11 +0100
---

## React Hook
Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。
组件尽量写成纯函数，如果需要外部功能和副作用，就用钩子把外部代码"钩"进来
Hook 是一些可以让你在函数组件里“钩入” React state 及生命周期等特性的函数


### React 生命周期

#### mount

* constructor()
* static getDerivedStateFromProps()
替换`componentWillReceiveProps`的.
用途，在组件初始化的时候将接收到的props参数添加到它的state当中，期望组建能够相应props值的变化.
* render()
* componentDidMount()

#### Update
* static getDerivedStateFromProps()
* shouldComponentUpdate()  
HOOK替换React.memo 包裹一个组件来对它的 props 进行浅比较

* render()
* getSnapshotBeforeUpdate()
* componentDidUpdate()
* componentWillUnmount



### 使用规则

* 只能在函数最外层调用 Hook。不要在循环、条件判断或者子函数中调用。
```
const [a, setA] = useState(false);
const [b, setB] = useState(false);
```

* 只能在 React 的函数组件中调用 Hook。不要在其他 JavaScript 函数中调用。（还有一个地方可以调用 Hook —— 就是自定义的 Hook 中，我们稍后会学习到。）


#### State Hook
 `useState()`用于为函数组件引入状态（state）
相当于class component里的state，与store中的state无关

```
const [count, setCount] = useState(0);
```

等价于
```
constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
```

#### `useContext()`
提供了在组建之间共享数据的一种方法.
基本的使用方法:
1. `const myContext = React.createContext(defaultValue)` react创建一个context
2. 将


[context blog](http://www.ptbird.cn/react-createContex-useContext.html#menu_index_5)

#### `useEffect`
useEffect 就是一个 Effect Hook，给函数组件增加了操作副作用的能力。它跟 class 组件中的 componentDidMount、componentDidUpdate 和 componentWillUnmount 具有相同的用途，只不过被合并成了一个 API。
function component中执行带有side effect
在组件每次重新渲染之后都会被执行    
    * 无需清除的effort
    api request. 在class component当中，很多时候我们需要react render出DOM之后才进行更新.所以会把副作用放在`
    componentDidMount` `componentDidUpdate`当中。
    但是class react并不提供每次组件渲染的这种call back函数.所以同一个实现往往需要写在`componentDidMount`(组件第一次加载) `componentDidUpdate`(组件更新)
    
    * 需要清除的
    //TODO ???? 为什么需要清除？？？？
    如果你的 effect 返回一个函数，React 将会在执行清除操作时调用它

##### 使用场景

    

#### `useMemo`
??? 默认父组建渲染，是不是子组建都需要重新渲染

使用 useMemo 当 deps 不变时，直接返回上一次计算的结果，从而使子组件跳过渲染。

#### `useReducer`

```
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```
当dependencies不改变的时候组件不会重新渲染

### Customize Hook
自定义 Hook 是一个函数，其名称以 “use” 开头，函数内部可以调用其他的 Hook

通过自定义 Hook，可以将组件逻辑提取到可重用的函数中
在组件之间重用一些状态逻辑
自定义 Hook 必须以 “use” 开头吗？必须如此

### FAQ
https://zh-hans.reactjs.org/docs/hooks-faq.html

### Reference

[official document](https://reactjs.org/docs/hooks-rules.html)
