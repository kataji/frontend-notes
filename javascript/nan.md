# NaN

NaN意为Not a Number，是Number对象和window对象的属性，这俩是一样的。NaN的type是number。

一般不会直接用它，而是在各种奇怪的操作中被产生和返回，比如`parseInt('a')`, `0/0`（不过`1/0 === Infinity`）。

所以更常用的是`isNaN`函数，用于判断一个东西是不是NaN。这个函数也是Number和window对象的属性，高潮来了：这俩效果不一样🙂。

* Number.isNaN只有该值真的是NaN的时候才返回true；
* window.isNaN在该值不能被parse成正经number的时候返回true，比如：

`Number.isNaN('a') // false` vs `isNaN('a') // true` 。

太草了。

Note：

es3及之前可以对window.NaN赋值，es5之后改成not configurable了。即使能改也别去改它2333。

