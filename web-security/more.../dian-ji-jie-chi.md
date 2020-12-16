# 点击劫持

有一个表面上显示的页面A，和一个实际接收交互的页面B。

页面A位于下层（比如用z-index），有内容按钮等。页面B位于上层，设为透明。当用户以为点击的是A中的按钮时，实际上点击的是B中的按钮。就可以做一些事…

尤其是用户登录了B，有B的权限的时候。被诱导去了A，然后A通过iframe引入B。

> **1）X-FRAME-OPTIONS**
>
> `X-FRAME-OPTIONS`是一个 HTTP 响应头，在现代浏览器有一个很好的支持。这个 HTTP 响应头 就是为了防御用 iframe 嵌套的点击劫持攻击。
>
> 该响应头有三个值可选，分别是
>
> * DENY，表示页面不允许通过 iframe 的方式展示
> * SAMEORIGIN，表示页面可以在相同域名下通过 iframe 的方式展示
> * ALLOW-FROM，表示页面可以在指定来源的 iframe 中展示
>
> **2）JavaScript 防御**
>
> 对于某些远古浏览器来说，并不能支持上面的这种方式，那我们只有通过 JS 的方式来防御点击劫持了。
>
> ```markup
> <head>
>   <style id="click-jack">
>     html {
>       display: none !important;
>     }
>   </style>
> </head>
> <body>
>   <script>
>     if (self == top) {
>       var style = document.getElementById('click-jack')
>       document.body.removeChild(style)
>     } else {
>       top.location = self.location
>     }
>   </script>
> </body>
> ```
>
> 以上代码的作用就是当通过 iframe 的方式加载页面时，攻击者的网页直接不显示所有内容了。
>
> 作者：浪里行舟  
> 链接：https://juejin.im/post/6844903772930441230  
> 来源：掘金  
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

