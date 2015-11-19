---
title: Node.js 第一次
date: 2018-08-02 00:02:08
tags: [node]
---
[文章来源:Node.js 第一次](http://blog.csdn.net//u011229848/article/details/81355902)


创建一个js文件，输入以下代码：
```javascript
    'use strict; 
    console.log('Hello, world.');
```

第一行总是写上

'use strict';
是因为我们总是以严格模式运行JavaScript代码，避免各种潜在陷阱。

然后，选择一个目录，例如

E:\NodeWorkspace
，把文件保存为

hello.js
，就可以打开命令行窗口，把当前目录切换到

hello.js
所在目录，然后输入以下命令运行这个程序了：
```base 
C:\Users\Administrator>e:
E:\>cd E:\NodeWorkspace
E:\NodeWorkspace>node hello.js

Hello, world.
```
下面开始第一个应用
创建 Node.js 应用

### 步骤一、引入 required 模块

我们使用 **require** 指令来载入 http 模块，并将实例化的 HTTP 赋值给变量 http，实例如下:
var http = require("http");

### 步骤二、创建服务器

接下来我们使用 http.createServer() 方法创建服务器，并使用 listen 方法绑定 8086 端口。 函数通过 request, response 参数来接收和响应数据。

实例如下，在你项目的根目录下创建一个叫 server.js 的文件，并写入以下代码

```javascript
var http = require('http');

http.createServer(function (request, response) {

    // 发送 HTTP 头部 
    // HTTP 状态值: 200 : OK
    // 内容类型: text/plain
    response.writeHead(200, {'Content-Type': 'text/plain'});
    
    // 发送响应数据 "Hello World"
    response.end('Hello World!\n');
}).listen(8088);

// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8088/');
```

以上代码我们完成了一个可以工作的 HTTP 服务器。

切换到server.js 所在目录，使用 **node** 命令执行以上的代码：
```
    node server.js 
    Server running at http://127.0.0.1:8088/
```
 接下来，打开浏览器访问 http://127.0.0.1:8088/
页面会显示返回的 “Hello World!”