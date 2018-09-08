---
title: ExtJS 4---主题theme
date: 2016-11-20 22:55:40
tags: [ext]
---
[文章来源:ExtJS 4---主题theme](http://blog.csdn.net/u011229848/article/details/53247453)


# 1 UI组件基础

学习ExtJs就是学习组件的使用。ExtJs4对框架进行了重构，其中最重要的就是形成了一个结构及层次分明的组件体系，由这些组件形成了Ext的控件。

ExtJs4的组件体系中有将近100种组件，而这些组件又可以大致分为四大类，即容器类组件、工具栏及菜单栏组件、表单及元素组件、其他组件。

# 2 主题

ExtJs4引入了全新的主题系统，采用Sass和Compass技术，提供了标准的主题模板，通过对主题模板的简单定制就可以创造出丰富多彩的各种主题。

## 2.1 Sass和Compass概述

### 2.1.1 Sass

Sass样式表语言是CSS的一个扩展，为CSS提供了变量、内嵌规则、混入（mixins）、选择器继承等特性，在最新的Sass3中100%兼容CSS3，语法文件也升级为SCSS（Sassy CSS），每一个有效的CSS3文件也是有效的SCSS文件，这种兼容性降低了学习成本，开发人员可以平稳的由CSS过渡到Sass的开发。

Sass样式表语言为CSS级联样式表提供了编程的能力，现在我们可以在Sass中定义变量在不同的样式中引用甚至进行计算，定义混入（mixins）在不同的地方进行复用，这些能力都是CSS所不具有的，经过编译之后Sass会输出标准的CSS文件在不同的浏览器中使用。

Sass特性：

* 混入(Mixins)——class中的class；
* 参数混入——可以传递参数的class，就像函数一样；
* 嵌套规则——Class中嵌套class，从而减少重复的代码；
* 运算——CSS中用上数学；
* 颜色功能——可以编辑颜色；
* 名字空间(namespace)——分组样式，从而可以被调用；
* 作用域——局部修改样式；
* JavaScript 赋值——在CSS中使用JavaScript表达式赋值。

Sass的详细介绍和说明可见：[http://sass-lang.com/](http://sass-lang.com/)

### 2.1.2 Compass

Compass是一个基于Ruby的、开源的、用于CSS创作的框架。它使用Sass样式表语言，可以非常容易和高效地构造样式表，同时，Compass内置了大量Web开发中可重用的优秀模式，以便开发者使用。下面用一个简单的等式来展示Compass所发挥的作用：

**Compass = Sass样式表语言 + 大量可重用的优秀CSS模式**

Compass的详细介绍和说明可见：[http://compass-style.org/](http://compass-style.org/)

## 2.2 准备工作

### 2.2.1 安装Ruby

使用SASS和Compass需要用到Ruby，可以到http://rubyinstaller.org/下载Ruby的安装包，下载后的文件是“rubyinstaller-1.9.3-p429.exe”。

（注意，不要下载最新版Ruby2.0.0-p195，不然后面开发中会由于版本问题出错。Ruby 1.9.3-p429就可以。）

双击运行，步骤如下：

![](http://images.cnitblog.com/blog/479020/201307/02162101-784d7dfe28744dfeb453d5fb7d87d85c.x-png)

![](http://images.cnitblog.com/blog/479020/201307/02162133-4e7e41b78d3044cc9c876c0338489369.jpg)

![](http://images.cnitblog.com/blog/479020/201307/02162158-7df108a5ecdb4329a4cb06136807879f.jpg)

注意将安装目录上的3个选项都选上。

点击完成。

至此，Ruby就安装完成了。

在开始菜单Ruby程序组下，单击“Start Command Prompt with Ruby”。

![](http://images.cnitblog.com/blog/479020/201307/02162344-a8b98533ca63498d91a36dda481ac4fa.x-png)

进入Ruby的命令行界面。输入
ruby –v

回车后界面提示如下：

![](http://images.cnitblog.com/blog/479020/201307/02162529-0044f8ff4368483abbb10bad049bac61.jpg)

说明Ruby运行环境安装成功。

### 2.2.2 安装Compass和Sass

要使用Compass，首先要在Ruby中安装框架。

在开始菜单Ruby程序组下，单击“Start Command Prompt with Ruby”，显示命令提示窗口。在命令行输入以下命令开始安装Compass：
gem install compass

（该命令可以自动远程安装Compass相关文档到本地文件夹中，由于采用远程安装的方式，因此安装时间较长，请耐心等待。）

当看到如下图所示窗口中的信息时，表示Compass已经安装成功。

![](http://images.cnitblog.com/blog/479020/201307/02162804-0fa4fcc5adef4d8eb79ae5e93feb9634.jpg)

从图中可以看出，已经安装了sass-3.2.9版和compass-0.12.2 版。

在命令行中执行：compass –v 和 sass –v可以分别查看当前系统中已安装的版本信息。

到这里，已经完成了Sass和Compass的安装。

### 2.2.2.1 Compass项目（Sass编译成Css）

1.**项目初始化**

接下来，要创建一个你的Compass项目，假定它的名字叫做myproject，那么在命令行键入：　　
compass create myproject

当前目录中就会生成一个myproject子目录。

进入该目录：

cd myproject

你会看到，里面有一个[config.rb](https://github.com/thesassway/sass-test/blob/master/config.rb)文件，这是你的项目的[配置文件](http://compass-style.org/help/tutorials/configuration-reference/)。还有两个子目录sass和stylesheets，前者存放Sass源文件，后者存放编译后的css文件。

![](http://images.cnitblog.com/blog/479020/201307/02162944-59fde656f3af4f0f979f3849d0c73d61.x-png)

接下来，就可以动手写代码了。

**2.编译**

在写代码之前，我们还要知道如何编译。因为我们写出来的是后缀名为scss的文件，只有编译成css文件，才能用在项目上。

Compass的编译命令是
compass compile

该命令在项目根目录下运行，会将sass子目录中的scss文件，编译成css文件，保存在stylesheets子目录中。

默认状态下，编译出来的css文件带有大量的注释。但是，生产环境需要压缩后的css文件，这时要使用--output-style参数。

compass compile --output-style compressed

Compass只编译发生变动的文件，如果你要重新编译未变动的文件，需要使用--force参数。

compass compile --force

除了使用命令行参数，还可以在配置文件config.rb中指定编译模式。
output_style = :expanded

:expanded模式表示编译后保留原格式，其他值还包括:nested、:compact和:compressed。

说明：

/* nested：嵌套缩进的css代码，它是默认值。

/* expanded：没有缩进的、扩展的css代码。

/* compact：简洁格式的css代码。

/* compressed：压缩后的css代码。

进入生产阶段后，就要改为:compressed模式。
output_style = :compressed

也可以通过指定environment的值（:production或者:development），智能判断编译模式。

environment = :development output_style = (environment == :production) ? :compressed : :expanded

在命令行模式下，除了一次性编译命令，compass还有自动编译命令
compass watch

运行该命令后，只要scss文件发生变化，就会被自动编译成css文件。

更多的compass命令行用法，请参考[官方文档](http://compass-style.org/help/tutorials/production-css/)。

### 2.2.2.2 Compass、Sass与eclipse集成

通过上面的配置，我们可以通过文本编译器等编辑好Sass文件后，再通过相应的目录结构通过Ruby将Sass编译成css，再复制粘贴到eclipse中我们的工程内，进行开发。可以看出这个过程比较繁琐。那么，如何在eclipse中可以直接编辑Sass文件，并能自动编译成css被工程应用?下面就是对Compass、Sass与eclipse的集成研究。

1. 确认ant已经安装
1. 打开工程的“properties”，选择“Builders”，然后点击 “New…” 按钮
1. 选择“Ant Builder”，点击“OK”
1. 输入名称“compass.compile”，在“Main”Tab页中，点击“Browse Workspace”来选择已放入工程中的build.xml文件。
1. 选择“Targets”Tab页，在Auto-Build中点击“Set Targets”按钮，选择“compass.compile”来使用Compass（ “sass.compile”只是用Sass），点击“OK”
1. 选择“Build Options”Tab页，点击“Select Resources”按钮，选择Sass文件目录，点击“Finish”，再点击“OK”。

现在当编辑Sass文件后， css文件将会自动的被创建或更新。

### 2.2.3 Sencha CMD安装

Sencha CMD是打包和部署ExtJs和Sencha Touch应用的命令行工具。为了开发ExtJs4.2的主题，你必须安装Sencha CMD3.1或更高的版本。

1. 安装Java Run-time Environment or JRE，要求版本>=6.0。
1. 为了编辑样式，需要Compass、Sass，所以需要安装Ruby。注意，不要下载最新版Ruby2.0.0-p195，不然后面开发中会由于版本问题出错。Ruby 1.9.3-p429就可以。
1. 下载Sencha Cmd安装包进行安装。
1. 下载并解压Ext JS SDK。

## 2.3 ExtJs主题样式开发（自定义主题）

### 2.3.1 创建工作空间

（注意，目录名称等不要用中文，特殊符号最好也不要用，否则会出错）

打开系统cmd命令行窗口，定位到SDK解压的目录。输入
sencha -sdk d:/ExtJs4-App/ext-4.2.1.883 generate workspace my-workspace

创建了一个包含自定义主题包package名为my-workspace的工作空间。这个命令将Ext JS SDK和package关联到您的工作区中,这样，主题和应用程序可以找到他们所需的依赖项。这个命令生成的主题和应用程序必须执行在工作区目录中，因此改变你的工作目录到新的“my-workspace”目录：

cd my-workspace

你应该在“my-workspace”文件夹下看到两个文件夹：

l “ext”：包含Ext JS SDK

l “package”包含Ext JS语言环境和主题包

### 2.3.2 生成一个应用程序进行测试的主题

创建一个自定义主题之前，我们需要设置一个方法来测试主题。最好的测试方法是在一个应用程序中使用它。在“my-workspace”目录下运行以下命令：
sencha -sdk ext generate app ThemeDemoApp theme-demo-app

这告诉Sencha Cmd在一个新的子目录“theme-demo-app”中生成一个名为ThemeDemoApp的应用程序，并同时关联" Ext "目录下的Ext JS SDK。现在让我们构建应用程序：

cd theme-demo-app sencha app build

有两种方法可以运行您的应用程序: 　

1. 开发模式：通过浏览器打开“theme-demo-app/index.html”。

这个是没有进行资源（源文件）压缩的，易于调试。

1. 产品模式：通过浏览器打开“build/ThemeDemoApp/production/index.html”。

这个是为了给应用提供更小的内存占用和更好的性能，进行了资源（源文件）压缩。

### 2.3.3 生成主题包和文件结构

Sencha Cmd允许自动的生成主题包和文件结构。在“theme-demo-app”下运行以下命令：
sencha generate theme my-custom-theme

这是告诉Sencha Cmd在当前工作空间下创建一个名为“my-custom-theme”的主题包。

让我们看一看它的内容：

l ”package.json”——这是包属性文件。它告诉Sencha Cmd一些包的信息，如 name, version, and dependencies (其他包所需要的配置)。　

l “sass/”——这个目录包含所有关于主题的sass文件。sass文件分为三个主要部分:

1) “sass/var/”——包含sass变量；　

2) “sass/src/”——包含sass规则和UI混合调用，它们可以使用“sass/var/”下的变量； 　　

3) “sass/etc/”——包含额外的公用功能或混入在“sass/var/”和“sass/src/”中的文件，这些文件应该是结构化匹配的组件类路径。例如， Ext.panel.Panel的样式变量应放置在一个命名为“sass/var/panel/Panel.scss”的文件内。　　

l “resources/”——包含主题需要的图像和其他静态资源。　　

l “overrides/”——包含一些替换Ext JS组件类（对组件进行主题化的类）的JavaScript。

### 2.3.4 配置主题继承

主题包通常是这样具有一个非常重要的、附加功能的特殊的包，它们可以继承自其他的主题包。新版本Ext Js 4.2中利用主题包的这一特性来创建它的主题层次结构：

![](http://images.cnitblog.com/blog/479020/201307/02163347-cdd16d78cccf46c58e5f7a47d60d4f9f.x-png)

每个主题包必须从Base主题扩展。创建自定义主题的下一步是找出从哪个主题进行扩展。在工作空间中可以看到以下主题包：

l "ext-theme-base"--这个包是其他主题的基本主题，它是唯一一个没有父主题的主题包。它包含了设置CSS规则的最少值，这些值是让Ext JS组件和布局正确工作所必需的的内容。"ext-theme-base"的样式规则在派生的主题中是不可配置的，应该避免覆盖由这个主题创建的任何样式规则。

l "ext-theme-neutral"--从

"ext-theme-base"
扩展而来，包含了绝大多数的可配置的样式规则。大多数的变量是用于配置在"ext-theme-neutral"中定义的Ext JS组件样式。这些变量可以被自定义主题替换。

l "ext-theme-classic"--默认主题。从"ext-theme-neutral"扩展而来。

l "ext-theme-gray"--从"ext-theme-classic"扩展而来

l "ext-theme-access"--从"ext-theme-classic"扩展而来。

l "ext-theme-neptune"--从"ext-theme-neutral"扩展而来。

建议使用"ext-theme-neptune"或"ext-theme-classic"作为自定义主题扩展的开始节点。这是因为，这些主题包含创建一个具有吸引力并立即可用的主题的所有必要的代码。"ext-theme-neutral"是一个非常抽象的主题，不应该直接用于扩展。基于"ext-theme-neutral"进行自定义主题的扩展，需要数以百计的变量覆盖和过多的工作量，而且可能只有非常高级的主题开发者才能进行这项工作，反之，使用"ext-theme-neptune"或"ext-theme-classic"可以通过简单地改变几个变量在几分钟内启动并运行。另外你可以覆盖"ext-theme-gray"或"ext-theme-access"，如果它们可以为自定义主题提供一个更理想的起点。

例如，我们创建一个由"ext-theme-neptune"扩展的自定义主题。首先需要替换目录"packages/my-custom-theme/package.json"：
"extend": "ext-theme-classic"

为

"extend": "ext-theme-neptune"

你现在需要更新您的应用程序。确保正确的主题JavaScript文件都包含在应用程序“bootstrap.js”中，这样应用程序就可以在开发模式下运行。在"theme-demo-app"目录下运行以下命令。
sencha app refresh

### 2.3.5 配置全局主题变量

现在已经建立了自己的主题包，可以开始修改主题外观。首先修改能够派生出ExtJs组件公有颜色的基础颜色。在"my-custom-theme/sass/var/"下，创建一个名为Component.scss的文件，在文件中输入：
$base-color: /#317040 !default;

如果你想让你的自定义主题是可扩展的，一定要在所有变量末尾配置!default。没有!default你将不能覆盖一个派生主题的变量，因为Sencha Cmd变量遵循“反向”规则——大多数派生样式第一，基本样式最后。更多的!default信息可参考：[Variable Defaults](http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#variable_defaults_)。

完整的Ext JS全局SASS变量列表请参考：[Global_CSS]()。

### 2.3.6 构建包

为了使所有自定义的样式规则生成css文件，需要在"packages/my-custom-theme/"目录下运行命令：
sencha package build

这将在package下构建一个目录。在 "my-custom-theme/build/resources"下你会发现一个文件命名为 my-custom-theme-all.css。这个文件包含所有Ext JS组件的样式规则。你可以直接在应用程序中运用这个文件，但这并不可取，因为“all”文件包含所有的样式，但是Ext JS组件和大多数应用程序只使用Ext JS组件的一个子集。当你构建一个应用程序时，Sencha Cmd能够过滤掉未使用的CSS样式规则，但是首先我们需要配置测试程序使用自定义主题。

### 2.3.7 在一个应用程序使用一个主题

为刚刚创建的自定义主题配置测试应用程序。找到theme-demo-app/.sencha/app/sencha.cfg
app.theme=ext-theme-classic

修改为

app.theme=my-custom-theme

如果你已经运行了一个构建使用经典主题的应用程序，你应该清理构建目录。从theme-demo-app运行:
sencha ant clean

接着构建应用：

sencha app build

在浏览器中打开"theme-demo-app/index.html"，你将看到如下

![](http://images.cnitblog.com/blog/479020/201307/02163532-1c7ca62c56cc4a16a2598cbdedece32a.x-png)

### 2.3.8 配置组件的变量

每一个Ext JS组件都有一个全局变量的列表,可以用来配置其外观。下面，我们来改变面板标题的字体类型。创建文件"packages/my-custom-theme/sass/var/panel/Panel.scss"，代码如下：
$panel-header-font-family: Times New Roman !default;

在“theme-demo-app”下运行：

sencha app build

在浏览器中打开"theme-demo-app/index.html"，你将看到如下

![](http://images.cnitblog.com/blog/479020/201307/02163605-fba908de5ca2416d8a388d19509cebc0.x-png)

在API文档的每一节 "CSS Variables"中，有组件SASS变量的详细列表。

### 2.3.9 创建自定义组件UI

在ExtJs框架中，每一个组件都有一个配置界面（默认为“default”）。这个属性可以被配置在单个组件实例中，区别于其他组件的相同类型，给他们提供一个不同的外观。

创建文件"packages/my-custom-theme/sass/src/panel/Panel.scss"，代码如下：
[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码") @include extjs-panel-ui( $ui-label: 'highlight-framed', $ui-header-background-color:red, $ui-border-color:red, $ui-header-border-color:red, $ui-body-border-color:red, $ui-border-width:5px, $ui-border-radius:5px );

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

打开"theme-demo-app/app/view/Viewport.js"，修改代码如下：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码") Ext.define('ThemeDemoApp.view.Viewport', {extend: 'Ext.container.Viewport',requires:['Ext.layout.container.Border','ThemeDemoApp.view.Main'],layout:{type: 'border'},items:[{ // default UIregion: 'west',xtype: 'panel',title: 'West',split:true,width: 150}, { // custom"highlight"UIregion: 'center',xtype: 'panel',layout: 'fit',bodyPadding: 20,items:[ {xtype: 'panel',ui: 'highlight',frame:true,bodyPadding: 10,title: 'Highlight Panel'} ] }, { // neptune"light"UIregion: 'east',xtype: 'panel',ui: 'light',title: 'East',split:true,width: 150}] });

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

在 "theme-demo-app" 下运行程序：
sencha app build

在浏览器中打开"theme-demo-app/index.html"，你将看到如下

![](http://images.cnitblog.com/blog/479020/201307/02163719-b1b241c2f4f642afbafe745f74d6b0b4.x-png)

虽然UI是一个方便的方法,可以混合多种外观配置为一个组件,他们不应该被过度使用。因为每次调用UI mixin会生成额外的CSS规则,无偿调用UI mixin会产生过大的CSS文件。

另外重要一点是要记住当调用UI mixin时，是通过其命名参数调用mixin，不是没有参数值的有序列表。尽管SASS都支持这两种形式，最好使用这种形式:
[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码") @include extjs-component-ui( $ui-foo:foo, $ui-bar:bar );

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

避免下面的形式：

@include extjs-component-ui(foo, bar);

因为mixin参数的复杂性和数量有可能变动，如新增或删除一个参数，那么后一种调用方式就会失效。

### 2.3.10 修改图像资源

所有必需的图像资源默认是继承父主题的,但在某些情况下,您可能需要覆盖一个图像。这可以通过把所需的图像在"my-custom-theme/resources/images/"下覆盖相同名称的图像。例如,让我们修改弹出窗口组件的信息图标。保存"packages/my-custom-theme/resources/images/shared/icon-info.png"

现在使用自定义图标，修改测试应用程序显示一个消息框。在应用程序窗口的highlight panel 添加items("theme-demo-app/app/view/Viewport.js"):
[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码") requires:[ ...'Ext.window.MessageBox', ... ], ...title: 'Highlight Panel',items:[{xtype: 'button',text: 'Show Message',handler:function() { Ext.Msg.show({title: 'Info',msg: 'Message Box with custom icon',buttons:Ext.MessageBox.OK,icon:Ext.MessageBox.INFO });} }] ...

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

构建应用程序，在浏览器中查看：

![](http://images.cnitblog.com/blog/479020/201307/02163815-01b7c6db93fe4eeda5e3a5bd51ad6a46.x-png)

原始样式：

![](http://images.cnitblog.com/blog/479020/201307/02163821-822eabfda54c4ac3a0320e647f880ec8.x-png)

### 2.3.11 为IE中的CSS3效果切割图像

在许多情况下,当创建新的用户界面时，可能包括背景渐变或圆角。不幸的是,不是所有浏览器都支持CSS3属性,所以我们必须使用图像来弥补。Sencha Cmd能够自动切割这些图片给你。要做到这一点,我们需要告诉Sencha Cmd哪些组件需要切片。这些切片配置文件都包含在每个主题的 "sass/example/"目录下。参考"packages/ext-theme-base/sass/example/"：

"shortcuts.js"--这个文件包含了组件类型的基本配置,可以切割。大多数自定义主题不需要包含"shortcuts.js"文件;除非你的主题包括自定义组件样式。你的样式继承所有的基本样式的快捷键定义,也可以添加额外的快捷定义，需要在 "shortcuts.js"文件中调用Ext.theme.addShortcuts()函数。　　

"manifest.js"--这个文件包含能够生成切片图像的组件UI列表。可以从父主题继承所有的清单条目,也可以在"manifest.js"中通过调用Ext.theme.addManifest()函数添加自己的清单条目。　　

"theme.html"--这个文件是用来渲染在"manifest.js"中定义的文件。

为"highlight" panel创建圆角切片， 创建"packages/my-custom-theme/sass/example/manifest.js" ，代码如下：
[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码") Ext.theme.addManifest( {xtype: 'panel',ui: 'highlight'} );

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

编辑"packages/my-custom-theme/sass/example/theme.html"添加以下脚本标签：

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码") <!-- Required because Sencha Cmd doesn't currently add manifest.js from parent themes --> <script src="../../../ext-theme-neptune/sass/example/manifest.js"></script> <!-- Your theme's manifest.jsfile --> <script src="manifest.js"></script>

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

为了能够在构建应用程序时将UI切片，必须在"theme-demo-app/sass/example/theme.html"中添加两个脚本标记：
<script type="text/javascript" src="../../../packages/ext-theme-neptune/sass/example/manifest.js"></script> <script type="text/javascript" src="../../../packages/my-custom-theme/sass/example/manifest.js"></script>

构建应用程序，在IE8及以下版本浏览，可以看到 "highlight" panel 展示的是圆角，和在IE9及其他浏览器的CSS3效果呈现一致。

![](http://images.cnitblog.com/blog/479020/201307/02163847-5216c9058eb14ae8993f31a47a5ba7e2.x-png)（IE7中）

### 2.3.12 覆盖主题JS

有时一个主题可以通过配置JS改变组件外观。通过覆盖主题包中的JavaScript就可以实现。如，创建"packages/my-custom-theme/overrides/panel/Panel.js" ，代码如下：
[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码") Ext.define('MyCustomTheme.panel.Panel', {override: 'Ext.panel.Panel',titleAlign: 'center'});

[![复制代码](http://common.cnblogs.com/images/copycode.gif)]( "复制代码")

现在需要构建主题包来让"packages/my-custom-theme/build/my-custom-theme.js"包含这个新的覆盖文件。在"packages/my-custom-theme/" 下运行：

sencha package build

现在应该刷新应用程序,这样在开发模式下运行应用程序会包括该主题的JS覆盖文件。在 "theme-demo-app"下运行：
sencha app refresh

构建应用：

sencha app build

在浏览器中打开"theme-demo-app/index.html"，你将看到如下

![](http://images.cnitblog.com/blog/479020/201307/02163920-aa287bdc9bb240b381ca8bad97d5703e.x-png)