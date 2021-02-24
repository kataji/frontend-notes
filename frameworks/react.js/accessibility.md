# Accessibility

[https://reactjs.org/docs/accessibility.html](https://reactjs.org/docs/accessibility.html)

也称为A11y（中间11个字母，跟k8s一样，类似的还有i18n，internationalization）

主要的guideline有两家出品的一些规则：WCAG和WAI-ARIA。

要记得用semantic HTML，form之类的要好好label。要考虑只使用键盘来浏览和交互。

有一些工具，例如

* 开发： [eslint-plugin-jsx-a11y](https://github.com/evcohen/eslint-plugin-jsx-a11y) 
* 测试：[aXe](https://www.deque.com/products/axe/)， [aXe-core](https://github.com/dequelabs/axe-core)， [react-axe](https://github.com/dylanb/react-axe)

