# JSX in Depth

[https://reactjs.org/docs/jsx-in-depth.html](https://reactjs.org/docs/jsx-in-depth.html)

> JSX just provides syntactic sugar for the `React.createElement(component, props, ...children)` function

可以玩 [the online Babel compiler](https://babeljs.io/repl/#?presets=react&code_lz=GYVwdgxgLglg9mABACwKYBt1wBQEpEDeAUIogE6pQhlIA8AJjAG4B8AEhlogO5xnr0AhLQD0jVgG4iAXyJA) 看jsx编译成了啥。

### tag部分

要是首字母大写的变量名，或者`A.B`这种也可以。如果不是这样的，就先赋给一个MyComponent的变量再用在jsx里。

由于直接编译成`React.createElement(Component)`，所以React和Component都必须是in scope的js变量。

### props

好像没啥特别的。

### children

children也是特别的prop，不过它仅仅特殊在如何传入上——不写在attr那里，而是写在tag之间，有默认的名字叫children。子组件拿到之后就是普通地props.children这样用。

children虽然好像要传element，其实和其他prop一样传啥都行，函数也行。

