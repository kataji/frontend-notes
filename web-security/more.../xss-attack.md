---
description: Cross Site Scripting Attack
---

# XSS Attack

* [为什么cookie中有httpOnly属性-前端进阶之旅-微信公众号](https://mp.weixin.qq.com/s?__biz=MzA4MjA1MDM3Ng==&mid=2450810812&idx=1&sn=61efbef818ee174585c638b9e6491505&chksm=886b6b9bbf1ce28dd2db16771baa23c38b10b989d06d354de85c3d608c86b055611fc4798a2c&scene=0&xtrack=1&key=221452a4d6b5ef377e316dccd1b5ebf9b7994f024699d638428256c146489ca07042f7a0566a0f2e5f68803ac168d2e2c83ee910f26a5d34f42a77f9ff1b0b5a595d44b918dc8632c050d76cb1d97038c6eda3084da88dded4fd211c04fa3da65cacff8b603c8e1c320b626e94711d94a568b34f589e00aaefd5160d490c96de&ascene=1&uin=MTI2ODU0NDIwMQ%3D%3D&devicetype=Windows+10+x64&version=62090529&lang=zh_CN&exportkey=ASAm6xt78tRSO4HiXNzPbZc%3D&pass_ticket=%2FVpJDVgkqW83zuRca%2Bg52%2BWef1AzxKMGfRTl1Jb5rPyXlFWnMiW3wf%2F2KjPZvizc&wx_header=0)
* [常见六大Web安全攻防解析](https://juejin.im/post/6844903772930441230) _\*\*_

> 这就需要在安全和自由之间找到一个平衡点，所以我们默认页面中可以引用任意第三方资源，然后又引入 CSP 策略来加以限制；默认 XMLHttpRequest 和 Fetch 不能跨站请求资源，然后又通过 CORS 策略来支持其跨域。
>
> 不过支持页面中的第三方资源引用和 CORS 也带来了很多安全问题，其中最典型的就是 XSS 攻击。

## 什么是XSS攻击

即Cross Site Scripting攻击，为了与CSS区分，所以叫XSS攻击（到了现在其实不仅是Cross Site了。

就是往HTML页面或者DOM里注入恶意脚本，用户浏览页面时，浏览器不能区分正常脚本和恶意脚本，以一样的权限执行，达成攻击的效果。

例如：

* 通过`document.cookie`拿到cookie，发送给攻击者的服务器，攻击者获取cookie之后就能在其他电脑上模拟用户登录状态，进行一些操作
* 通过`addEventListener`添加监听器监听键盘输入，窃取用户名密码发送给攻击者的服务器
* 修改DOM伪造表单，欺骗用户输入用户名密码等，然后发送给攻击者的服务器
* 篡改DOM显示恶意内容

## 恶意脚本注入方式

### 存储型（持久型）XSS攻击

攻击者利用服务器漏洞，在服务器端存储了恶意脚本，用户正常访问，结果拿到了嵌入了恶意脚本的页面。

例：

喜马拉雅FM可以自定义专辑并分享，其中可以自定义专辑名称（字符串）。有攻击者在这个字符串里写入恶意脚本，保存，然后分享给其他用户。用户点开这个专辑，浏览器尝试渲染这个专辑名字的时候就会执行这个脚本，会窃取cookie信息。

> 攻击成功需要同时满足以下几个条件：
>
> * POST 请求提交表单后端没做转义直接入库。
> * 后端从数据库中取出数据没做转义直接输出给前端。
> * 前端拿到后端数据没做转义直接渲染成 DOM。
>
> 持久型 XSS 有以下几个特点：
>
> * 持久性，植入在数据库中
> * 盗取用户敏感私密信息
> * 危害面广
>
> 链接：[https://juejin.im/post/6844903772930441230](https://juejin.im/post/6844903772930441230)

### 反射型（非持久型）XSS攻击

恶意代码somehow由用户发送给服务器，服务器再返回，并由浏览器执行。

恶意代码是请求的带的参数或数据，所请求的页面依赖这个参数或数据，会把它里面的字符串嵌到页面里。

例：

诱导点击的链接，其中包含一个参数是恶意脚本。用户点击后访问的页面就含恶意脚本。

> 非持久型 XSS 漏洞攻击有以下几点特征：
>
> * 即时性，不经过服务器存储，直接通过 HTTP 的 GET 和 POST 请求就能完成一次攻击，拿到用户隐私数据。
> * 攻击者需要诱骗点击,必须要通过用户点击链接才能发起
> * 反馈率低，所以较难发现和响应修复
> * 盗取用户敏感保密信息
>
> 链接：[https://juejin.im/post/6844903772930441230](https://juejin.im/post/6844903772930441230)

### 基于DOM的XSS攻击

？

## 防御XSS攻击

* 不要执行来源不明的字符串！
  * 不要把字符串嵌入DOM渲染
  * 不要去`eval`，以及`new Function()`，`document.write()`，`document.writeln()`，`window.setInterval()`，`window.setTimeout()`，`innerHTML`，`document.createElement()`
* escape转义，例如 &lt;script&gt; （有个js-xss包）
* [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy)，可以设置 `default-src`， `img-src`，`media-src`，`script-src`等的白名单，限制可以加载的资源的来源
  * > 实施严格的 CSP 可以有效地防范 XSS 攻击，具体来讲 CSP 有如下几个功能：
    >
    > * 限制加载其他域下的资源文件，这样即使黑客插入了一个 JavaScript 文件，这个 JavaScript 文件也是无法被加载的；
    > * 禁止向第三方域提交数据，这样用户数据也不会外泄；
    > * 禁止执行内联脚本和未授权的脚本；
    > * 还提供了上报机制，这样可以帮助我们尽快发现有哪些 XSS 攻击，以便尽快修复问题
    >
    > [为什么cookie中有httpOnly属性-前端进阶之旅-微信公众号](https://mp.weixin.qq.com/s?__biz=MzA4MjA1MDM3Ng==&mid=2450810812&idx=1&sn=61efbef818ee174585c638b9e6491505&chksm=886b6b9bbf1ce28dd2db16771baa23c38b10b989d06d354de85c3d608c86b055611fc4798a2c&scene=0&xtrack=1&key=221452a4d6b5ef377e316dccd1b5ebf9b7994f024699d638428256c146489ca07042f7a0566a0f2e5f68803ac168d2e2c83ee910f26a5d34f42a77f9ff1b0b5a595d44b918dc8632c050d76cb1d97038c6eda3084da88dded4fd211c04fa3da65cacff8b603c8e1c320b626e94711d94a568b34f589e00aaefd5160d490c96de&ascene=1&uin=MTI2ODU0NDIwMQ%3D%3D&devicetype=Windows+10+x64&version=62090529&lang=zh_CN&exportkey=ASAm6xt78tRSO4HiXNzPbZc%3D&pass_ticket=%2FVpJDVgkqW83zuRca%2Bg52%2BWef1AzxKMGfRTl1Jb5rPyXlFWnMiW3wf%2F2KjPZvizc&wx_header=0)
  * 开启CSP可以通过服务器返回头，或网页meta标签
    * `<meta http-equiv="Content-Security-Policy" content="default-src https:">`
* cookie HttpOnly：服务器响应头set-cookie的一个属性，可以防止js读cookie

