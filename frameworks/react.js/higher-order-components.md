# Higher-Order Components

> Concretely, **a higher-order component is a function that takes a component and returns a new component.**

说起来不是react的一部分，而是js函数。

可以给组件加一些附加功能，特别这种“附加”过程是值得抽取独立的重复逻辑的话。记得把props传给被包裹的组件。记得设displayName。

### caveats

#### Static Methods Must Be Copied Over <a id="static-methods-must-be-copied-over"></a>

或者用一个小lib： [hoist-non-react-statics](https://github.com/mridgway/hoist-non-react-statics) 

```jsx
import hoistNonReactStatic from 'hoist-non-react-statics';
function enhance(WrappedComponent) {
  class Enhance extends React.Component {/*...*/}
  hoistNonReactStatic(Enhance, WrappedComponent);
  return Enhance;
}
```

