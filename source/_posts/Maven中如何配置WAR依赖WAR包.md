---
title: Maven中如何配置WAR依赖WAR包
date: 2017-10-11 09:15:11
tags: [maven]
---
[文章来源:Maven中如何配置WAR依赖WAR包](http://blog.csdn.net//u011229848/article/details/78199846)


**项目背景：**

1.war项目C和war项目B都依赖war项目A和JAR项目D. 项目A中保存了B和C项目通用的web资源,比如通用的[JavaScript](http://lib.csdn.net/base/javascript "JavaScript知识库"),CSS,jsp等. 项目D中保存了B和C项目中都依赖的一些class

2.开发人员希望每次都只面对一个项目,即Team A 开发项目A, Team B开发项目B, Team C开发项目C，以此类推

3.每个Team在开发自己项目时,都希望能直接进行调试,例如war项目A可以直接部署到TOMCAT上运行[测试](http://lib.csdn.net/base/softwaretest "软件测试知识库")

4.最后实际交付给客户的却只有2个项目: B和C 。也就是说,最后要打包B和C,而在B和C的war包中都要包含A中的web资源和D中的class
<!--more-->
**解决思路：**

使用maven-warpath-plugin插件来解决WAR所依赖的WAR

**解决方案：**
```xml


<dependency>

<groupId>com.isoftstone.gads</groupId>

<artifactId>common-web</artifactId>

<version>0.0.1-SNAPSHOT</version>

<type>war</type>

</dependency>

<dependency>

<groupId>com.isoftstone.gads</groupId>

<artifactId>common-web</artifactId>

<version>0.0.1-SNAPSHOT</version>

<type>warpath</type>

</dependency>

<plugin>

<groupId>org.apache.maven.plugins</groupId>

<artifactId>maven-war-plugin</artifactId>

<version>2.1-beta-1</version>

<configuration>
<!--默认会变成在target/war/work导致被打包进war文件,指定后为target/work-->

<workDirectory>${project.build.directory}/work</workDirectory>

<webappDirectory>WebContent</webappDirectory>

<useCache>false</useCache>

<archive>

<addMavenDescriptor>true</addMavenDescriptor>

</archive>

<overlays>

<overlay>

<groupId>com.isoftstone.gads</groupId>

<artifactId>ebiz-common-web</artifactId>

</overlay>

<overlay>

<!-- empty groupId/artifactId is detected as the current build -->

<!--代表当前WAR项目，默认情况下当前WAR项目是先被拷贝的，如果要控制其顺序，则使用空的overlay -->

<!-- any other overlay will be applied after the current build since they have not been configured in the overlays

element -->

</overlay>

</overlays> <dependentWarExcludes>/*/web.xml,**WEB-INF/lib//***,/sql-map-config.xml,/jdbc.properties,/META-INF//*</dependentWarExcludes>

</configuration>

</plugin>

<plugin>

<groupId>org.appfuse.plugins</groupId>

<artifactId>**maven-warpath-plugin**</artifactId>

<version>2.1.0-M1</version>

<extensions>true</extensions>

<executions>

<execution>

<goals>

<goal>add-classes</goal>

</goals>

</execution>

</executions>

<configuration><!-- below WEB-INF/classes -->

<warpathExcludes>/*/*/logback-test.xml</warpathExcludes>

</configuration>

</plugin>
```

1.首先是使用了maven-warpath-plugin插件,处理所有<type>warpath</type>的artifact,这个插件可以将从依赖的WAR中传递的依赖都打包到当前的WAR中,没有这个插件时,当前WAR从所依赖的WAR artifact那所传递来的依赖在打包成WAR时都会被忽略.既然现在能将传递的依赖打包了,就不用copy依赖的war中的WEB-INF/lib//*,所以被加入到<dependentWarExcludes>中

2.<workDirectory>的设置看我写的注释

3.webappDirectory的指定需要额外注意.首先,我使用了MAVEN默认的资源路径,也就是 src/main/webapp,而这里却告诉maven-war-plugin另一个路径"WebContent",产生的结果就是,执行mvn package时,war-plugin和warpath-plugin会将当前war和所有依赖的war的web资源都拷贝到WebContent目录下.这样,WebContent目录包含的内容就是最终打包成WAR的内容了.

参考:http://blog.csdn.net/kobejayandy/article/details/8143930