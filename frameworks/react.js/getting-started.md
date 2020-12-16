# Getting Started

不多说啦，和vue一样，自称是可以progressive引入的，可以只用它给页面添加一点点小功能，也可以整成一个完整的app。

## 用\`&lt;script&gt;\`引入

[https://zh-hans.reactjs.org/docs/add-react-to-a-website.html](https://zh-hans.reactjs.org/docs/add-react-to-a-website.html)

### 引入React

> #### 步骤 2：添加 Script 标签 <a id="step-2-add-the-script-tags"></a>
>
> 接下来，在 `</body>` 结束标签之前，向 HTML 页面中添加三个 `<script>` 标签：
>
> ```markup
>   <!-- ... 其它 HTML ... -->
>
>   <!-- 加载 React。-->
>   <!-- 注意: 部署时，将 "development.js" 替换为 "production.min.js"。-->
>   <script src="https://unpkg.com/react@17/umd/react.development.js" crossorigin></script>
>   <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js" crossorigin></script>
>   <!-- 加载我们的 React 组件。-->
>   <script src="like_button.js"></script>
> </body>
> ```
>
> 前两个标签加载 React。第三个将加载你的组件代码。

其中组件代码里会定义一个react component，然后获取dom里的容器元素，然后调用render函数。

### 引入jsx

虽然官网也特别提醒jsx不是必须的啦，不过写react不写jsx不是没那味儿嘛。

简单demo可以直接用`script`引入babel代码，runtime时编译jsx。

```markup
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

不过性能肯定贼差啦。

正经可以在本地用babel编译完了再添加到HTML里面。比如这样：

```bash
npm install babel-cli@6 babel-preset-react-app@3
npx babel --watch src --out-dir . --presets react-app/prod
```

在项目的node\_modules里装了babel-cli和react的preset，然后启起来一个babel监听`src`目录下的代码变更，自动编译，输出放在`.`里面。

好的，今天的表演就到这里。下一回，create-react-app。

