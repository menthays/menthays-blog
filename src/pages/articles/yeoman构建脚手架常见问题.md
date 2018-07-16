---
title: yeoman构建脚手架常见问题
date: "2018-03-08T22:40:32.169Z"
layout: post
draft: false
path: "/posts/yeoman-FAQ"
category: "FAQ"
tags:
  - "yeoman"
description: "yeoman构建脚手架遇到的一些常见问题和解决方式的记录"
---

## 简介
这篇文章用来记录yeoman开发前端脚手架的过程中遇到的一些问题。

1. 创建了新的项目目录后，怎么进入该目录再执行 npm install ?  
	
	```javascript
	process.chdir(process.cwd()+'projectName');
	```
2. 复制空目录？  
	查了一些做法感觉不是很合适，所以简单点，直接放一个dumps.txt文件在目录下占个坑即可。。
	
3. 关闭 bower install ?
	
	```javascript
	this.installDependencies({
      npm: true,
      bower: false,
      yarn: false
    });
    // or
    this.npmInstall()
	```
	
	
4. 快速调试？
	用 npm link 将整个 generator link 到全局 node_modules，然后直接执行 yo 命令体验交互过程和生成的项目

5. .babelrc .eslintrc文件没有生成？ 
	发布到npm的过程中这些文件被默认忽略，目前我的做法是改名成 babelrc 然后在 copy 的过程中改名为.babelrc