# Rendering Elements

[https://reactjs.org/docs/rendering-elements.html](https://reactjs.org/docs/rendering-elements.html)

```jsx
const element = <div></div>

ReactDOM.render(
    element,
    document.getElementById('root')
)
```

👆这样就可以在root里面渲染element了。

Note：

* 是把element渲染到\#root里面，而不是替换了\#root（这点和vue不同
* element是immutable的，也就是说一旦创建，无法改变包括它的attribute和children等（回答了上一节jsx里的小问题
* 要更新element，需要重新创建一个，然后再`ReactDOM.render(newElement, container)`。虽然是这么写的，但是React也不会真的全部重绘，而是很聪明地diff然后只更新必须的部分
* 不过正经的情况下是使用stateful component，只需要调一次`ReactDOM.render`，其他的就交给组件的state更新机制

