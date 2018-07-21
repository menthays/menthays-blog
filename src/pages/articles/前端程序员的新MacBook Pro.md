---
title: 前端程序员的新 MacBook Pro
date: "2018-07-01T14:30:32.119Z"
layout: post
draft: false
path: "/posts/new-mbp-of-a-frontend-engineer"
tags:
- "develop environment"

description: "记录拿到新 MacBook Pro 需要安装的一些常用软件/工具/配置以及安装方法"
---

##Software required:

- [iTerm2](https://www.iterm2.com/index.html)
- [homebrew](https://brew.sh)

	```bash
	/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
	```
- node & nvm
	
	```bash
	brew install node nvm
	```
	
- [ohMyZsh](https://ohmyz.sh)
	
	```bash
	sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

	```
	
- [vscode](https://code.visualstudio.com)
- [shadowsocks-ng](https://github.com/shadowsocks/ShadowsocksX-NG/releases)
- git
	
	```
	brew install git
	```
	
- [go](https://golang.org/dl/)
	
	```
	#install tools
	cd %GOPATH%\src\golang.org\x\
	git clone https://github.com/golang/tools.git tools
	# do install (trigger by vs code extension)
	```
	
- sketch
- Mac Down / Mark text
- 微信开发者工具
- Docker
- Dr Unarchiver
- Chrome / Firefox
- Chrome extension: vue/react/translate/octotree/adblock plus/postman
- safari: adblock 

##Config should be known
- git config \-\-global http.proxy "http://127.0.0.1:1087" 
- git config \-\-global https.proxy "http://127.0.0.1:1087"
- export http_proxy=http://127.0.0.1:1087
- export https_proxy=http://127.0.0.1:1087
- npm config registry/http\_proxy/https\_proxy
- npm config set registry https://registry.npm.taobao.org

##Personal 
- Wacom
- sketchbook
- youdao


