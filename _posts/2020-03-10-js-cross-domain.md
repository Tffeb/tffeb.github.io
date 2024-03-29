---
layout: post
title: 前端跨域通信的几种方式
categories: Javascript
description: js 跨域通信的几种方式
keywords: Javascript
---


#### jsonp、hash、postMessage、WebSocket

`jsonp`

```js
    <script src="http://www.abc.com/?data=name&callback=jsonp" charset="utf-8"></script> 
    // 通过callback的jsonp函数获取数据
    <script type="text/javascript">
     function jsonp(data) {
         console.log(data)
     }
    </script>
```

`hash`

```js
      //场景是当前页面 A 通过iframe或frame嵌入了跨域的页面 B
      // 在A中伪代码如下：
      var B = document.getElementsByTagName('iframe');
      B.src = B.src + '#' + 'data';
      // 在B中的伪代码如下
      window.onhashchange = function () {
          var data = window.location.hash;
      };
```

`postMessage`

```js
      // 窗口A(http:A.com)向跨域的窗口B(http:B.com)发送信息
      Bwindow.postMessage('data', 'http://B.com');
      // 在窗口B中监听
      Awindow.addEventListener('message', function (event) {
          console.log(event.origin);
          console.log(event.source);
          console.log(event.data);
      }, false);
```

`WebSocket`

```js
      var ws = new WebSocket('ws://echo.websocket.org');
      ws.onopen = function (evt) {
          console.log('Connection open ...');
          ws.send('Hello WebSockets!');
      };
      ws.onmessage = function (evt) {
          console.log('Received Message: ', evt.data);
          ws.close();
      };
      ws.onclose = function (evt) {
          console.log('Connection closed.');
      };
```

`CORS`

```js
      // url（必选），options（可选）
      fetch('/some/url/', {
          method: 'get',
      }).then(function (response) {

      }).catch(function (err) {
        // 出错了，等价于 then 的第二个参数，但这样更好用更直观
      });
```