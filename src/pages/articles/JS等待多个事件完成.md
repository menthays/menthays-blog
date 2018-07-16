---
title: JS等待多个事件完成
date: "2018-07-16T16:40:20.101Z"
layout: post
draft: false
path: "/posts/wait-for-multi-events"
tags:
- "event"

description: "一组数据由多个事件的返回值组成，如何在数据全部到位时执行相应操作"
---

## 背景
最近遇到这样一个需求，用来在react/vue展示视图的一组数据，包含多个部分，每个部分是由不同事件返回的，如果每次事件触发时去改变数据，那么就会过于频繁的触发更新，当然我们可以通过 shouldComponentUpdate 来根据业务逻辑做 work-around，但是我需要一种适用性更广泛更普遍的办法。  
假设一个用户的数据由他的视频流(stream)和他的个人信息(userInfo)组成。

## 过程
由于 stream 和 userInfo 是由不同事件返回的，我们不能直接在某个事件中得到所有数据并 push 进一个数组从而更新视图，因此首先我们需要建立一种对象能够分次设置属性并在属性全部到位后触发相应动作，以及用来存储这些对象的map。

```javascript
const userList = {}
function addUser(uid, stream, info): void
function newUser(uid, callback): void
```
###首先实现 newUser
主要利用 Object.defineProperties 方法劫持数据的 set 和 get ，通过检测其余需要的值是否已就绪来决定是否触发回调事件

```javascript
function newUser(uid, callback) {
	let target = {
		uid,
		hasStream: false,
		hasInfo: false
	}
	Object.defineProperties(target, {
		info: {
		  set: function (val) {
		    this.accessInfo = val
		    if (val) {
		      this.hasInfo = true
		      if (this.hasStream) {
		        callback(this.uid, this.stream, this.info)
		      }
		    }
		  },
		  get: function () {
		    return this.accessInfo
		  }
		},
		stream: {
		  set: function (val) {
		    this.accessStream = val
		    if (val) {
		      this.hasStream = true
		      if (this.hasInfo) {
		        callback(this.uid, this.stream, this.info)
		      }
		    }
		  },
		  get: function () {
		    return this.accessStream
		  }
		}
	})
	return target
}
```

###其次实现 addUser
```javascript
const EventEmitter = require('event').EventEmitter
let emitter = new EventEmitter()
let callback = function(uid, stream, info) {
	// do something, for example, emit another event
	emitter.emit('user-ready', uid, stream, info)
	
}
function addUser(uid, stream, info) {
	if (!userList.hasOwnProperty(uid)) {
		userList[uid] = newUser(uid, callback)
	}

	let target = userList[uid]
	if (info) {
		target.info = info
	}
	if (stream) {
		target.stream = stream
	}
}
```

###最终监听两个事件并分别调用addUser
```javascript
someEmitter.on('stream-ready', (uid, stream) => {
	addUser(uid, stream, undefined)
})
someEmitter.on('info-ready', (uid, info) => {
	addUser(uid, undefined, info)
})
// deal with your custom event
emitter.on('user-ready', (uid, stream, info) => {
	// push obj to your array and update view
})
```



