# Promise

{% embed url="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using\_promises" %}

## Promise

`Promise`是js在浏览器和node里都有的内置对象, 它的出现好像是为了解决这么一个问题:

```javascript
doFirstThing(params, function handleSuccess(result1) {
  doSecondThing(result1, function (result2) {
    doThirdThing(result2, function (result3) {
      doFourthThing(result3, ...)
    }, function (error3){ handleError3(error3) })
  }, function (error2) { handleError2(error2) })
}, function handleFail(error1) { throw new Error(error) })
```

这种需要等一件事情做完再做另一件事情的情况中, 古早方法会通过传入回调函数解决, 等待链路长的时候就会变成疯狂嵌套的回调. 还有那些错误处理的handler, 也要被传入好几次, 很多情况下其实只要链路中出了任何一个问题, 统一处理就好了.

用promise的话就可以把事情flatten, 用chain的方式去做"等一件事完了做下一件", 并在最后统一处理any错误.

```javascript
doFirstThing(params)
.then(function (result1) {
  return doSecondThing(result1)
})
.then(function (result2){
  return doThirdThing(result2)
})
.then((result3) => doFourthThing(result3))
.catch(function (error) {
  handleError(error)
})
```

其中每个`.then`其实也能传入successHandler和failHandler两个函数, 在promise被resolve或者reject的时候分别调用. `.then`会return handler 产生并return的新的promise, 从而实现chaining.

 `.catch`那段其实就相当于`.then(null, function handleError(error) {})`, 所以如果在handleError函数里继续return某promise, 还能继续chain下去.

用async/await语法糖可以把事情写得更漂亮一点, 它本质上就是promise.

```javascript
// in async function
try {
  const result1 = await doFirstThing(params)
  const result2 = await doSecondThing(result1)
  const result3 = await doThirdThing(result2)
  // ...
} catch (error) {
  handleError(error)
}
```

## 一些trick

### parallel跑promise

用Promise.all. 会生成一个新promise, 只有全部resolve后才会resolve, 有一个reject就是reject, 不然就pending. resolve结果是所有结果的数组, reject是那一个reject的结果.

```javascript
// 假设有func1, func2, func3三个会返回promise的函数
const arr = [func1, func2, func3]
Promise.all(arr.map(func => func())) // 生成了一个新promise
.then(results => {
  // do things to [res1, res2, res3]
})
// 记得catch
```

也可以用Promise.allSettled. 会生成一个新promise, 会好好的等所有promise settle \(resolve or reject\). 

还有一个诡异的Promise.any, 是Promise.all的真正反义词. 有一个resolve就resolve, 只有全部reject才会reject, 不然就pending. 和race的差别是一个reject之后不会reject. 

### race跑promise

用Promise.race. 也是生成一个新promise, 有一个resolve就resolve, 一个reject就reject, 全部都pending那就pending.

```javascript
// 假设有func1, func2, func3三个会返回promise的函数
const arr = [func1, func2, func3]
Promise.race(arr.map(func => func()))  // 生成了一个新promise
.then(result => {
  // 最早resolve的那个result
})
// 记得catch
```

### 一个接一个跑promise

```javascript
// 假设有func1, func2, func3三个会返回promise的函数, 后一个以前一个的resolve结果作参数
const arr = [func1, func2, func3]
arr.reduce((prev, cur) => {
  return prev.then(cur)
}, Promise.resolve(initParams))

// or
let result = initParams
for (const func of arr) {
  result = await func(result)
}
// eslint 有个rule是no await in loop
// 不过它有不适用的场景, 即promise之间不independent的情况, 比如这里就是
// 此时官方推荐用disable comment的方式给它disable掉
```

### 在chain的最后cleanUp

`.finally`, 不管resolve还是reject都会执行. 和`.then(onFinally, onFinally)`类似, 不过

* 它deliberately不接受入参,
* 而且除非在里面throw或者return rejected promise, 只会return和上一个promise状态和值都一样的promise \(虽然不是同一个\).

```javascript
doSomething()
.then(...)
.catch(...)
.finally(() => {
  isLoading = false
})
```

### 消灭古早callback

```javascript
// 比如包一包某些setTimeout
const wait = (delay) => new Promise((resolve) => setTimeout(resolve(), delay))

// 然后
setTimeout(func, 500)
// 就能改写成
await wait(500)
func()
```

## 手工产生promise

按照mdn的说法, 美好愿景是一般人都只要用会返回promise的函数, 不用自己手动创建promise. anyway还是可以做的.

比如用Constructor

```javascript
new Promise((resolve, reject) => {
  try {
    // dosomething
    resolve(result)
  } catch (error) {
    reject(error)
  }
})
```

或者还有俩简便方法

```javascript
Promise.resolve(value)
// 相当于
new Promise((resolve, reject) => {
  resolve(value)
})
// 类似的还有Promise.reject(value)
```

## Common Mistakes

好像主要有三种

1. 不必要的嵌套
2. 忘记return promise
3. 忘记catch

对2.: handler函数得继续return新的promise, 才能在后面接`.then`的chain, 达到依次执行的效果. 无返回的话会自动返回包成`Promise.resolve(undefined)`, 虽然也能在后面接`.then`, 但是注意链已经断了, 后续的chain等的不是在handler里产生的Promise, 而是一个不相关的Promise.resolve\(undefined\). 要让后续的chain接上, 必须返回**要被等的**那个promise, 可以真的返回promise, 或者返回一个非promise的value, 会自动包成`Promise.resolve(value)`. 

## Promise的所有方法

* Promise.all\(\)
* Promise.allSettled\(\)
* Promise.any\(\)
* Promise.prototype.catch\(\)
* Promise.prototype.finally\(\)
* Promise.prototype.then\(\)
* Promise.race\(\)
* Promise.reject\(\)
* Promise.resolve\(\)

