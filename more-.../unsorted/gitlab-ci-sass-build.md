# gitlab-ci sass build

今天pjc setup gitlab-ci的时候遇到卡在npm install的问题，看了下log好像是sass build卡住了。我记得我以前也遇到过这个问题…🤔不记得怎么hack的了，还是就疯狂重试？

> wd：在script里加一条
>
> `npm config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass -g`

妙啊

