---
title: 用yeoman构建前端脚手架（二）——增加命令行交互
date: "2018-03-04T22:40:32.169Z"
layout: post
draft: false
path: "/posts/building-scaffolds-with-yeoman-2"
tags:
  - "yeoman"
  - "scaffold"
description: "为脚手架增加命令行交互进行自定义配置"
---

###背景
我们已经可以通过新建一个目录，并且执行 yo \<generator-name\> 通过脚手架初始化一个项目了，但是有两点还是很麻烦：  
1. 要新建一个目录，再进入目录下  
2. 最好能够在初始化时，通过命令行做一些定制，比如package.json的信息

###过程
先完成第一条，首先我们需要获得相应的 argument ，比如这里的项目名

```javascript
module.exports = class extends Generator {
  // note: arguments and options should be defined in the constructor.
  constructor(args, opts) {
    super(args, opts);

    // This makes `appname` a required argument.
    this.argument('appname', { type: String, required: true });

    // And you can then access it later; e.g.
    this.log(this.options.appname);
  }
};
```
参考上面这个[官网示例](http://yeoman.io/authoring/user-interactions.html)，在 yo \<generator-name\> arg --opt 这样一条命令中，我们可以通过 argument/option 方法来声明并定义参数/选项的类型，并且所有的参数都可以通过 this.options[\<key\>] 来取得。
获得这个参数后，我们就可以改写原来的 writing 方法了。

```javascript 
  writing() {
    this.fs.copyTpl(
      this.templatePath('./'),
      this.destinationPath(`./${this.options['appname']}/`),
    );
  }
```
修改 destinationPath 参数，将template下的内容全都写入文件名之前传入的项目名的文件夹下，第一条就完成啦。

完成第二点我们需要修改promting方法，比较浅显易懂，直接贴代码

```javascript
  prompting() {
    // Have Yeoman greet the user.
    this.log(yosay(
      'Welcome to the sweet ' + chalk.red('generator-menthays') + ' generator!'
    ));

    return this.prompt([{
      type    : 'input',
      name    : 'name',
      message : 'Your project name',
      default : this.options.appname // Default to current folder name
    }, {
      type    : 'input',
      name    : 'username',
      message : 'What\'s your GitHub username',
      store   : true
    }, {
      type    : 'input',
      name    : 'description',
      message : 'Some description pls',
    }]).then((props) => {
      this.props = props
    });
  }
```

这段代码会在执行yo \<generator-name\> \<project-name\> 后给出三个交互，要求输入 name, author(username), description 这三个信息，其中 name 默认是之前填的项目名，username 会被储存成为下次执行命令时的默认值。最终将这三个参数存在this.props里。然后修改writing

```javascript 
  writing() {
    this.fs.copyTpl(
      this.templatePath('./'),
      this.destinationPath(`./${this.options['appname']}/`),
      {...this.props}
    );
  }
```
在写入相应目录的时候附带参数。然后修改 template context （这里是package.json文件）

```json
{
  "name": "<%= name %>",
  "author": {
    "name": "<%= username %>"
  },
  "description": "<%= description %>"
}
```
使用 ejs 语法将三个参数的key值写到文件里，这样在执行yo的时候，之前的三个参数便会替代进相应的位置。第二条也就完成了。

###调试
当然不能每次修改一点就提交到github然后publish到npm，然后再本地尝试。利用之前提到的 npm link ,将这个generator项目关联到本机全局node_modules，然后就可以无需npm install直接测试了

```bash
npm link
yo <generator-name> <project-name>
```

###总结
本文主要讲了yo命令行的用户交互以及文件系统交互两个方面，也是开发脚手架最常用的一些内容，到这一步基本就可以做一些可配置性较高的脚手架了。更深入的可前往浏览官网[file systems一节](http://yeoman.io/authoring/file-system.html)以及[user interactions一节](http://yeoman.io/authoring/user-interactions.html)。