# JSX

[https://reactjs.org/docs/introducing-jsx.html](https://reactjs.org/docs/introducing-jsx.html)

## Intro

JSX就是React里面的“template”，它提供类似于HTML的tag语法，用来描述希望渲染出来的DOM的样子。

```jsx
const myjsx = (
    <div className={varClassName} id="normal-string">
        {varContent}
    </div>
)

// 然后render：
ReactDOM.render(
  myjsx,
  document.getElementById('my-container')
)
```

这点似乎和vue的template或者其他template差不多，妙就妙在它其实不是markup，而是js。

React推荐用Babel来编译jsx，编译完了之后其实就是对React.createElement\(\)函数的一次调用：

```javascript
const myjsx = React.createElement(
  'div',
  {className: varClassName, id: 'normal-string'},
  varContent
);
```

所以可以对它做各种奇怪的js事情：

```jsx
const div1 = <div></div> // 它可以很复杂
const div2 = <div></div>
const div3 = <div></div>

const wrapper = ( //可以像这样保存变量然后以后使用
    <div>
        <div>{div1}</div>
        {div2}
        <div><div>{div3}</div></div>
    </div>
)

// ---

[1, 2, 3].map(v => {
    const newValue = complexProcess(v)
    return (
        <div>{newValue}</div> // 可以像普通值一样被返回
    )
})
```

不过它还是不能真的像一个js里的HTMLElement那样呢，比如生成之后再修改它的attribute，往里面插child之类的🤔。

Note：

* 推荐在jsx表达式的两边加上`()`，以防止js蜜汁“自动插入分号”现象造成的诡异断句问题
* 由于jsx相比html更类似js，命名规则采用的是camelCase。有些名字要注意下，比如class和className
* jsx可以防止一般的注入，即使你直接插字符串进去`<div>{dangerousString}</div>`，它也会自动帮你escape的，很棒棒哦

## Why

按照React逻辑，虽然他们也是认可separate concerns的，但是他们不认为把代码分成markup和script是一个好想法，因为在这个状况下DOM内容本来就是脚本计算得到的，不如写在js里（？）。（react不是把css也写在js里了嘛…

他们的separate体现在拆分components上。

