---
layout: post
title: js运行机制
categories: Javascript
description: js运行机制
keywords: Javascript
---

`单线程` JavaScript语言的一大特点就是单线程,同一个时间只能做同一件事

**核心理解**

- 首先判断JS是同步还是异步,同步就进入主线程,异步就进入event table
- 异步任务在event table中注册函数,当满足触发条件后,被推入event queue
- 同步任务进入主线程后一直执行,直到主线程空闲时,才会去event queue中查看是否有可执行的异步任务,如果有就推入主线程中

1. 一般的JavaScript代码（同步）的属于宏任务，定时器相关的异步代码，包括setTimeOut、setInterval等也属于宏任务，promise、 process.nextTick属于微任务；

2. 同步的代码会按照执行顺序顺序执行，遇到异步代码的时候，属于宏任务的放到宏队列，微任务放到微队列，其中promise需要resolve或者reject才会执行then或者catch里面的内容，其他的放到队列的属于回调函数的内容。

3. 执行顺序是宏任务-微任务-宏任务……，因为整个脚本就是一个大的宏任务，所以当里面宏任务和微任务同时放入队列，会先执行玩微任务再执行宏任务；前提是主线程代码执行完毕，如果存在嵌套关系，则会先执行完该任务再执行下一个任务，如果问题复杂建议通过画图来理清楚

##### 参考文章
JavaScript 运行机制详解：再谈Event Loop
[http://www.ruanyifeng.com/blog/2014/10/event-loop.html](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)

深入理解JS引擎的执行机制
[https://segmentfault.com/a/1190000012806637](https://segmentfault.com/a/1190000012806637)

Tasks, microtasks, queues and schedules
[https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/?utm_source=html5weekly](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/?utm_source=html5weekly)