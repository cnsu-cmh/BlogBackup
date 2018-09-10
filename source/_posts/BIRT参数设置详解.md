---
title: BIRT参数设置详解
date: 2017-02-09 14:13:33
tags: [birt]
---

## [BIRT参数设置详解](http://www.cnblogs.com/langlangago/articles/5077226.html)

2015-12-25 23:02 by langlangago, 673 阅读, 0 评论, [收藏](http://www.cnblogs.com/langlangago/articles/5077226.html#), [编辑](https://i.cnblogs.com/EditArticles.aspx?postid=5077226)

[BIRT参数设置详解](http://www.blogjava.net/huangzhanhu/archive/2010/02/03/311777.html)

http://www.blogjava.net/huangzhanhu/archive/2010/02/03/311777.html

在使用birt报表的时候感觉页面的 BIRT Report Viewer头标题没有用，想去掉在网上一查原来有详细的参数设置，看来birt的功能还是很强大啊。现在转帖如下：
BIRT作为一款功能强大的开源报表工具，其版本的升级更新速度也非常快，从1.0到2.0，一直到最新的2.2.1版本，无论从功能上还是性能上都得到了极大的提高和扩充。BIRT也提供了一个标准的J2EE实现组件，可以发布到支持J2EE应用的web server服务器上，可以对生成的报表进行预览等操作。在大家使用BIRT Viewer的时候，可能会对它日益纷繁的参数设置如云里雾里，在网上论坛里也有很多人碰到这样哪样的问题，同时官方的文档也不细致不全。所以我就对这些参数进行了一个简单系统的总结，希望能对大家的BIRT开发有所帮助。这些参数以2.2.1版本为准，请大家特别注意。
<!--more--><br/>1. Servlet模式说明
查看BIRT Viewer自带的web.xml文件，可以看到有以下几个pattern：<br/>
frameset ---- 采用Ajax框架，可以显示工具条，导航条和TOC面板，实现复杂的操作，如分页处理，导出数据，导出报表，打印等等。该模式下会自动生成report document文件(预览report design文件)到特定的目录(用户可以用参数指定，也可以定义在web.xml里)。采用Ajax，速度较慢。
<br/>run ---- 也采用Ajax框架，但不实现frameset的复杂功能，不会生成临时的report document文件(预览report design文件)，也不支持分页，这个主要是应用在BIRT Designer里的preview tab里，可以支持cancel操作，其它不怎么常用。采用Ajax，速度较慢。
<br/>preview --- 没有用到Ajax框架，直接调用底层Engine API对报表进行render，把生成的报表内容直接输出到浏览器。这种模式和run模式调用的是相同的Engine API，唯一区别在于run采用Ajax获取报表内容，而preview直接输出到浏览器。如果要支持分页，用户需要在URL上定义__page和 __pagerange参数，这两个参数也会在后面详细说明。需要特别说明的是，在这几种预览模式中，preview的速度是最快的。
<br/>document --- 该模式主要是为了从report design文件生成report document文件。用户可以在URL上提定document文件生成存放的路径(存放在server端)，如果未指定，会直接生成 rptdocument发送到客户端浏览器，用户可以下载到客户端。
<br/>output --- 该模式类似于frameset，会自动生成report document文件(预览report design文件)，区别在于output不采用Ajax，而是将生成的报表内容直接输出到浏览器。
<br/>parameter --- 该模式主要用于生成一个参数对话框，一般用户不常用，用户可以直接通过提供的JSP Tag--parameterPage去实现参数对话框，不需要直接调用。
<br/>download --- 用于导出报表数据为CSV格式，当你使用frameset工具条里的导出数据功能时，会用到这个模式。
<br/>2. web.xml里的参数设置<br/>
web.xml文件里有许多参数，用户应该根据自已的需求出发对这些参数有一个深入的了解。下面我会对这些参数一一做以说明。
<br/>[BIRT_VIEWER_LOCALE]
<br/>设置默认的Locale信息，暂时没有太大意义。因为Locale的信息，首先以URL上定义的__locale为准，如果没有定义，会找到当前浏览器的Locale信息，最后才会用到这里定义的信息。
<br/>[BIRT_VIEWER_WORKING_FOLDER]
<br/>设置BIRT Viewer的工作目录。用户可以把report design或是report document文件存放在这个目录下，这样就可以在URL上采用相对路径去预览这些报表文件了。默认是当前根目录。
<br/>当前支持三种形式：
<br/>相对路径 --- 这个相对当前的WEB应用的context root.
<br/>绝对路径
<br/>JAVA系统变量 --- 可以在启动服务器时，定义JVM的系统变量，如java –Dmyworkingfolder=D:/reports。这样就可以在web.xml中用${myworkingfolder}进行引用了。
<br/>[BIRT_VIEWER_DOCUMENT_FOLDER]
<br/>设置生成的document文件的存放路径。默认是documents目录。路径设置同上。
<br/>[WORKING_FOLDER_ACCESS_ONLY]
<br/>简单的报表访问限制控制实现，如果设为true,哪就只能预览存放在工作目录下的报表文件。默认值是false。
<br/>[BIRT_VIEWER_IMAGE_DIR]
<br/>设置生成的临时图片的存放路径。默认是report/images目录。路径设置同工作目录设置。
<br/>[BIRT_VIEWER_LOG_DIR]
<br/>设置生成的日志文件存放路径。默认是logs目录。路径设置同工作目录设置。
<br/>[BIRT_VIEWER_LOG_LEVEL]
<br/>设置日志的level，可选的值有：ALL|SEVERE|WARNING|INFO|CONFIG|FINE|FINER|FINEST|OFF。级别由高到低。
<br/>[BIRT_VIEWER_SCRIPTLIB_DIR]
<br/>设置用户script lib文件的存放目录( 在报表中用到的Java Event Handler Class )。默认值是scriptlib。路径设置同工作目录设置。
<br/>[BIRT_RESOURCE_PATH]
<br/>设置用户资源存放路径，这些资源包括library文件，image文件等。默认是当前根目录。路径设置同工作目录设置。
<br/>[BIRT_VIEWER_MAX_ROWS]
<br/>设置获取dataset的最大记录数。主要应用于设计报表的时候，预览报表如果记录数太多，会花费很多的时间，也可能会引起out of memory问题。默认是不限制。
<br/>[BIRT_VIEWER_MAX_CUBE_LEVELS]
<br/>设置CUBE查询的最大级数。和前面的参数作用类似。默认是不限制。
<br/>[BIRT_VIEWER_CUBE_MEMORY_SIZE]
<br/>设置在生成CUBE时，可以写在memory中的最大值，单位是MB。可以提高效率，写在内存会比直接写在硬盘快很多。但同时也要注意内存占用的问题。
<br/>[BIRT_OVERWRITE_DOCUMENT]
<br/>该参数主要用于frameset/output模式，它们会生成临时的document文件上。如果设为true，则每次刷新页面时，都会重新去生成document文件，如果为false，则不会重新生成，只会用原来的document文件去生成报表内容。
<br/>[BIRT_VIEWER_CONFIG_FILE]
<br/>定义properties文件的路径，不可以修改。
<br/>[BIRT_VIEWER_PRINT_SERVERSIDE]
<br/>在frameset工具条上，提供有后台服务器打印的功能，该参数可以设置是打开还是关闭后台打印的功能。默认是打开。可选值为: ON 和 OFF。
<br/>[HTML_ENABLE_AGENTSTYLE_ENGINE]
<br/>这个参数是会传递给Engine的，主要用于一些CSS的兼容性方面的问题。默认值是true。
<br/>3. viewer.properties参数设置
<br/>viewer.properties文件主要是定义一些扩展的参数。
<br/>/# configurable variable for JSP base href. Please uncomment the below line.
<br/>/#base_url=http://127.0.0.1:8080
<br/>该设置主要应用于代理服务器的情况下，在使用代理服务器后，从request里获取的URI并非真正的URI，需要在这里定义。
<br/>/# [EXTENSION SETTING]
<br/>viewer.extension.html=html
<br/>viewer.extension.pdf=pdf
<br/>viewer.extension.postscript=ps
<br/>viewer.extension.doc=doc
<br/>viewer.extension.xls=xls
<br/>viewer.extension.ppt=ppt
<br/>定义输出的报表文件的后缀名，和format相关联。
<br/>/# [OUTPUT FORMAT LABEL NAME]
<br/>viewer.label.html=HTML
<br/>viewer.label.pdf=PDF
<br/>viewer.label.postscript=PostScript
<br/>viewer.label.doc=Word
<br/>viewer.label.xls=Excel
<br/>viewer.label.ppt=PowerPoint
<br/>定义导出报表对话框里的报表格式列表，和format相关联，这样名字会更有意义。
<br/>/# [CSV SEPARATOR]
<br/>viewer.sep.0=,
<br/>viewer.sep.1=;
<br/>viewer.sep.2=:
<br/>viewer.sep.3=|
<br/>viewer.sep.4=\t
<br/>支持多种CSV分隔符，用户也可以增加新的分隔符(只支持char，而不是string)。但同时需要修改JSP文件和Messages.properties文件。
<br/>/# [LOGGERS]
<br/>/# "logger."+class=level
<br/>/# if no level is specified or the text "DEFAULT",
<br/>/# then the default level from the web.xml will be used
<br/>logger.org.eclipse.datatools.connectivity.oda=DEFAULT
<br/>logger.org.eclipse.datatools.enablement.oda=DEFAULT
<br/>当前的日志都是通过Engine输出的，如果需要其它组件的日志输出，可以在这里定义。注意格式必须为logger.org……。而且该组件必须实现了java的logger。
<br/>可以单独为该组件设置日志级别，如果设为DEFAULT，就会使用web.xml里的设置。
<br/>4. URL参数
<br/>下面是一些主要用到的URL参数。
<br/>“__report”
<br/>定义要预览的rptdesign文件路径，支持相对路径和绝对路径，相对路径是相对于web.xml中定义的工作目录。
<br/>“__document”
<br/>定义要预览的rptdocument文件路径，同样支持相对和绝对路径。相对路径是相对于web.xml中定义的工作目录。在同时定义了__report 和__document参数时，以__document为优先，如未找到相应的document文件，才会从design文件生成document文件 (frameset/output)或是直接去render这个design文件(preview/run)。
<br/>“__title”
<br/>定义报表显示的标题。
<br/>“__showtitle”
<br/>是否显示frameset模式下上方的标题部分。true | false
<br/>“__toolbar”
<br/>是否显示frameset模式下的工具条。true | false
<br/>“__navigationbar”
<br/>是否显示frameset模式下的导航条。true | false
<br/>“__parameterpage”
<br/>是否强制弹出或不弹出报表参数对话框。true | false
<br/>“__format”
<br/>输出报表的格式，默认为html。现在支持：pdf | doc | xls | postscript | ppt
<br/>“__locale”
<br/>设置Locale信息，如 __locale=zh_CN， 注意必须是国家加语言。
<br/>“__svg”
<br/>设置chart输出是否以SVG格式输出。true | false
<br/>frameset和run模式下，会采用javascript判断客户端浏览器是否支持svg，但并非对所有浏览器有效。
<br/>“__bookmark”
<br/>设置页面要定位的书签名字。
<br/>“__istoc”
<br/>指定定位的书签是不是一个TOC名字。如为true,就会根据__bookmark参数值去获取一个真正的书签名，从而实现正常的跳转。这个主要用于定位到一个TOC上。
<br/>“__rtl”
<br/>指定HTML页面输出是否需要right to left。支持不同国家的阅读习惯，如阿拉伯国家是从右到左的。
<br/>“__page”
<br/>指定要输出的报表页数，这个依赖于报表的分页设计(page break)。
<br/>“__pagerange”
<br/>指定要输出的报表页数范围。如1,3,5-9。
<br/>“__resourceFolder”
<br/>定义资源目录路径。同web.xml中的BIRT_RESOURCE_PATH设置。
<br/>“__asattachment”
<br/>是否以附件方式下载报表，如生成PDF或是其它格式里。默认是inline。
<br/>“__masterpage”
<br/>是否要显示master page。true | false
<br/>“__designer”
<br/>该参数主要是应用在BIRT Designer环境下，如会读取cache的报表参数等等，一般不用。true | false
<br/>“__overwrite”
<br/>该参数同web.xml定义的参数，不过web.xml里是全局设置，在URL上通过参数可以定义本次操作的设置。
<br/>“__imageID”
<br/>内部参数，用于image的引用，一般不用。
<br/>“__maxrows”
<br/>设置Dataset查询的最大记录数，要注意这个设置是全局的，会影响后面所有的请求。主要用于BIRT Designer下，提高报表设计效率。同web.xml中的BIRT_VIEWER_MAX_ROWS设置。
<br/>“__maxlevels”
<br/>设置查询获取Cube的最大级数。同上面的__maxrows，也主要用于BIRT Designer设计环境。
<br/>同web.xml中的BIRT_VIEWER_MAX_CUBE_LEVELS设置。
<br/>“__cubememsize”
<br/>同web.xml中的BIRT_VIEWER_CUBE_MEMORY_SIZE参数设置。
<br/>“__instanceid”
<br/>如果查看BIRT输出的HTML代码，你就可以看到一些HTML Element会有一个iid的属性(如table)，这个就是instanceid。这个是Engine动态生成的，不可提前预知。所以你需要从 HTML代码中得到这个值。该参数主要是为了获取reportlet(报表片断，如只输出报表中的一个Table或是一个Chart)。需要配合 __isreportlet参数。
<br/>“__isreportlet”
<br/>指定当前输出是不是一个reportlet。true | false
<br/>特别说明：为了输出一个reportlet，BIRT现在提供两种方式。
<br/>1． 为要输出的对象(表格或是Chart)定义一个bookmark，然后可以用下面的URL输出reportlet.
<br/>http://localhost:8080/birt/frameset?__report=test.rptdesign&__bookmark=bk&__isreportlet=true
<br/>2． 采用instanceid，但这个值事先是无法预知的，需要预览一次后从HTML代码中得到。然后用下面的URL输出reportlet.
<br/>http://localhost:8080/birt/output?__report=test.rptdesign&__instanceid=iid&__isreportlet=true
<br/>还有就是要注意，reportlet只支持document文档。如果是预览design文档去输出reportlet，就必须要使用frameset/output(自动生成document文档)。
<br/>“__clean”
<br/>BIRT里临时生成的一些文件都是和session相关的，比如临时document文件，还有image文件。这些文件也可以通过session进行管理，这个参数就是指定是否需要在session timeout的时候清除这些临时文件。默认值是true。
<br/>true | false
<br/>“__dpi”
<br/>可以设置输出Chart的dpi数值。
<br/>“__fittopage”
<br/>暂时这个参数只对PDF和postscript格式报表有效，指定是否调整至适合页面。
<br/>“__pagebreakonly”
<br/>暂时这个参数只对PDF和postscript格式报表有效，指定是否只采用BIRT报表内定的分页设置。这个参数一般需要和__fittopage联合使用。
<br/>“__agentstyle”
<br/>同web.xml中的HTML_ENABLE_AGENTSTYLE_ENGINE参数设置。
<br/>========================== 后台Server端打印相关参数 ==========================
<br/>“__action”
<br/>定义执行的指令名称。当前只支持print指令，用于后台服务器打印。
<br/>“__printer”
<br/>后台打印机名称。
<br/>“__printer_copies”
<br/>对应打印机的打印份数参数。
<br/>“__printer_collate”
<br/>对应打印机的双面打印参数。
<br/>“__printer_duplex”
<br/>对应打印机的duplex参数。
<br/>“__printer_mode”
<br/>对应打印机的模式参数。是单色还是彩色。
<br/>“__printer_pagesize”
<br/>对应打印机的纸型参数。比如A4。
<br/>===============================================================================
<br/>========================== JSP Tag相关参数 ===================================
<br/>“__id”
<br/>viewer的ID号，这个参数一般不常用，主要用于JSP Tag中，如在一个页面插入两个BIRT Viewer，而且预览同一个报表文件，这时候因为在一个session下面，所以需要用不同的ID去生成单独的document文件。不至于都生成同一个document文件上，从而引发冲突。
<br/>“__pattern”
<br/>在JSP Tag中用于指定要提交的Servlet Pattern名字，如frameset/output/run/preview等。主要用于采用parameter模式生成parameter dialog对话框时。
<br/>“__target”
<br/>可以指定提交到的窗口名称。如_blank,_self等。
<br/>“__nocache”
<br/>指定是否会用到cache的报表参数值，这些cache的值一般保存在rptconfig文件里。在设计报表并预览的时候，可以保存输入的报表参数值。这个在runtime的时候不常用。
<br/>===============================================================================
<br/>========================== 报表参数相关 ===================================
<br/>“__isnull”
<br/>指定当前的报表参数为null值，后面是报表的参数名。
<br/>“__islocale”
<br/>指定当前的报表参数值是和Locale/Format相关的，必须用特定的Locale/Format转化参数值(从String转化为Object)。格式为__islocale=paramName。
<br/>“__isdisplay__”
<br/>指定报表参数的displayText值,格式为__isdisplay__paramName=displayText。可以在报表中引用displayText值，如params[“p1”].displayText。
<br/>在URL上传displayText时如下(报表参数名为p1)：
<br/>&__isdisplay__p1=hello
<br/>“__islocale__”
<br/>指定该报表参数值是Locale/Format相关的，同时给定了参数值。格式为__islocale__paramName=paramValue。
<br/>===============================================================================
<br/>========================== Export Data参数 ===================================
<br/>“__exportEncoding”
<br/>该参数应用于导出数据为CSV中，可以指定导出的文件编码，如GBK或是GB2312等。
<br/>“__sep”
<br/>该参数应用于导出数据为CSV中，可以指定数据分隔符，如逗号，冒号等。
<br/>“__exportdatatype”
<br/>该参数应用于导出数据为CSV中，可以指定是否输出数据类型。true | false
<br/>“ResultSetName”
<br/>要导出数据的记录集名字。
<br/>“SelectedColumnNumber”
<br/>要导出的栏位数。
<br/>“SelectedColumn”
<br/>要导出的数据栏位名称。
<br/>具体可以查看BirtSimpleExportDataDialog.js文件。
<br/>===============================================================================
<br/>5. 其它参数设置
<br/>在BIRT Viewer里还有一个比较特殊的参数应用，就是用户可以自定义自已的servlet，然后传递对象到Application Context中，在报表中就可以从全局的Application Context去获取到这个对象。
<br/>这里相关的有两个内定的参数，AppContextKey和AppContextValue。下面是一个简单的示例。
```java
public void service( HttpServletRequest request,
HttpServletResponse response ) throws ServletException,IOException, BirtException {
    String myKeyName = "mykey";
    List values = new ArrayList();
    values.add( "hello" );
    values.add( new Date() );
    request.setAttribute( "AppContextKey", myKeyName );
    request.setAttribute( "AppContextValue", values );
    RequestDispatcher rd = request.getRequestDispatcher( "/frameset" );
    rd.include( request, response );
}
```
