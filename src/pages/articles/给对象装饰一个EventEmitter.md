---
title: 给对象装饰EventEmitter——理解原型
date: "2018-03-17T05:09:59.811Z"
layout: post
draft: false
path: "/posts/decorate-event-emitter"
tags:
- "inherit"
- "prototype"
description: "为了解决callback引起的问题，需要为对象增加EventEmitter来实现事件机制。在这个过程中，针对不同情况的对象实现这一点，逐渐加深了对JS Prototype的理解。"
---

## 背景
为了解决callback引起的问题，需要为对象增加EventEmitter来实现事件机制。在这个过程中，逐渐加深了对 JS Prototype 的认识。

## 过程
### 对象已实例化
对于这种情况，可以想到直接给它增加一个子对象emitter，然后将事件相关的方法全部代理到这个子对象上.

```javascript
import EventEmitter from 'events'

const EventDecorator = (obj) => {
  // create myObj private child _emitter 
  obj._emitter = new EventEmitter()
  
  // record methods need proxy
  let methodArr = [
    'addListener', 'on', 'once', 'removeListener', 
    'removeAllListeners', 'setMaxListeners', 'listeners', 'emit'
  ]
  // do proxy
  for (let method of methodArr) {
    obj[method] = (...args) => obj._emitter[method](...args)
  }
}
```
做个测试

```javascript
let myObj = {
	callback: {}
}

EventDecorator(myObj)
// use event instead of callback
myObj.callback = (...args) => myObj.emit('callback', ...args)
// register two anoymous listeners
myObj.on('callback', val => {
  console.log('Callback: ' + val)
})
myObj.on('callback', val => {
  console.log('Another: ' + val)
})

myObj.callback('foo')
// Callback: foo
// Another: foo

```
这方法很正常也不容易有问题，那么接下来开始折腾之旅：首先，借用EventEmitter的构造函数，并且将a的原型链指向EventEmitter的原型

```javascript
EventEmitter.call(myObj)
Object.setPrototypeOf(myObj, events.EventEmitter.prototype)
```
尽管setPrototypeOf有许多别的问题，但是再次测试是成功的。不过这是a没有自己的原型方法的原因（准确的说，a的原型方法来自Object.prototype，而Object是一切的起点，所以即使改向EventEmitter也不影响），假如a的原型链并不直接指向Object.prototype而是有自己的原型方法呢。

```javascript
function MyClass() {
  this.name='haha'
}
MyClass.prototype = {
  say: function() {
    console.log(this.name)
  }
}
let myObj = new MyClass()
EventEmitter.call(myObj)
Object.setPrototypeOf(myObj, EventEmitter.prototype)
myObj.say() // not exsited
```
跑之前的测试通过了，但是a从A的原型上获得的方法say()方法不存在，因为现在a的原型链指向EventEmitter了。
### 对象已实例化，并且有自己的原型方法
因此对于这种情况，在借用构造函数之后，我们需要向a添加方法而不是覆盖它的原型

```javascript
EventEmitter.call(myObj)
Object.assign(myObj, EventEmitter.prototype)
myObj.say() // haha
myObj instanceOf MyClass // true
```
测试通过。a仍是由A实例化的，只不过拷贝了来自EventEmitter.prototype的方法和EventEmitter的属性和方法。

### 对象还未实例化
那么问题就转化为，在JavaScript中如何实现多重继承，方法很多，这边列举用Object.create方法实现

```javascript
function MyChildClass() {
  // constructor stealing
  EventEmitter.call(this)
  MyClass.call(this)
}
// inherit EventEmitter
MyChildClass.prototype = Object.create(EventEmitter.prototype)
// mix MyClass
Object.assign(MyChildClass.prototype, MyClass.prototype)
// redefine constructor
MyChildClass.prototype.constructor = MyChildClass

let myObj = new MyChildClass()
```

## 总结
只看第一种情况，其实这是个很简单的问题，不过正是后面这种折腾的过程，帮助加深对prototype的认识。