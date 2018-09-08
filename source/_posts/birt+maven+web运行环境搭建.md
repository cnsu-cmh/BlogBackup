---
title: birt+maven+web运行环境搭建
date: 2017-02-10 09:30:12
tags: [birt]
---

具体的项目配置，在这里不做过多的阐述，之前很多的文章都做了介绍，我这里直接给出结果：<br/>
第一步：下载报表环境birt-runtime-4_3_1，去官网下载，这里不做过多的阐述。<br/>
第二步：环境配置<br/>
1、解压缩，先拷贝WebViewerExample\WEB-INF下如下文件到工程的WEB-INF目录下：<br/>
jrun.web.xml<br/>
server-config.wsdd<br/>
viewer.properties<br/>
2、 将WebViewerExample\WEB-INF\tlds下的birt.tld文件拷贝到工程的\WEB-INF\tlds下<br/>
3、在工程的WEB-INF下建立report-engine的文件夹，在report-engine下新建如下四个文件夹<br/>
documents<br/>
images<br/>
logs<br/>
scriptlib<br/>
4、将WebViewerExample\ webcontent文件夹拷贝到要集成的WEB应用的根目录下（如果工程的web目录也叫WebContent的话会很奇怪，可以将webcontent改名，改名方法另开贴说明）。
<br/>5、在web应用的根目录下建立reportFiles文件夹，用来存放报表文件。
<br/>6、将web.xml中的如下内容拷贝到工程的web.xml中：
```xml
<display-name>Eclipse BIRT Report Viewer</display-name>
<!-- Default locale setting. -->
<context-param>
<param-name>BIRT_VIEWER_LOCALE</param-name>
<param-value>en-US</param-value>
</context-param>
<!--
Default timezone setting.
Examples: "Europe/Paris", "GMT+1".
Defaults to the container's timezone.
-->
<context-param>
<param-name>BIRT_VIEWER_TIMEZONE</param-name>
<param-value></param-value>
</context-param>
<!--
Report resources directory for preview. Defaults to ${birt home}
-->
<context-param>
<param-name>BIRT_VIEWER_WORKING_FOLDER</param-name>
<param-value></param-value>
</context-param>
<!-- Temporary document files directory. Defaults to ${birt home}/documents -->
<context-param>
<param-name>BIRT_VIEWER_DOCUMENT_FOLDER</param-name>
<param-value></param-value>
</context-param>
<!-- Flag whether the report resources can only be accessed under the
working folder. Defaults to true -->
<context-param>
<param-name>WORKING_FOLDER_ACCESS_ONLY</param-name>
<param-value>true</param-value>
</context-param>
<!-- Settings for how to deal with the url report path. e.g. "http://host/repo/test.rptdesign".
Following values are supported: <all> - All paths.
<domain> - Only the paths with host matches current domain. Note the comparison is literal, "127.0.0.1" and "localhost" are considered as different hosts.
<none> - URL paths are not supported.
Defaults to "domain".
-->
<context-param>
<param-name>URL_REPORT_PATH_POLICY</param-name>
<param-value>domain</param-value>
</context-param>
<!--
Temporary image/chart directory. Defaults to ${birt home}/report/images
-->
<context-param>
<param-name>BIRT_VIEWER_IMAGE_DIR</param-name>
<param-value></param-value>
</context-param>
<!-- Engine log directory. Defaults to ${birt home}/logs -->
<context-param>
<param-name>BIRT_VIEWER_LOG_DIR</param-name>
<param-value></param-value>
</context-param>
<!-- Report engine log level -->
<context-param>
<param-name>BIRT_VIEWER_LOG_LEVEL</param-name>
<param-value>WARNING</param-value>
</context-param>
<!--
Directory where to store all the birt report script libraries (JARs).
Defaults to ${birt home}/scriptlib
-->
<context-param>
<param-name>BIRT_VIEWER_SCRIPTLIB_DIR</param-name>
<param-value></param-value>
</context-param>
<!-- Resource location directory. Defaults to ${birt home} -->
<context-param>
<param-name>BIRT_RESOURCE_PATH</param-name>
<param-value></param-value>
</context-param>
<!-- Preview report rows limit. An empty value means no limit. -->
<context-param>
<param-name>BIRT_VIEWER_MAX_ROWS</param-name>
<param-value></param-value>
</context-param>
<!--
Max cube fetch levels limit for report preview (Only used when
previewing a report design file using the preview pattern)
-->
<context-param>
<param-name>BIRT_VIEWER_MAX_CUBE_ROWLEVELS</param-name>
<param-value></param-value>
</context-param>
<context-param>
<param-name>BIRT_VIEWER_MAX_CUBE_COLUMNLEVELS</param-name>
<param-value></param-value>
</context-param>
<!-- Memory size in MB for creating a cube. -->
<context-param>
<param-name>BIRT_VIEWER_CUBE_MEMORY_SIZE</param-name>
<param-value></param-value>
</context-param>
<!-- Defines the BIRT viewer configuration file -->
<context-param>
<param-name>BIRT_VIEWER_CONFIG_FILE</param-name>
<param-value>WEB-INF/viewer.properties</param-value>
</context-param>
<!--
Flag whether to allow server-side printing. Possible values are "ON"
and "OFF". Defaults to "ON".
-->
<context-param>
<param-name>BIRT_VIEWER_PRINT_SERVERSIDE</param-name>
<param-value>ON</param-value>
</context-param>
<!-- Flag whether to force browser-optimized HTML output. Defaults to true -->
<context-param>
<param-name>HTML_ENABLE_AGENTSTYLE_ENGINE</param-name>
<param-value>true</param-value>
</context-param>
<!-- Filename generator class/factory to use for the exported reports. -->
<context-param>
<param-name>BIRT_FILENAME_GENERATOR_CLASS</param-name>
<param-value>org.eclipse.birt.report.utility.filename.DefaultFilenameGenerator</param-value>
</context-param>
<!-- Viewer Filter used to set the request character encoding to UTF-8. -->
<filter>
<filter-name>ViewerFilter</filter-name>
<filter-class>org.eclipse.birt.report.filter.ViewerFilter</filter-class>
</filter>
<filter-mapping>
<filter-name>ViewerFilter</filter-name>
<servlet-name>ViewerServlet</servlet-name>
</filter-mapping>
<filter-mapping>
<filter-name>ViewerFilter</filter-name>
<servlet-name>EngineServlet</servlet-name>
</filter-mapping>
!-- Viewer Servlet Context Listener -->
<listener>
<listener-class>org.eclipse.birt.report.listener.ViewerServletContextListener</listener-class>
</listener>
<!-- Viewer HttpSession Listener -->
<listener>
<listener-class>org.eclipse.birt.report.listener.ViewerHttpSessionListener</listener-class>
</listener>
<!-- Viewer Servlet, Supports SOAP -->
<servlet>
<servlet-name>ViewerServlet</servlet-name>
<servlet-class>org.eclipse.birt.report.servlet.ViewerServlet</servlet-class>
</servlet>
<!-- Engine Servlet -->
<servlet>
<servlet-name>EngineServlet</servlet-name>
<servlet-class>org.eclipse.birt.report.servlet.BirtEngineServlet</servlet-class>
</servlet>
<servlet-mapping>
<servlet-name>ViewerServlet</servlet-name>
<url-pattern>/frameset</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>ViewerServlet</servlet-name>
<url-pattern>/run</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>EngineServlet</servlet-name>
<url-pattern>/preview</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>EngineServlet</servlet-name>
<url-pattern>/download</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>EngineServlet</servlet-name>
<url-pattern>/parameter</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>EngineServlet</servlet-name>
<url-pattern>/document</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>EngineServlet</servlet-name>
<url-pattern>/output</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>EngineServlet</servlet-name>
<url-pattern>/extract</url-pattern>
</servlet-mapping>
<jsp-config>
<taglib>
<taglib-uri>/birt.tld</taglib-uri>
<taglib-location>/WEB-INF/tlds/birt.tld</taglib-location>
</taglib>
</jsp-config>
```
其中web.xml文件需做如下修改：<br/>
a、修改BIRT_VIEWER_WORKING_FOLDER项的值为reportFiles;
<br/>b、修改BIRT_VIEWER_DOCUMENT_FOLDER项的值为WEB-INF/report-engine/documents;
<br/>c、修改BIRT_VIEWER_IMAGE_DIR项的值为WEB-INF/report-engine/images;
<br/>d、修改BIRT_VIEWER_LOG_DIR项的值为WEB-INF/report-engine/logs;
这里有个注意事项就是，这个日志文件如果配置了地址的话，到时候每次生成报表的时候，都会生成一批日志文件，会占用服务器资源。一个办法是定期的删除这个下面的文件，还有一个办法就是不配置日志文件地址，我就是采用第二种方法的。
<br/>e、修改BIRT_VIEWER_SCRIPTLIB_DIR项的值为WEB-INF/report-engine/scriptlib;
<br/>f、如果需调整日志级别可修改BIRT_VIEWER_LOG_LEVEL的值为ALL;
可选的值有：ALL|SEVERE|WARNING|INFO|CONFIG|FINE|FINER|FINEST|OFF。级别由高到低。
<br/>7、下面就是最重要的步骤了，Maven中jar包的引入，到底4.3.1需要引入多少包，以及哪些包，我目前做了配置，如下：
```xml
<dependency>
<groupId>org.eclipse.birt.runtime</groupId>
<artifactId>org.eclipse.birt.runtime</artifactId>
<version>4.3.1</version>
</dependency>
<dependency>
<groupId>commons-discovery</groupId>
<artifactId>axis</artifactId>
<version>1.4</version>
</dependency>
<dependency>
<groupId>commons-cli</groupId>
<artifactId>commons-cli</artifactId>
<version>1.0</version>
</dependency>
<dependency>
<groupId>javax.xml</groupId>
<artifactId>jaxrpc</artifactId>
<version>1.1</version>
</dependency>
<dependency>
<groupId>org.eclipse.birt.runtime</groupId>
<artifactId>com.ibm.icu</artifactId>
<version>50.1.1.v201304230130</version>
</dependency>
<dependency>
<groupId>org.eclipse.birt.runtime</groupId>
<artifactId>org.eclipse.core.runtime</artifactId>
<version>3.9.0.v20130326-1255</version>
</dependency>
<dependency>
<groupId>org.eclipse.core</groupId>
<artifactId>org.eclipse.core.runtime</artifactId>
<version>3.6.200.v20130402-1505</version>
</dependency>
<dependency>
<groupId>org.eclipse.birt.runtime</groupId>
<artifactId>org.eclipse.equinox.registry</artifactId>
<version>3.5.301.v20130717-1549</version>
</dependency>
<dependency>
<groupId>org.eclipse.birt.runtime</groupId>
<artifactId>org.eclipse.datatools.connectivity</artifactId>
<version>1.2.9.v201307261105</version>
</dependency>
<dependency>
<groupId>org.eclipse.birt.runtime</groupId>
<artifactId>org.eclipse.datatools.connectivity.oda</artifactId>
<version>3.4.1.v201308160907</version>
</dependency>
<dependency>
<groupId>org.eclipse.birt.runtime</groupId>
<artifactId>org.eclipse.datatools.connectivity.oda.consumer</artifactId>
<version>3.2.6.v201305170644</version>
</dependency>
<dependency>
<groupId>org.eclipse.birt.runtime</groupId>
<artifactId>org.eclipse.osgi</artifactId>
<version>3.9.1.v20130814-1242</version>
</dependency>
<dependency>
<groupId>com.hfmx</groupId>
<artifactId>Tidy</artifactId>
<version>1.0</version>
</dependency>
<dependency>
<groupId>commons-discovery</groupId>
<artifactId>commons-discovery</artifactId>
<version>0.2</version>
</dependency>
<dependency>
<groupId>org.eclipse.birt.runtime</groupId>
<artifactId>viewservlets</artifactId>
<version>4.3.1</version>
</dependency>
<dependency>
<groupId>org.eclipse.birt.runtime.3_7_1</groupId>
<artifactId>org.w3c.css.sac</artifactId>
<version>1.3.0</version>
</dependency>
<dependency>
<groupId>org.eclipse.birt.runtime.3_7_1</groupId>
<artifactId>org.apache.batik.css</artifactId>
<version>1.6.0</version>
</dependency>
<dependency>
<groupId>org.eclipse.birt.runtime.3_7_1</groupId>
<artifactId>org.apache.batik.util</artifactId>
<version>1.6.0</version>
</dependency>
<dependency>
<groupId>org.eclipse.birt.runtime.3_7_1</groupId>
<artifactId>org.apache.xerces</artifactId>
<version>2.9.0</version>
</dependency>
<dependency>
<groupId>org.eclipse.birt.runtime.3_7_1</groupId>
<artifactId>com.lowagie.text</artifactId>
<version>2.1.7</version>
</dependency>
```

所有的这些包，在下载的环境中都有，如果maven管理中没有的依赖的话，需要手动注册，注册方式如下：
mvn install:install-file -Dfile=e:/Tidy.jar -DgroupId=com.hfmx -DartifactId=Tidy -Dversion=1.0 -Dpackaging=jar -DgeneratePom=true