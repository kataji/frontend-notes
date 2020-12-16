# Listen to DOM element resize

一个是用MutationObserver，好像可以，不过会听到多余的attributes change，需要自己在callback里判断。这个是living standard。

有一个专用的ResizeObserver，可以专用地听resize，还能很精致地听content-box resize, border-box resize，或者svg元素的bounding-box resize。不过还是Editor's Draft状态。

StackOverflow上有一个评价很好的答案：[https://stackoverflow.com/a/19418065](https://stackoverflow.com/a/19418065) ，不过我没试过，据说是用了css-element-query，一个很smart的高效的不loop的方法。

