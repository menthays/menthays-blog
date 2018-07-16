---
title: Babel常用选项说明
date: "2018-06-23"
layout: post
draft: false
path: "/posts/babel-option-introduction"
tags:
- "babel"

description: "简要介绍babel的常用选项"
---
### 为什么使用 babel-preset-env
babel-preset-2015 包含全部语法转化插件，实际情况不需要（浏览器，其余选项），根据需要配置 babel-preset-env 避免全部引用。 

#### target 选项
只需支持chrome59

```javascript
"presets": [
	[
		"env",
		{
			"targets": [
				"chrome": 59
			]
		} 
	]
]
```

所有浏览器的最新两个版本且ie>=8

```javascript
"browsers": ["last 2 versions", "ie >= 8"]
```

按市场份额

```javascript
"browsers": "> 5%"
```

#### modules
false/"commonjs"(defaulted)/"amd"/"esm"

#### stage 的含义
- 0 Strawman 正在热议的观点
- 1 Proposal 值得深入研究的提案
- 2 Draft 加入草案的提案，有可能加入正式版本
- 3 Candidate 接近完成的的提案，发布前需要得到浏览器厂商反馈
- 4 Finished 确认会加入下一版正式规范中 

> 因此 stage-0 所包含的 polyfill 是最多的，因为所有热议的观点都被包含在内，stage后的值越大包含的 polyfill 也就越少，也越有可能加入正式规范中。

### 为什么需要babelrc
理论上完全可以把配置选项写在babel-loader的options里，但单独写在babelrc中，除了为了美观/关注点分离外，还因为 webpack 配置中配置 babel-loader option 并不完全靠谱，比如.vue文件。