# Vue.js

### Life Cycle

new了一个新的Vue对象，里面只有些默认玩意儿比如生命周期函数和默认事件，data和methods都是空的（？

#### beforeCreate

data和methods之类的有了

#### created

模板编译好了，在内存中渲染成DOM了，但还没挂到页面上

#### beforeMount

挂到页面上了，data和view达成了同步

#### mounted

data更新了，view还没更新

#### beforeUpdate

通过内存中的DOM，diff，更新了DOM，data和view又同步了

#### updated

调用`vm.$destroy()`后，啥都还可用？

#### beforeDestroy

已经完全销毁，啥都不可用了= =？

#### destroyed

### Parent-Child Life Cycle

> `父beforeCreate`-&gt;`父created`-&gt;`父beforeMount`-&gt;`子beforeCreate`-&gt;`子created`-&gt;`子beforeMount`- &gt;`子mounted`-&gt;`父mounted`

> `父beforeUpdate`-&gt;`子beforeUpdate`-&gt;`子updated`-&gt;`父updated`

### Diff

> #### 42 既然Vue通过数据劫持可以精准探测数据变化,为什么还需要虚拟DOM进行diff检测差异?
>
> > 现代前端框架有两种方式侦测变化,一种是pull一种是push
>
> * pull: 其代表为React,我们可以回忆一下React是如何侦测到变化的,我们通常会用setStateAPI显式更新,然后React会进行一层层的Virtual Dom Diff操作找出差异,然后Patch到DOM上,React从一开始就不知道到底是哪发生了变化,只是知道「有变化了」,然后再进行比较暴力的Diff操作查找「哪发生变化了」，另外一个代表就是Angular的脏检查操作。
> * push: Vue的响应式系统则是push的代表,当Vue程序初始化的时候就会对数据data进行依赖的收集,一但数据发生变化,响应式系统就会立刻得知,因此Vue是一开始就知道是「在哪发生变化了」,但是这又会产生一个问题,如果你熟悉Vue的响应式系统就知道,通常一个绑定一个数据就需要一个Watcher,一但我们的绑定细粒度过高就会产生大量的Watcher,这会带来内存以及依赖追踪的开销,而细粒度过低会无法精准侦测变化,因此Vue的设计是选择中等细粒度的方案,在组件级别进行push侦测的方式,也就是那套响应式系统,通常我们会第一时间侦测到发生变化的组件,然后在组件内部进行Virtual Dom Diff获取更加具体的差异,而Virtual Dom Diff则是pull操作,Vue是push+pull结合的方式进行变化侦测的
>
> [梳理Vue常考面试题型](https://mp.weixin.qq.com/s?__biz=MzA4MjA1MDM3Ng==&mid=2450810634&idx=1&sn=528dbceaec62241d94fb0b439fc42aae&chksm=886b6b2dbf1ce23bf8a07e075ca933da4599e214bea41f3c8541a20bf4f904f6e43800d17fc0&scene=126&sessionid=1603091150&key=221452a4d6b5ef370a0957a66478e9dc00ec458f727160703b401ef7096c945db5037011bf9a9694fc9d6777a62bea6cd35759e1357cef09bd001929828856b00384ce7555db8444c64a37773a4594e52d7c2f72ae56e1527491b6d5c973b49c3f146011f634808ca60b486b9d3776eb8457a600b1d109b1e0a17ff426503a58&ascene=1&uin=MTI2ODU0NDIwMQ%3D%3D&devicetype=Windows+10+x64&version=62090529&lang=zh_CN&exportkey=AaZX4TYHEwAxhQHShRgNyLM%3D&pass_ticket=%2FVpJDVgkqW83zuRca%2Bg52%2BWef1AzxKMGfRTl1Jb5rPyXlFWnMiW3wf%2F2KjPZvizc&wx_header=0)

### Scoped Style

> #### 31 scoped样式穿透
>
> > `scoped`虽然避免了组件间样式污染，但是很多时候我们需要修改组件中的某个样式，但是又不想去除`scoped`属性
>
> 1. 使用`/deep/`
>
> ```markup
> //Parent
> <template>
> <div class="wrap">
>     <Child />
> </div>
> </template>
>
> <style lang="scss" scoped>
> .wrap /deep/ .box{
>     background: red;
> }
> </style>
>
> //Child
> <template>
>     <div class="box"></div>
> </template>
> ```
>
> 2. 使用两个style标签
>
> ```markup
> //Parent
> <template>
> <div class="wrap">
>     <Child />
> </div>
> </template>
>
> <style lang="scss" scoped>
> //其他样式
> </style>
> <style lang="scss">
> .wrap .box{
>     background: red;
> }
> </style>
>
> //Child
> <template>
>     <div class="box"></div>
> </template>
> ```
>
> [梳理Vue常考面试题型](https://mp.weixin.qq.com/s?__biz=MzA4MjA1MDM3Ng==&mid=2450810634&idx=1&sn=528dbceaec62241d94fb0b439fc42aae&chksm=886b6b2dbf1ce23bf8a07e075ca933da4599e214bea41f3c8541a20bf4f904f6e43800d17fc0&scene=126&sessionid=1603091150&key=221452a4d6b5ef370a0957a66478e9dc00ec458f727160703b401ef7096c945db5037011bf9a9694fc9d6777a62bea6cd35759e1357cef09bd001929828856b00384ce7555db8444c64a37773a4594e52d7c2f72ae56e1527491b6d5c973b49c3f146011f634808ca60b486b9d3776eb8457a600b1d109b1e0a17ff426503a58&ascene=1&uin=MTI2ODU0NDIwMQ%3D%3D&devicetype=Windows+10+x64&version=62090529&lang=zh_CN&exportkey=AaZX4TYHEwAxhQHShRgNyLM%3D&pass_ticket=%2FVpJDVgkqW83zuRca%2Bg52%2BWef1AzxKMGfRTl1Jb5rPyXlFWnMiW3wf%2F2KjPZvizc&wx_header=0)

### computed和watch的触发时机

假设有data property `propA`，watch `propA`的watchA，有依赖`propA`的`compA`

当propA改变时，只会触发watchA，不会触发compA；除非compA被读，则会在那时调用computed的getter。

有watch`compA`的watchCompA，则一开始的时候就会先调一下compA的getter（应该是把它赋给了watchCompA的oldValue？）

当propA改变时，会先触发watchA，然后调用compA的getter，然后触发watchCompA。

