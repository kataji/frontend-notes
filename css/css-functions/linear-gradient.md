# linear-gradient

[https://developer.mozilla.org/zh-CN/docs/Web/CSS/linear-gradient\(\)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/linear-gradient%28%29)

## 原理

linear-gradient会产生&lt;gradient&gt;数据类型（嗬，我竟然在css里讲起了数据类型，css啊你终于也长大了吗（不是））。&lt;gradient&gt;数据类型是&lt;image&gt;的一种，所以可以用在需要&lt;image&gt;的地方，比如background-image，或者background。

如下图，linear-gradient的定义主要有两个重要的点：1. 轴线；2. color-stop列表。

轴线差不多由角度定义（所以轴线一定要过中心？好像是吧…

color-stop由一个颜色和该颜色在轴线上的位置定义。过color-stop点和轴线垂直的一条线上的颜色都相同。起点和终点的位置如下图所示。

![](../../.gitbook/assets/image%20%287%29.png)

注：

* 其他image类型还包括静态类型（常见在url里被引用的）、其他函数计算所得image或者通过变量计算得到的image

## 语法

linear-gradient的语法：

```text
linear-gradient(
  [ <angle> | to <side-or-corner> ,]? <color-stop-list> )
  \---------------------------------/ \----------------------------/
    Definition of the gradient line        List of color stops

where <side-or-corner> = [ left | right ] || [ top | bottom ]
  and <color-stop-list> = [ <linear-color-stop> [, <color-hint>? ]? ]#, <linear-color-stop>
  and <linear-color-stop> = <color> [ <color-stop-length> ]?
  and <color-stop-length> = [ <percentage> | <length> ]{1,2}
  and <color-hint> = [ <percentage> | <length> ]
```

## 例子

```css
linear-gradient(0deg, red, yellow) // 顺时针角度，朝上是0deg
linear-gradient(red, yellow 40%, blue) // 40%位置是黄色，三色过渡
linear-gradient(red, yellow 20% 30%, blue) // 中间20%-30%位置是纯yellow
linear-gradient(red 10%, 70%, yellow 80%) // 中间色位于70%处，默认(10+80)/2=45；10%之前纯red，80%之后纯yellow
linear-gradient(red 50%, transparent 40%) // 一半的纯红，生硬线
```

