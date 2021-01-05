# Handling Events

[https://reactjs.org/docs/handling-events.html](https://reactjs.org/docs/handling-events.html)

React里的event不是原生的event，是[synthetic](https://reactjs.org/docs/events.html)的，不过还是按w3c spec的规则的。

常见例：

```jsx
function ActionLink() {
  function handleClick(e) {
    // 原生html里可以return false来达到preventDefault的效果，react里不行
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}> // 这里添加listener
      Click me
    </a>
  );
}
```

在class形式的component里，需要注意handler里this的处理。react推荐在constructor里bind一次：

```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

也可以走另外两种，即一[public class fields syntax](https://babeljs.io/docs/plugins/transform-class-properties/)：

```jsx
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>button</button>
    );
  }
}
```

或者二：

```jsx
<button onClick={this.handleClick.bind(this)}>button</button>
<button onClick={() => this.handleClick()}>button</button>
```

第一种需要借助一种experimental的语法，不过在用create-react-app创建的项目里这个语法是默认enable的。

第二种也行，但是有个性能问题，就是它每次都会创建一个新函数，如果作为prop传给子component可能会导致其额外re-render？

> In most cases, this is fine. However, if this callback is passed as a prop to lower components, those components might do an extra re-rendering. We generally recommend binding in the constructor or using the class fields syntax, to avoid this sort of performance problem.

okk，那如果是create-react-app的我就用public class fields syntax，如果不是我就在constructor里bind吧= =。

