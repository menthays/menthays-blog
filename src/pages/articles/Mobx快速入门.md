---
title: Mobx 快速入门
date: "2018-03-15T22:40:32.169Z"
layout: post
draft: false
path: "/posts/mobx-quick-start"
category: "Quick Start"
tags:
- "react"
- "mobx"
- "flux"
description: "最快速地开始使用mobx进行状态管理"
---


## 背景
这篇文章旨在记录如何快速上手mobx，即最快地将 mobx ‘跑’起来，看了很多教程里介绍了太多概念性的东西看完还是不知道怎么组织代码，因此便有了这篇快速入门，深入内容和最佳实践将不被涉及，按需自行了解。

## 准备
首先使用npm安装mobx

```bash
npm i -D mobx
npm i -D mobx-react
```
然后推荐使用 ES6 Decorator语法，方便且干净不少。
如果使用了 create-react-app 可参考 [Rewire create-react-app to use MobX](https://github.com/timarney/react-app-rewired/tree/master/packages/react-app-rewire-mobx) 。
或者安装使用babel的拓展 mobx preset：

```bash
npm install --save-dev babel-preset-mobx
```
修改.babelrc

```json
{
  "presets": ["mobx"]
}
```
*[更多配置方法](https://mobx.js.org/best/decorators.html)*

## 开始
然后可以开始建立一个store对象了，新建store.js:

```javascript
import { observable, action } from 'mobx'
class Store {
	@observable state = ''
	@observable arr = []
	
	@action fn (param) {...}
	...
}
```
@observable装饰需要管理的‘状态’，比如这里如果修改state/arr，就和setState()类似，会触发视图层的变化。
@action装饰修改状态的‘动作’，因为我们不会在子组件中直接修改数据，而是通过action来维护管理。

然后在根组件里使用mobx-react的Provider将store注册到全局，并通过props继承下去。

```javascript
...
import { Provider } from 'mobx-react'
import Store from 'path/to/store'
...

class App extends Component {
	render () {
		return (
			<Provider Store={Store}>
				<Router>
					...
				<Router>
			<Provider>
		)
	}
}
```

接下来就可以在子组件中使用了，使用inject和observer来装饰子组件

```javascript
import { inject, observer } from 'mobx-react'

@inject('Store')
@observer
class Child extends Component {
	constructor (props) {
		super(props)
		this.store = props.store
	}
	
	render () {
		return (
			// now you can use state in store like this.state
			let children = this.store.arr.map(item => (
				<div>{item}</div>
			))
			<div>
				...
				{children}
			</div>
		)
	}
}
```

搞定。