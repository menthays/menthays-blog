---
title: 用yeoman构建前端脚手架（一）——快速开始
date: "2018-02-18T22:40:32.169Z"
layout: post
draft: false
path: "/posts/building-scaffolds-with-yeoman-1"
tags:
  - "yeoman"
  - "scaffold"
description: "利用yeoman最快速地搭建一个自己的前端脚手架"
---

###背景
最近经常需要用原生js写项目，但是同时为了写的舒服且效率，仍然离不开webpack/babel/postcss/scss/jest 等，每次复制粘贴各种config.js/json又很麻烦，因此先写一个简单的项目同时配置好这一切，然后通过yeoman这个工具来制作脚手架，这样每次新建项目的时候一条命令即可搭建所需的开发环境。  

###过程
首先全局安装yeoman命令行工具yo，以及yeoman官方推出的用来生成脚手架模板的generator。

``` bash
npm install yo -g
npm install generator-generator -g

```
执行 yo generator \<project name\> ，根据提示输入项目信息（ name 必须为generator-***）

``` bash
yo generator <project name>
```
第一步先不考虑命令行中的交互等等功能，我们只想要在执行一条命令后，根据脚手架生成相应的项目结构。修改 /generators/app/index.js

``` javascript
  // writing() {
  //   this.fs.copy(
  //     this.templatePath('dummyfile.txt'),
  //     this.destinationPath('dummyfile.txt')
  //   );
  // }
  writing() {
    this.fs.copy(
      this.templatePath('./'),
      this.destinationPath('./')
    );
  }
  
```
这段的大致意思就是简单地将template下地文件全都拷到执行命令时所在的目录下。  

因此接下来需要将事先写好的项目结构拷贝到 /generators/app/templates/ 下。比如我的[generator](https://github.com/menthays/generator-menthays)里放了一个用来写原生多页应用的，配置好 webpack/babel/postcss/scss/jest 等常用的工具。到这一步我们的脚手架基本算完成了。如果需要提供诸如命令行交互/自定义配置等更丰富的功能，请参考[官方进阶教程](http://yeoman.io/authoring/running-context.html)


此时你可以选择执行 npm link 将项目链接到本机全局npm依赖中，或是将其发布到npm上，然后执行yo \<your generator name\>（比如说如果之前 name 填的是 generator-test ，那么此处便执行 yo test），就可以在当前目录下根据脚手架生成项目结构。  

``` bash
mkdir my-project  
cd my-project
yo test
```  
###注意
不少人可能修改npm的源为淘宝源，但是如果想要发布到npm上，需要先将源改回去后再执行adduser/publish等命令

```bash
npm config set registry http://registry.npm.taobao.org
npm config set registry http://registry.npmjs.org     

```

###总结
本文主要是解决两个问题，一个是懒，不想每次都从新搭建前端开发环境却又舍不得各种使用的现代工具，还有一个是懒，暂时不想深入学习过多的内容来做这件事，通过了解这些最少的内容来完成我们的目的，有更深层次的需要时自然也会进行深入了解。  
我自己的写的 generator ，前文提到过：[*一个搭建好 webpack/postcss/babel/scss/jest 的原生js多页应用脚手架*](https://github.com/menthays/generator-menthays)，已发布到npm。

