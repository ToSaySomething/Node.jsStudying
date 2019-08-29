# [Node.js]学习之二 为什么要使用Node.js

## 1 前言

Node.js ，来自https://nodejs.org/en/：Node.js® is a JavaScript runtime built on [Chrome's V8 JavaScript engine](https://v8.dev/).，即Node.js并不是一门语言，要用js/ts进行编程，运行在Chrome浏览器平台的V8引擎上

## 2 单线程怎么实现

Node.js跑的时候是一个单线程的主进程，它不会去创建多线程而去解决多请求的问题。在学习和借鉴他人博客中，我根据我自己的理解，画了两个不太标准的图，望指正。

下图，主线程的执行栈的图可能画得不是特别对，好像是我自己对项目的理解，只针对于在IO和异步情况下发生的事件处理，栈调用方法没有异步的情况都想着在瞬间可以处理的，不做考虑。

![1567070698400](https://github.com/ToSaySomething/Node.jsStudying/raw/master/stack.png)

![1567069137943](https://github.com/ToSaySomething/Node.jsStudying/raw/master/all.png)

​		主线程跑的时候，类似C++的main跑下来有一个**stack**（执行栈），可能会调用其他的方法压入栈，运行完了之后再出栈，当客户端有多协议请求/异步（多IO请求）发到服务端时，主线程不会马上执行，只是会放入Event Queue （事件循环队列），然后进行Event Loop（事件循环），取出当前第一个事件，在线程池分配一个线程进行处理，以异步的方式将任务结果返回，再继续取出第二个直到事件处理完。

​		而且，主线程会主动去检查队列，当有事件处理完，会通知主线程，主线程回调，释放线程。

​		其中主要是**libuv**库（l是基于事件驱动的异步IO模型库，我们的js发出请求最后是由libuv完成，而所设置的回调则也是在libuv触发）调用它的API去从线程池分配线程处理。

​		最后来自这位大佬的动图https://blog.risingstack.com/node-js-at-scale-understanding-node-js-event-loop/

![The Node.js Event Loop - cat version](https://blog-assets.risingstack.com/2017/01/cat-node-js-event-loop-.gif)

参考链接：

Nodejs探秘：深入理解单线程实现高并发原理  https://www.sohu.com/a/248807106_495695

Nodejs的运行原理-架构篇 https://www.cnblogs.com/peiyu1988/p/8192066.html

Nodejs：单线程为什么能支持高并发？https://www.cnblogs.com/linzhanfly/p/9082895.html
