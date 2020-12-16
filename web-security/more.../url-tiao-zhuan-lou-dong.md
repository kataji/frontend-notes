# URL跳转漏洞

利用可信网站的url处理漏洞？把恶意网址伪装成可信网址

例如

```text
http://gate.baidu.com/index?act=go&url=http://t.cn/RVTatrd
http://qt.qq.com/safecheck.html?flag=1&url=http://t.cn/RVTatrd
http://tieba.baidu.com/f/user/passport?jumpUrl=http://t.cn/RVTatrd
```

假如`http://gate.baidu.com/index`的服务器会傻傻地执行go [http://t.cn/RVTatrd，用户点击这个链接的时候，以为自己访问的是baidu，实际上访问了一个未知站点。](http://t.cn/RVTatrd，用户点击这个链接的时候，以为自己访问的是baidu，实际上访问了一个未知站点。)

## 防御

> **1\)referer的限制**
>
> 如果确定传递URL参数进入的来源，我们可以通过该方式实现安全限制，保证该URL的有效性，避免恶意用户自己生成跳转链接
>
> **2\)加入有效性验证Token**
>
> 我们保证所有生成的链接都是来自于我们可信域的，通过在生成的链接里加入用户不可控的Token对生成的链接进行校验，可以避免用户生成自己的恶意链接从而被利用，但是如果功能本身要求比较开放，可能导致有一定的限制。  
> 作者：浪里行舟  
> 链接：[https://juejin.im/post/6844903772930441230](https://juejin.im/post/6844903772930441230)  
> 来源：掘金  
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

