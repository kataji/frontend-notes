# CSS variables

或者称作CSS custom properties，可以在css里定义变量然后之后再引用。我觉得最有趣的是，它打开了css和js通信的通道。

basic usage：

```css
body {
  --my-var: lightpink;
}

p {
  color: var(--my-var, black); /* 这里的black是fallback值 */
}

div {
  background: var(--my-var);
}
```

读取变量值用var\(\)函数，它第二个参数是默认值。默认值可以带逗号或者空格，比如字体列表或者padding的四个数字，都okk。

用css变量和js通信，可以获取用户交互的信息，达到根据交互改变样式的效果👍：

```css
.tooltip {
    position: fixed;
    top: calc(var(--mouse-y, 500) * 1px - 30px);
    left: calc(var(--mouse-x, 500) * 1px - 60px);
    background: rgba(0,0,0, calc(var(--mouse-x, 500) / 2000))
}
```

```javascript
const docStyle = document.documentElement.style;
function setCssVariable(name, value) {
  docStyle.setProperty(name, value);
}

// ------only need to set value on event------- //

document.addEventListener('mousemove', (event) => {
  setCssVariable('--mouse-x', event.clientX);
  setCssVariable('--mouse-y', event.clientY);
});
```

js也能读css变量的值，虽然我还没想到这有什么卵用：

```javascript
// 从inline style里取
element.style.getPropertyValue('--my-var')
// 从来自任何地方的正在生效的style里取
// getComputedStyle是一个window上的方法
getComputedStyle(element).getPropertyValue('--my-var')
```

对了，css变量是有scope的，scope在定义它的那个selector里。如果要定义全局变量，不如：

```css
:root {
  --background: green;
}
```

以及，css变量是大小写敏感的。

