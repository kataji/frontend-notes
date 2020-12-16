# 前端事件

事件处理

事件发生有三个阶段

![](../../.gitbook/assets/image%20%281%29.png)

这三个阶段必定发生。

可以用一个参数控制eventListener在捕获阶段还是冒泡阶段触发（会影响父子节点的eventListener的触发顺序；只能二选一）

旧版的api就是addEventListener\(event, listener, useCapture\)，其中useCapture是个boolean，默认false，也就是不用capture，而用冒泡，也就是按先子后父的顺序触发。设成true的话就是先父后子的顺序触发。

新的api的第三个参数可以是一个option object，里面可以设capture，once，passive（passive的不可以call preventDefault，是为了应对某些touch事件影响性能？比如移动端的用户touch滚动

