# Components

[https://reactjs.org/docs/components-and-props.html](https://reactjs.org/docs/components-and-props.html)

[https://reactjs.org/docs/react-component.html](https://reactjs.org/docs/react-component.html)

## Component

React的Component据说可以理解成函数，接收props，返回elements。

比如最简单的：

```jsx
function SimpleComponent(props) { // 函数名大写开头
    return (
        <div>{props.firstName + props.lastName}</div>
    )
}
```

或者利用es6的class语法：

```jsx
class ClassComponent extends React.Component {
    render() {
        return (
            <div>{this.props.firstName + this.props.lastName}</div>
        )
    }
}
```

然后用的时候就：

```jsx
ReactDOM.render(<MyComponent firstName="A" lastName="B"/>, container)
```

注意component名大写是必须的，不然React会把它当作DOM自带的tag类型。

还有注意不要去修改props值，Component面对props的时候应该表现得像纯函数一样，不去修改input，并且对相同的input永远返回相同的output。

## Composing

看React的教程，它挺鼓励把组件逐级拆分成小积木，然后再逐层搭起来的。

## State & Lifecycle

要写stateful的component，需要用到class语法：

```jsx
class StatefulComponent extends React.Component {
    constructor(props) {
        super(props) // 记得先call super
        this.state = { myState: new Date() } // 初始化state
    }
    render() {
        return <div>{this.state.myState}</div>
    }
    tick() {
        this.setState({ myState: new Date() }) // 需要调用setState来更新state
    }
    componentDidMount() {
        this.timerId = setInterval(() => this.tick(), 1000)
    }
    componentWillUnmount() {
        clearInterval(this.timerId)
    }
}
```

setState并没有同步地更新state，只是在异步队列里注册了一个任务而已。

所以如果在setState的时候需要上一个状态的值，不能直接读this.state，而需要把它写成一个接收state的函数：

```jsx
this.setState((state, props) => ({ // 上个state和此时的props
  counter: state.counter + props.increment
}));
```

还有，setState不需要整个儿地更新state，按property的粒度来更新就行。

