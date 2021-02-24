# Forwarding Refs

ref感觉不像是prop那样从上往下传数据，而是像opencv函数调用那样，传下去一个空的variable，让里面去赋值…于是父组件里就能拿到子组件内部的东西的引用了，在某些场合（比如需要控制组件内原生 input的focus）时会有用。

ref和key不是prop，会被特殊处理（hmmm，遇到特殊处理事情就烦了起来），在子组件的props中是拿不到ref的，要拿到则必须用React.forwardRef函数，它的参数函数的第二个参数就是ref，然后把ref赋给内部的element。

```jsx
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// You can now get a ref directly to the DOM button:
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
// When the ref is attached, ref.current will point to the <button> DOM node.
```





