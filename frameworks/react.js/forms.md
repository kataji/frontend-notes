# Forms

[https://reactjs.org/docs/forms.html](https://reactjs.org/docs/forms.html)

React的form元素常常用controlled component，就是状态存在react component的state里，输入和submit的时候都另外写handler，渲染时绑定到相应props上，比如input的value。

"Single source of truth."

注：

* React里textarea也用value prop渲染内容，而不是在tag之间嵌文本。
* select的selected option不在option元素上放selected attribute，而在select元素上放value attribute。

> This is more convenient in a controlled component because you only need to update it in one place.

* &lt;input type="file" /&gt; 是readonly的，所以是[uncontrolled](https://reactjs.org/docs/uncontrolled-components.html)。

React推荐Form库： [Formik](https://jaredpalmer.com/formik)

