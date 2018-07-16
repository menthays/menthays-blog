---
title: 使用Eslint+Prettier规范代码
date: "2018-04-27"
layout: post
draft: false
path: "/posts/format-code-with-eslint-and-prettier"
tags:
  - "eslint"
  - "prettier"
  - "scaffold"
description: "在项目中配置eslint+prettier以规范并美化代码"
---

Eslint 和 Prettier 大部分前端程序员至少都应该有所耳闻，本文主要介绍如何快速地搭建一套代码检查环境，包含代码美化与规范化，自动修复，以及不能修复的地方抛出错误。

首先第一步安装所有依赖的第三方lib

```javascript
npm install eslint prettier eslint-plugin-prettier eslint-config-prettier --save-dev
```

eslint 和 prettier不必多说，剩下的其中 eslint-plugin-prettier 主要的作用在 eslint 执行的过程中，让代码符合 prettier 的规范，如有不同则会抛出错误，可以通过手动的 eslint --fix 来修复；eslint-config-prettier 则是用来处理 prettier 和 eslint 规则可能冲突的地方（比如单双引号/是否使用分号等），开启后会关闭所有可能引起冲突的规则。

然后开始编写配置文件，新建文件 .eslintrc

```javascript
{
  "extends": ["prettier"],
  "plugins": [
    "prettier"
  ],
  "env": {
    "jest": true,
    "node": true,
    "browser": true
  },
  "rules": {
    "prettier/prettier": [
      "error",
      {
        "singleQuote": true,
        "printWidth": 90
      }
    ]
  }
}
```



*extends* 传入一个数组，里面是一些对 rule 的修改的拓展设置（前文中的 eslint-config-prettier）

*plugins* 传入了 eslint-plugin-prettier （在 eslint 过程中同时使代码符合prettier规范）

*env* 表示代码检查运行的环境，比如这里 browser: true 是为了关闭对 window/navigator 等浏览器中对象的报错（否则会报使用了未声明的变量），jest/node 也是同理，比如 jest 的 describe/it 等函数。

*rules* 可以手动配置一些额外的规则，这里修改了 prettier 的部分规则，比如不符合 prettier 时抛出 error 而不是 warning，使用单引号，一行最大宽度为90。

然后修改 **package.json** 文件添加相应命令

```json
{
  ...
	scripts: {
    ...
    "lint": "eslint . --ignore-path .gitignore",
    "format": "eslint . --fix --ignore-path .gitignore"
  }
  ...
}
```

这里的 *--ignore-path .gitignore* 是为了直接使用 gitignore 当做 eslintignore，当然你也完全可以新建一个 .eslintignore 文件来规定自己需要忽略的文件。然后我们就可以在项目中使用这两条命令规范我们的代码啦。

```
# auto format code, throw error if it cannot be fixed automatically
npm run format
```

最后，很多开发者可能会不习惯 eslint + prettier 的规则，有不想从头自己写一个，那么可以通过插件的方式引用别的规则，比如 eslint-config-xo ，这是 yeoman 的 generator-generator 中推荐使用的规则。同样的安装后略微修改 .eslintrc 文件即可。

```
extends: ["xo", "prettier"]
```

然后我们就可以看到赏心悦目治愈了强迫症的代码啦。
