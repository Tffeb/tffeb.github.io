---
layout: post
title: 关于闭包的理解
categories: Javascript
description: 关于闭包的理解
keywords: Javascript
---

##### 闭包应用场景

> `闭包函数定义` 声明在一个函数中的函数

> `闭包定义` 内部函数总是可以访问其所在的外部函数中声明的参数和变量,即使在其外部函数被返回了之后

 **特点**

 1. 让外部访问函数内部变量成为可能；
 2. 局部变量会常驻在内存中； 
 3. 可以避免使用全局变量，防止全局变量污染；
 4. 会造成内存泄漏(内存空间长期被占用，而不被释放)


Example 1

```js
function funA() {
  var a = 10; // funA的活动对象之中;
  return function() {
    //匿名函数的活动对象;
    alert(a);
  };
}
var b = funA();
b(); //10
```

Example 2

```js
function outerFn() {
  var i = 0;
  function innerFn() {
    i++;
    console.log(i);
  }
  return innerFn;
}
var inner = outerFn(); //每次外部函数执行的时候，外部函数的地址不同，都会重新创建一个新的地址
inner();
inner();
inner();
var inner2 = outerFn();
inner2();
inner2();
inner2(); //1 2 3 1 2 3
```

Example 3

```js
var i = 0;
function outerFn() {
  function innnerFn() {
    i++;
    console.log(i);
  }
  return innnerFn;
}
var inner1 = outerFn();
var inner2 = outerFn();
inner1();
inner2();
inner1();
inner2(); //1 2 3 4
```

Example 4

```js
function fn() {
  var a = 3;
  return function() {
    return ++a;
  };
}
alert(fn()()); //4
alert(fn()()); //4
```

Example 5

```js
function outerFn() {
  var i = 0;
  function innnerFn() {
    i++;
    console.log(i);
  }
  return innnerFn;
}
var inner1 = outerFn();
var inner2 = outerFn();
inner1();
inner2();
inner1();
inner2(); //1 1 2 2
```

Example 6

```js
function outerFn() {
  var i = 0;
  function innnerFn() {
    i++;
    console.log(i);
  }
  return innnerFn;
}
var inner1 = outerFn();
var inner2 = outerFn();
inner1();
inner2();
inner1();
inner2(); //1 1 2 2
```

Example 7

```js
(function() {
  var m = 0;
  function getM() {
    return m;
  }
  function seta(val) {
    m = val;
  }
  window.g = getM;
  window.f = seta;
})();
f(100);
console.info(g()); //100  闭包找到的是同一地址中父级函数中对应变量最终的值
```

Example 8

```js
function a() {
  var i = 0;
  function b() {
    alert(++i);
  }
  return b;
}
var c = a();
c(); //1
c(); //2
```

Example 9

```js
var add = function(x) {
  var sum = 1;
  var tmp = function(x) {
    sum = sum + x;
    return tmp;
  };
  tmp.toString = function() {
    return sum;
  };
  return tmp;
};
alert(add(1)(2)(3)); //6
```

Example 10

```js
var lis = document.getElementsByTagName("li");
for (var i = 0; i < lis.length; i++) {
  (function(i) {
    lis[i].onclick = function() {
      console.log(i);
    };
  })(i); //事件处理函数中闭包的写法
}
```

Example 11

```js
function m1() {
  var x = 1;
  return function() {
    console.log(++x);
  };
}
m1()(); //2
m1()(); //2
m1()(); //2
var m2 = m1();
m2(); //2
m2(); //3
m2(); //4
```

Example 12

```js
var  fn=(function(){
   var  i=10;
   function  fn(){
      console.log(++i);
   }
   return   fn;
})() 
fn();   //11
fn();   //12
```

Example 13

```js
function love1(){
     var num = 223;
     var me1 = function() {
           console.log(num);
     }
     num++;
     return me1;
}
var loveme1 = love1();
loveme1();   //输出224
```

Example 14

```js
function fun(n,o) {
    console.log(o);
    return {
         fun:function(m) {
               return fun(m,n);
         }
    };
}
var a = fun(0);  //undefined
a.fun(1);  //0  
a.fun(2);  //0  
a.fun(3);  //0  
var b = fun(0).fun(1).fun(2).fun(3);   //undefined  0  1  2
var c = fun(0).fun(1);  
c.fun(2);  
c.fun(3);  //undefined  0  1  1
```

Example 15

```js
function fn(){
   var arr = [];
   for(var i = 0;i < 5;i ++){
	 arr[i] = function(){
		 return i;
	 }
   }
   return arr;
}
var list = fn();
for(var i = 0,len = list.length;i < len ; i ++){
   console.log(list[i]());
}  //5 5 5 5 5
```

Example 16

```js
function fn(){
  var arr = [];
  for(var i = 0;i < 5;i ++){
	arr[i] = (function(i){
		return function (){
			return i;
		};
	})(i);
  }
  return arr;
}
var list = fn();
for(var i = 0,len = list.length;i < len ; i ++){
  console.log(list[i]());
}  //0 1 2 3 4
```