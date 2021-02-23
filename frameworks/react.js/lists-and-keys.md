# Lists and Keys

就像这样：

```jsx
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>{number}</li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

记得放key，最好是data的id，别用index（自然啦，因为顺序一变index和id的含义就很不一样了），如果没写key，会给warning，并默认成index。

key在sibling之间unique。

key不是一个prop，在组件内部的props里读不到，如果需要的话另外搞一个prop。

