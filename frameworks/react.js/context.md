# Context

[https://reactjs.org/docs/context.html](https://reactjs.org/docs/context.html)

一个功能和Vue的provide/inject差不多的东西？

> Context is primarily used when some data needs to be accessible by _many_ components at different nesting levels. Apply it sparingly because it makes component reuse more difficult.

### 创建

```jsx
const ThemeContext = React.createContext('light'); // 创建时给一个默认值
```

### 祖先组件提供值

```jsx
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      theme: 'dark',
    };
  }
  
  render() {
    return (
      <ThemeContext.Provider value={this.state.theme}>
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}
```

### 子孙组件使用值

```jsx
// class 组件
class ThemedButton extends React.Component {
  // 如果用experimental public class fields syntax，
  // 可以写static contextType = ThemeContext;
  // 就不用另外写ThemedButton.contextType = ThemeContext;了
  render() {
    let props = this.props;
    let theme = this.context; // 定义了contextType之后就能用this.context取到context的值
    return (
      <button
        {...props}
        style={{backgroundColor: theme === 'dark' ? '#000' : '#fff'}}
      />
    );
  }
}
ThemedButton.contextType = ThemeContext;

// function 组件
function ThemeTogglerButton() {
  return (
    <ThemeContext.Consumer>
      {(theme) => (
        <button
          style={{backgroundColor: theme === 'dark' ? '#000' : '#fff'}}
        />
      )}
    </ThemeContext.Consumer>
  );
}
```

### deep子孙组件更新值

可以把context值搞成`{value: someValue; method: someMethod}`，这样的形式传下去。

### 使用多个context的值

nest起来：

```jsx
class App extends React.Component {
  render() {
    const {signedInUser, theme} = this.props;

    // App component that provides initial context values
    return (
      <ThemeContext.Provider value={theme}>
        <UserContext.Provider value={signedInUser}>
          <Content />
        </UserContext.Provider>
      </ThemeContext.Provider>
    );
  }
}

// A component may consume multiple contexts
function Content() {
  return (
    <ThemeContext.Consumer>
      {theme => (
        <UserContext.Consumer>
          {user => (
            <ProfilePage user={user} theme={theme} />
          )}
        </UserContext.Consumer>
      )}
    </ThemeContext.Consumer>
  );
}
```

> If two or more context values are often used together, you might want to consider creating your own render prop component that provides both.

