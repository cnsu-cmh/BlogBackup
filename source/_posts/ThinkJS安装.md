---
title: ThinkJS安装
date: 2018-08-03 17:32:20
tags: [node]
---
[文章来源:ThinkJS安装](http://blog.csdn.net/u011229848/article/details/81391601)


本文摘抄于 ThinkJS官方文档2.2版本

ThinkJS 是一款 Node.js 的 MVC 框架，所以安装 ThinkJS 之前，需要先安装 Node.js 环境。ThinkJS 需要 Node.js 的版本

>=0.12.0
，如果版本小于这个版本，需要升级 Node.js，否则无法启动服务。

### 安装 ThinkJS

通过下面的命令即可安装 ThinkJS：
```
npm install thinkjs@2 -g --verbose
```
```
安装 ThinkJS 3 命令
npm install -g think-cli

安装完成后，系统中会有 thinkjs 命令（可以通过 thinkjs -v 查看 think-cli 的版本号，此版本号非 thinkjs 的版本号）。 
注：如果是从 2.x 升级，需要将之前的命令删除，然后重新安装。

卸载旧版本命令
npm uninstall -g thinkjs
```
 <!--more-->
如果安装很慢的话，可以尝试使用 [taobao](http://npm.taobao.org/) 的源进行安装。具体如下：
```
npm install thinkjs@2 -g --registry=https://registry.npm.taobao.org --verbose
```

安装完成后，可以通过

thinkjs --version
或

thinkjs -V
命令查看安装的版本。

注
：如果之前安装过 ThinkJS 1.x 的版本，可能需要将之前的版本删除掉，可以通过

npm uninstall -g thinkjs-cmd
命令删除。

### 更新 ThinkJS

更新全局的 ThinkJS

执行下面的命令即可更新全局的 ThinkJS：
```
npm install -g thinkjs@2
```
更新项目里的 ThinkJS

在项目目录下，执行下面的命令即可更新当前项目的 ThinkJS：
```
npm install thinkjs@2
```
### 使用命令创建项目

ThinkJS 安装完成后，就可以通过下面的命令创建项目:
```
thinkjs new project_path; /#project_path为项目存放的目录
```
注
： 从

2.2.12
版本开始，创建的项目默认为 ES6 模式，不再需要加

--es
参数, 如果想创建一个 ES5 模式项目，需要加参数

--es5
。

### 安装依赖

项目安装后，进入项目目录，执行

npm install
安装依赖，可以使用

taobao
源进行安装。
```
npm install --registry=https://registry.npm.taobao.org --verbose
```
### 启动项目

在项目目录下执行命令

npm start