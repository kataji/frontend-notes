# OS Injection

也是误信了用户提交的参数，把它拼接到shell指令里面，结果造成了意想不到的后果。

比如服务器读取用户参数`repo`，执行

```javascript
`git clone ${repo} /some/path`
```

万一`repo`是这么个字符串 `https://github.com/xx/xx.git && rm -rf /* &&`且正好有root权限，就完了👍。

## 防御

> * 后端对前端提交内容进行规则限制（比如正则表达式）。
> * 在调用系统命令前对所有传入参数进行命令行参数转义过滤。
> * 不要直接拼接命令语句，借助一些工具做拼接、转义预处理，例如 Node.js 的 `shell-escape npm`包

