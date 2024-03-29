---
layout: post
title: js原型链
categories: Javascript
description: js原型链
keywords: Javascript
---

##### 原型链

> 在任意对象和Object.prototype之间，存在着一条以非标准属性__proto__进行连接的链，那我们管这条链叫做`原型链`

`原型`每个函数都有一个prototype属性,这个属性指向的就是原型对象. 实例有一个__proto__指向它构造函数的原型对象. <br/> 

`原型链`当调用一个对象的属性时，如果自身对象未找到，便会去对象的__proto__属性去查找，找到原型，原型也拥有__proto__属性，所以对继续向上查找，一直找到Object.prototype.__proto__ === null 

<br/> 

`特点`原型对象上的方法是被不同实例共有的, 当我们修改原型时，与之相关的对象也会继承这一改变。

```js
      // 第一种方式：字面量
      var o1 = {name: 'o1'};
      var o2 = new Object({name: 'o2'});

      // 第二种方式：构造函数
      var M = function (name) { this.name = name; };
      var o3 = new M('o3');
      
      // 第三种方式：Object.create
      var p = {name:'p'}
      var o4 = object.create(p)

      // new 运算符的工作原理
      var new1 = function(func) {
        var o = Object.create(func.prototype)
        var k = func.call(o)
        if(typeof k === 'object') {
          return k
        }
        else {
          return o
        }
      }
```
