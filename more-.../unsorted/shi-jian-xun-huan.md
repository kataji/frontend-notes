# 事件循环

> [https://mp.weixin.qq.com/s/QgfE5Km1xiEkQqADMLmj-Q](https://mp.weixin.qq.com/s/QgfE5Km1xiEkQqADMLmj-Q)

分浏览器和后端nodejs，不过现在node好像在和浏览器统一

宏任务：macro-tasks，新称tasks

* script\(整体代码，可以理解成整个代码是一个函数，然后自动调用了这个函数？
* setTimeout
* setInterval
* setImmediate
* I/O
* UI render （浏览器only

微任务：micro-tasks，新称jobs

* process.nextTick
* Promise
* Async/Await\(实际就是promise\)
* MutationObserver\(html5新特性，浏览器only\)

循环做法就是执行一个宏任务，然后执行该宏任务产生的微任务。如果在此过程中微任务产生了新的微任务，那就再执行这些微任务，直到执行完所有微任务，那就执行下一个宏任务，循环。

chrome对于async好像有一些不标准的优化。

node的话版本11是个坎，10及以上会不一致一点。node事件循环分阶段，重视的其实也就poll、check、timers阶段。

![](../../.gitbook/assets/image%20%282%29.png)



**process.nextTick**

process.nextTick 是一个独立于 eventLoop 的任务队列。在每一个 eventLoop 阶段完成后会去检查 nextTick 队列，如果里面有任务，会让这部分任务优先于微任务执行。例：

```javascript
setImmediate(() => {
    console.log('timeout1')
    Promise.resolve().then(() =>console.log('promise resolve'))
    process.nextTick(() =>console.log('next tick1'))
});
setImmediate(() => {
    console.log('timeout2')
    process.nextTick(() =>console.log('next tick2'))
});
setImmediate(() =>console.log('timeout3'));
setImmediate(() =>console.log('timeout4'));
```

* 在 node11 之前，因为每一个 eventLoop 阶段完成后会去检查 nextTick 队列，如果里面有任务，会让这部分任务优先于微任务执行，因此上述代码是先进入 check 阶段，执行所有 setImmediate，完成之后执行 nextTick 队列，最后执行微任务队列，因此输出为`timeout1=>timeout2=>timeout3=>timeout4=>next tick1=>next tick2=>promise resolve`
* 在 node11 之后，process.nextTick 是微任务的一种,因此上述代码是先进入 check 阶段，执行一个 setImmediate 宏任务，然后执行其微任务队列，再执行下一个宏任务及其微任务,因此输出为`timeout1=>next tick1=>promise resolve=>timeout2=>next tick2=>timeout3=>timeout4`

node事件循环阶段补充说明：

![](../../.gitbook/assets/image%20%283%29.png)

> * 定时器检测阶段\(timers\)：本阶段执行 timer 的回调，即 setTimeout、setInterval 里面的回调函数。
> * I/O事件回调阶段\(I/O callbacks\)：执行延迟到下一个循环迭代的 I/O 回调，即上一轮循环中未被执行的一些I/O回调。
> * 闲置阶段\(idle, prepare\)：仅系统内部使用。
> * 轮询阶段\(poll\)：检索新的 I/O 事件;执行与 I/O 相关的回调（几乎所有情况下，除了关闭的回调函数，那些由计时器和 setImmediate\(\) 调度的之外），其余情况 node 将在适当的时候在此阻塞。
> * 检查阶段\(check\)：setImmediate\(\) 回调函数在这里执行
> * 关闭事件回调阶段\(close callback\)：一些关闭的回调函数，如：socket.on\('close', ...\)。

