---
title: BIRT集成WEB项目
date: 2017-02-09 14:15:32
tags: [birt]
---

从项目要用到BIRT工具开始在坛子里泡了好久，从0开始慢慢琢磨现在终于可以把BIRT集成到项目中来运行了，当中的过程还真有点艰难，但这也是每个学习BIRT工具的人都遇到的，现在把中间的些许过程贴出来，希望对初次学习BIRT的人有些帮助。
一、BIRT与工程的集成。
1、从eclipse官网下载birt的运行包（地址：[http://www.eclipse.org/birt/](http://www.eclipse.org/birt/)），解压缩，先拷贝WebViewerExample\WEB-INF下如下文件到工程的WEB-INF目录下：<br/>
jrun.web.xml<br/>
server-config.wsdd<br/>
viewer.properties<br/>
2、将WebViewerExample\WEB-INF\tlds下的birt.tld文件拷贝到工程的\WEB-INF\tlds下。<br/><!--more-->
3、在工程的WEB-INF下建立report-engine的文件夹，在report-engine下新建如下四个文件夹<br/>
documents
images
logs
scriptlib<br/>
4、将WebViewerExample\ webcontent文件夹拷贝到要集成的WEB应用的根目录下（如果工程的web目录也叫WebContent的话会很奇怪，可以将webcontent改名，改名方法另开贴说明）。<br/>
5、在web应用的根目录下建立reportFiles文件夹，用来存放报表文件。<br/>
6、将web.xml(web-template)中的如下内容拷贝到工程的web.xml中：

```html

1. <display-name>Eclipse BIRT Report Viewer</display-name>
1.
1.
1. <!-- Default locale setting.
1. -->
1. <context-param>
1. <param-name>BIRT_VIEWER_LOCALE</param-name>
1. <param-value>en-US</param-value>
1. </context-param>
1.
1.
1. <!--
1. Default timezone setting.
1. Examples: "Europe/Paris", "GMT+1".
1. Defaults to the container's timezone.
1. -->
1. <context-param>
1. <param-name>BIRT_VIEWER_TIMEZONE</param-name>
1. <param-value></param-value>
1. </context-param>
1.
1. <!--
1. Report resources directory for preview. Defaults to ${birt home}
1. -->
1. <context-param>
1. <param-name>BIRT_VIEWER_WORKING_FOLDER</param-name>
1. <param-value></param-value>
1. </context-param>
1.
1. <!--
1. Temporary document files directory. Defaults to ${birt home}/documents
1. -->
1. <context-param>
1. <param-name>BIRT_VIEWER_DOCUMENT_FOLDER</param-name>
1. <param-value></param-value>
1. </context-param>
1.
1.
1. <!--
1. Flag whether the report resources can only be accessed under the
1. working folder. Defaults to true
1. -->
1. <context-param>
1. <param-name>WORKING_FOLDER_ACCESS_ONLY</param-name>
1. <param-value>true</param-value>
1. </context-param>
1.
1.
1. <!--
1. Settings for how to deal with the url report path. e.g. "http://host/repo/test.rptdesign".
1.
1. Following values are supported:
1.
1. <all>- All paths.
1. <domain>- Only the paths with host matches current domain. Note the comparison is literal, "127.0.0.1" and "localhost" are considered as different hosts.
1. <none>- URL paths are not supported.
1.
1. Defaults to "domain".
1. -->
1. <context-param>
1. <param-name>URL_REPORT_PATH_POLICY</param-name>
1. <param-value>domain</param-value>
1. </context-param>
1.
1.
1. <!--
1. Temporary image/chart directory. Defaults to ${birt home}/report/images
1. -->
1. <context-param>
1. <param-name>BIRT_VIEWER_IMAGE_DIR</param-name>
1. <param-value></param-value>
1. </context-param>
1.
1.
1. <!-- Engine log directory. Defaults to ${birt home}/logs -->
1. <context-param>
1. <param-name>BIRT_VIEWER_LOG_DIR</param-name>
1. <param-value></param-value>
1. </context-param>
1.
1.
1. <!-- Report engine log level -->
1. <context-param>
1. <param-name>BIRT_VIEWER_LOG_LEVEL</param-name>
1. <param-value>WARNING</param-value>
1. </context-param>
1.
1.
1. <!--
1. Directory where to store all the birt report script libraries (JARs).
1. Defaults to ${birt home}/scriptlib
1. -->
1. <context-param>
1. <param-name>BIRT_VIEWER_SCRIPTLIB_DIR</param-name>
1. <param-value></param-value>
1. </context-param>
1.
1. <!-- Resource location directory. Defaults to ${birt home} -->
1. <context-param>
1. <param-name>BIRT_RESOURCE_PATH</param-name>
1. <param-value></param-value>
1. </context-param>
1.
1.
1. <!-- Preview report rows limit. An empty value means no limit. -->
1. <context-param>
1. <param-name>BIRT_VIEWER_MAX_ROWS</param-name>
1. <param-value></param-value>
1. </context-param>
1.
1.
1. <!--
1. Max cube fetch levels limit for report preview (Only used when
1. previewing a report design file using the preview pattern)
1. -->
1. <context-param>
1. <param-name>BIRT_VIEWER_MAX_CUBE_ROWLEVELS</param-name>
1. <param-value></param-value>
1. </context-param>
1. <context-param>
1. <param-name>BIRT_VIEWER_MAX_CUBE_COLUMNLEVELS</param-name>
1. <param-value></param-value>
1. </context-param>
1.
1.
1. <!-- Memory size in MB for creating a cube. -->
1. <context-param>
1. <param-name>BIRT_VIEWER_CUBE_MEMORY_SIZE</param-name>
1. <param-value></param-value>
1. </context-param>
1.
1.
1. <!-- Defines the BIRT viewer configuration file -->
1. <context-param>
1. <param-name>BIRT_VIEWER_CONFIG_FILE</param-name>
1. <param-value>WEB-INF/viewer.properties</param-value>
1. </context-param>
1.
1.
1. <!--
1. Flag whether to allow server-side printing. Possible values are "ON"
1. and "OFF". Defaults to "ON".
1. -->
1. <context-param>
1. <param-name>BIRT_VIEWER_PRINT_SERVERSIDE</param-name>
1. <param-value>ON</param-value>
1. </context-param>
1.
1.
1. <!--
1. Flag whether to force browser-optimized HTML output. Defaults to true
1. -->
1. <context-param>
1. <param-name>HTML_ENABLE_AGENTSTYLE_ENGINE</param-name>
1. <param-value>true</param-value>
1. </context-param>
1.
1.
1. <!--
1. Filename generator class/factory to use for the exported reports.
1. -->
1. <context-param>
1. <param-name>BIRT_FILENAME_GENERATOR_CLASS</param-name>
1. <param-value>org.eclipse.birt.report.utility.filename.DefaultFilenameGenerator</param-value>
1. </context-param>
1.
1.
1. <!--
1. Viewer Filter used to set the request character encoding to UTF-8.
1. -->
1. <filter>
1. <filter-name>ViewerFilter</filter-name>
1. <filter-class>org.eclipse.birt.report.filter.ViewerFilter</filter-class>
1. </filter>
1. <filter-mapping>
1. <filter-name>ViewerFilter</filter-name>
1. <servlet-name>ViewerServlet</servlet-name>
1. </filter-mapping>
1. <filter-mapping>
1. <filter-name>ViewerFilter</filter-name>
1. <servlet-name>EngineServlet</servlet-name>
1. </filter-mapping>
1.
1. <!-- Viewer Servlet Context Listener -->
1. <listener>
1. <listener-class>org.eclipse.birt.report.listener.ViewerServletContextListener</listener-class>
1. </listener>
1.
1.
1. <!-- Viewer HttpSession Listener -->
1. <listener>
1. <listener-class>org.eclipse.birt.report.listener.ViewerHttpSessionListener</listener-class>
1. </listener>
1.
1. <!-- Viewer Servlet, Supports SOAP -->
1. <servlet>
1. <servlet-name>ViewerServlet</servlet-name>
1. <servlet-class>org.eclipse.birt.report.servlet.ViewerServlet</servlet-class>
1. </servlet>
1.
1.
1. <!-- Engine Servlet -->
1. <servlet>
1. <servlet-name>EngineServlet</servlet-name>
1. <servlet-class>org.eclipse.birt.report.servlet.BirtEngineServlet</servlet-class>
1. </servlet>
1.
1.
1. <servlet-mapping>
1. <servlet-name>ViewerServlet</servlet-name>
1. <url-pattern>/frameset</url-pattern>
1. </servlet-mapping>
1.
1. <servlet-mapping>
1. <servlet-name>ViewerServlet</servlet-name>
1. <url-pattern>/run</url-pattern>
1. </servlet-mapping>
1.
1. <servlet-mapping>
1. <servlet-name>EngineServlet</servlet-name>
1. <url-pattern>/preview</url-pattern>
1. </servlet-mapping>
1.
1.
1. <servlet-mapping>
1. <servlet-name>EngineServlet</servlet-name>
1. <url-pattern>/download</url-pattern>
1. </servlet-mapping>
1.
1.
1. <servlet-mapping>
1. <servlet-name>EngineServlet</servlet-name>
1. <url-pattern>/parameter</url-pattern>
1. </servlet-mapping>
1.
1.
1. <servlet-mapping>
1. <servlet-name>EngineServlet</servlet-name>
1. <url-pattern>/document</url-pattern>
1. </servlet-mapping>
1.
1.
1. <servlet-mapping>
1. <servlet-name>EngineServlet</servlet-name>
1. <url-pattern>/output</url-pattern>
1. </servlet-mapping>
1.
1. <servlet-mapping>
1. <servlet-name>EngineServlet</servlet-name>
1. <url-pattern>/extract</url-pattern>
1. </servlet-mapping>
1.
1.
1. <jsp-config>
1. <taglib>
1. <taglib-uri>/birt.tld</taglib-uri>
1. <taglib-location>/WEB-INF/tlds/birt.tld</taglib-location>
1. </taglib>
1. </jsp-config>
```
其中web.xml文件需做如下修改：<br/>
a、修改BIRT_VIEWER_WORKING_FOLDER项的值为reportFiles;<br/>
b、修改BIRT_VIEWER_DOCUMENT_FOLDER项的值为WEB-INF/report-engine/documents;<br/>
c、修改BIRT_VIEWER_IMAGE_DIR项的值为WEB-INF/report-engine/images;<br/>
d、修改BIRT_VIEWER_LOG_DIR项的值为WEB-INF/report-engine/logs;<br/>
e、修改BIRT_VIEWER_SCRIPTLIB_DIR项的值为WEB-INF/report-engine/scriptlib;<br/>
f、如果需调整日志级别可修改BIRT_VIEWER_LOG_LEVEL的值为ALL;<br/>
可选的值有：ALL|SEVERE|WARNING|INFO|CONFIG|FINE|FINER|FINEST|OFF。级别由高到低。<br/>
7、拷贝jar包，这一步放最后是因为我对示例工程中的jar包进行了清理。<br/>
我用的是最新版的BIRT 4.2.2，从官网下的部署包，论坛里的大部分人的集成方法是将“WebViewerExample\WEB-INF\lib”中的jar包全部拷到工程的lib目录下，说实话，这里的包实在是太多了，4.2.2 runtime下的jar包有86个47.8兆，这么多的jar包全部拷贝到工程下的话造成工程里面有很多冗余的jar包，也造成了工程的庞大，相信中也是很多人在项目中遇到的问题，工程中有很多冗余的jar包，但是有不敢删除。
我用最土的办法将birt集成到项目中的办法就是，先将配置文件配置好，不拷贝jar包到工程，然后每次启动，根据启动日志的错误提示信息找到缺失的类所在的jar包，然后将对应的jar包拷贝到工程中，再次启动，以此类推，知道工程启动和报表展示没有错误为止，得到的运行birt所必须的jar清单如下：<br/>
axis.jar
com.ibm.icu_4.4.2.v20110823.jar
com.lowagie.text_2.1.7.v201004222200.jar
commons-cli-1.0.jar
commons-discovery-0.2.jar
jaxrpc.jar
js.jar
org.apache.batik.css_1.6.0.v201011041432.jar
org.apache.batik.util_1.6.0.v201011041432.jar
org.apache.xerces_2.9.0.v201101211617.jar
org.eclipse.birt.runtime_4.2.2.v20130216-1152.jar
org.eclipse.core.runtime_3.8.0.v20120912-155025.jar
org.eclipse.datatools.connectivity.oda.consumer_3.2.5.v201109151100.jar
org.eclipse.datatools.connectivity.oda_3.3.4.v201212070447.jar
org.eclipse.datatools.connectivity_1.2.7.v201302060508.jar
org.eclipse.equinox.common_3.6.100.v20120522-1841.jar
org.eclipse.equinox.registry_3.5.200.v20120522-1841.jar
org.eclipse.osgi_3.8.2.v20130124-134944.jar
org.w3c.css.sac_1.3.0.v200805290154.jar
Tidy.jar
viewservlets.jar
derby.jar<br/>
从86个精简到22个，工程下不用到那么多无用的jar包了，其中derby.jar这个包单独拿出来是因为这个包可要可不要，因为这个是示例工程中的数据源，但我们的项目中一般都不需要用到示例工程中的报表数据源，所以我没有拷贝这个jar包。
至此通过如上步骤已经将birt报表集成到我们的项目中来了。