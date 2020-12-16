# React Hooks

## 前端框架做了什么

优雅地表达渲染内容？重新渲染的控制？

开发效率、可维护性

cjh：data、view依赖data关系的描述、data更新时view的更新

依赖与监听刷新

## 类组件与函数组件

类组件：2013，v0.3.0，initial public release，含有state, props, render, lifecycle 

函数组件：2016，v15.0.0，stateless components，仅含props和render。 

* （渲染？）高效，因为无需处理props和lifecycle。
* 若数量多的时候可以尝试使用。
* 使用方法： 
  * 高层组件：类组件，含有数据结构与更新方法，此两者作为props传给子的函数组件
  * 低层组件：函数组件，高效渲染，不处理数据

## react hooks

上面版本使用函数组件的方式有点麻烦。 

react hooks：2019，v16.8.0，针对函数组件，补充使用state和lifecycle以及其他react features的方式

```javascript
  import React, {useState} from 'react'
  export default function App() {
    const [count, setCount] = useState(0) // 初始值：0；返回是数组，两个元素，一个是状态一个是更新状态的方法
    return (
      <>
        <div>{count}</div>
        <button onClick={() => setCount(count + 1)}>Add</button>
      </>
    )
  }
```

### 哪些hooks？

* 官方hooks 
  * 基础：useState, useEffect, useContext 
  * 进阶：useReducer, useRef, useMemo, useCallback, useImperativeHandle, useDebugValue, useLayoutEffect 
* 第三方hooks
  * 例如阿里的ahooks 
* 自己写的hooks
  * 万物皆可hooks：useXXX\(\)



* useState: 定义state和更新state的方法 
* useEffect: vue watch? 可以模拟一些lifecycle

## why react hooks

* 更好地组织代码逻辑
* 更好地复用逻辑
* 更好地优化渲染

## react hooks 原理

React Hook的实现原理和最佳实践（[https://zhuanlan.zhihu.com/p/75146261](https://zhuanlan.zhihu.com/p/75146261) 大概吧）



