---
title: CSS预编译与PostCSS简介
date: "2018-06-23"
layout: post
draft: false
path: "/posts/css-preprocessor-and-postcss"
tags:
- "postcss"
- "css"
- "sass"

description: "简要说明css预编译和postcss的作用和意义"
---
## 预编译 sass/less/styl
### 目的
- 增强可编程性
- 增强复用性与可维护性

### 核心功能
- 嵌套
- 变量
- mixin
- 运算
- 模块化

## PostCss
### 后处理
类似 babel ，可以兼容一些”css未来语法“

### 常用功能
- autoprefixer
- sprites


## Webpack中的配置

```javascript
{
	test: /\.less$/,
	use: ['style-loader', 'css-loader', 'postcss-loader', 'less-loader']
}
```
- 按use反向执行  
- 预编译=>后处理=>获取引用资源(url/@import)=>通过标签插入html文件中
- style-loader 可用插件(ExtractTextPlugin/MiniCssExtractPlugin)替换，用来将css文件独立导出

