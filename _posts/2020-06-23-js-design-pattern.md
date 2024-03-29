---
layout: post
title: 简单记录下JavaScript设计模式
categories: 设计模式
description: 简单记录下JavaScript设计模式
keywords: 设计模式
---

#### 单例模式 

单例模式保证类只有一个实例，并提供一个访问它的全局访问点


Example:

```js
 function getSingle(fn){
    let result

    return function (){
        return result || (result=fn.apply(this,arguments))
    }

  }
```

#### 策略模式

 解决一个问题的多个方法，将每种方法独立封装起来，相互可以替换 <br/>
  一个基于策略模式的程序至少由两部分组成，一个是一组策略类，策略类封装了具体的算法，并负责具体的计算过程，一个是环境类，环境类接受客户的请求，随后把请求委托给某个策略类

  <h4>策略模式的一个使用场景：表单验证，将不同验证规则封装成一组策略，避免了多重条件判断语句，一句经典的话: 在函数作为一等对象的语言中，策略模式是隐性的，策略就是值为函数的变量</h4>

Example:

```js
    const S = (salary)=>{
        return salary * 4
    }
    const A = (salary)=>{
        return salary * 3
    }
    const B = (salary)=>{
        return salary * 2
    }
    const calculate = (fun,salary)=>{
        return fun(salary)
    }
    calculate(S,1000)
```

#### 代理模式

 代理模式为一个对象提供一个代用品或占位符，以便控制对它的访问<br/>
  不直接和本体进行交互，而是在中间加入一层代理，代理来处理一些不需要本体做的操作

```js
var myImage=function(){
    var imgNode=document.createElement('img')
    document.body.appendChild(imgNode)
    return {
        setImg(src){
            imgNode.src=src
        }
    }
}

var proxyImg=function(){
    var img =new Image()
    img.onload=function(){
        myImage.setSrc(this.src)
    }
    return {
        setImg(src){
            myImage.setSrc(‘loading.png’)
            img.src=src      
        }
    }
}
```

#### 观察者和发布订阅模式

观察者和发布、订阅模式使程序的两部分不必紧密耦合在一起，而是通过通知的方式来通信

**观察者模式**

 一个对象维持一系列依赖于它的对象，当对象状态发生改变时主动通知这些依赖对象

```js
class Subject{
        constructor(){
            this.observers=[]
        }
        add(observer){
            this.observers.push(observer)
        }
        notify(data){
            for(let observer of this.observers){
                observer.update(data)
            }
        }
    }
    class Observer{
        update(){
        }
    }
```

观察者模式的缺点是对象必须自己维护一个观察者列表，当对象状态有更新时，直接调用其他对象的方法，所以，在使用中，我们一般采用一种变形方式，即发布订阅模式

**发布订阅模式**

 该模式在主题和观察者之间加入一层管道，使得主题和观察者不直接交互，发布者将内容发布到管道，订阅者订阅管道里的内容，目的是避免订阅者和发布者之间产生依赖关系

```js
class Pubsub{

        constuctor(){
            this.pubsub={}
            this.subId=-1
        }

        publish(topic,data){
            if(!this.pubsub[topic]) return
            const subs=this.pubsub[topic]
            const len=subs.length
            while(len--){
                subs[len].update(topic,data)
            }
        }

        /**
        *  topic {string}
        *  update {function}
        */
        subscribe(topic,update){
            !this.pubsub[topic] && (this.pubsub[topic]=[])
            this.subId++
            this.pubsub[topic].push({
                token:this.subId,
                update
            })
        }

        unsubscribe(token){
            for(let topic in this.pubsub){
                if(this.pubsub.hasOwnProperty(topic)){
                    const current=this.pubsub[topic]
                    for(let i=0,j=current.length;i<j;i++){
                        if(current[i].token==token){
                            current.splice(i,1)
                            return token
                        }
                    }
                }
            }
            return this
        }

    }
```

#### 命令模式
> 命令模式的命令指的是一个执行某些特定事情的指令
命令模式最常见的使用场景是：有时候需要向某些对象发送请求，但是不知道请求的接受者是谁，也不知道被请求的操作是什么。此时希望用一种松耦合的方式来设计程序，是使得请求发送者和接受者消除彼此之间的耦合关系


### 几大原则

**单一职责原则SRP**   

  一个类只负责一个功能领域的相应职责，即就一个类而言，应该只有一个引起它变化的原因。
单一职责原则是实现高内聚、低耦合的指导方针，它是最简单但又最难运用的原则，需要设计人员发现类的不同职责并将其分离，而发现类的多重职责需要设计人员具有较强的分析设计能力和相关实践经验。

**开闭原则OCP**

一个软件实体应当对扩展开放，对修改关闭。即软件实体应尽量在不修改原有代码的情况下进行扩展。
在开闭原则的定义中，软件实体可以指一个软件模块、一个由多个类组成的局部结构或一个独立的类

**里氏替换原则LSP**

所有引用父类的地方必须能透明地使用其子类的对象。
里氏替换原则告诉我们：在软件中将一个基类对象替换成它的子类对象，程序将不会产生任何错误和异常，反过来则不成立，如果一个软件实体使用的是一个子类对象，它不一定能够使用基类对象

**依赖倒置原则DIP**

抽象不应该依赖于细节，细节应当依赖于抽象，要针对接口编程，而不是针对实现编程。

**接口隔离原则ISP**

使用多个专门的接口，而不使用单一的总接口，即客户端不应该依赖那些它不需要的接口。
根据接口隔离原则，当一个接口太大时，我们需要将它分割成一些更细小的接口，使用该接口的客户端仅需知道与之相关的方法即可。每一个接口应该承担一种相对独立的角色，不干不该干的事，该干的事都要干。

**迪米特法则LoD**

一个软件实体应当尽可能少地与其他实体发生相互作用。
如果一个系统符合迪米特法则，那么当其中某一个模块发生修改时，就会尽量少地影响其他模块，扩展会相对容易，这是对软件实体之间通信的限制，迪米特法则要求限制软件实体之间通信的宽度和深度。迪米特法则可降低系统的耦合度，使类与类之间保持松散的耦合关系。