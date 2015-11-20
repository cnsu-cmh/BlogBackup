---
title: 删除或修改eclipse中svn的账号密码
date: 2016-11-21 13:46:42
tags: [java,svn,eclipse]
---
[文章来源:删除或修改eclipse中svn的账号密码](http://blog.csdn.net/u011229848/article/details/53258740)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/53258740

首先确保eclipse已安装且可以正常使用svn

打开eclipse ---> window ---> 点击Perference ,打开eclipse配置，输入svn,然后点击svn，找到下方svn接口，查看svn接口类型，如果是JavaHL

![](删除或修改eclipse中svn的账号密码/1.png)
打开电脑计算机，系统安装盘，一般是C盘，找到用户，点击打开

![](删除或修改eclipse中svn的账号密码/2.png)
打开之后，在搜索栏里面输入auth,就是在用户里面搜索这个文件夹
![](删除或修改eclipse中svn的账号密码/3.png)
找到这个文件夹之后点击打开，然后删除这个文件夹里面的文件，然后重启eclipse,重新使用svn的时候就会重新输入账号密码了！
![](删除或修改eclipse中svn的账号密码/4.png)
eclipse的svn类型一般为JavaHL类型，路径在C:\Users\Administrator\AppData\Roaming\Subversion\auth，要是实在找不到可以全局搜索在C盘

![](删除或修改eclipse中svn的账号密码/5.png)
要是svn的接口类型是SVNKit，则删除eclipse ,configuration--->org.eclipse.core.runtime
![](删除或修改eclipse中svn的账号密码/6.png)
确保eclipse的svn能正常使用，已经安装完svn插件了！