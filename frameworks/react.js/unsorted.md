# unsorted

Vue 的false会渲染成啥呢（会渲染成字符串false；null和undefined会无视；object会渲染成json，好像是自动调用toJSON；函数会渲染成走样的字符串\[狗头\]

React 会无视false，但是会渲染falsy，比如0（也会无视true和null和undefined；object：Objects are not valid as a React child；array渲染每个element；函数也被无视了…

online playground：

* [https://codesandbox.io/s/react-new](https://codesandbox.io/s/react-new)
* [https://codesandbox.io/s/vue](https://codesandbox.io/s/vue)

