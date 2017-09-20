---
title: 解决Cannot change version of project facet Dynamic web module to 3.0
date: 2017-11-08 15:20:41
tags: [java,eclipse]
---
[文章来源:解决Cannot change version of project facet Dynamic web module to 3.0](http://blog.csdn.net/u011229848/article/details/78479168)


# 问题描述

用Eclipse创建Maven结构的web项目的时候选择了Artifact Id为maven-artchetype-webapp，由于这个catalog比较老，用的servlet还是2.3的，而一般现在都是用3.0，在Project Facets里面修改Dynamic web module为3.0的时候就会出现Cannot change version of project facet Dynamic web module to 3.0，如图：

其实在右边可以看到改到3.0需要的条件以及有冲突的facets，4

# 解决这个问题的步骤如下：

## 1.把Servlet改成3.0，打开项目的web.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" version="3.0">
</web-app>
```
## 2.修改项目的设置，在Navigator下打开项目.settings目录下的org.eclipse.jdt.core.prefs
<!--more-->
把1.5改成1.8
```
eclipse.preferences.version=1
org.eclipse.jdt.core.compiler.codegen.inlineJsrBytecode=enabled
org.eclipse.jdt.core.compiler.codegen.targetPlatform=1.8
org.eclipse.jdt.core.compiler.compliance=1.8
org.eclipse.jdt.core.compiler.problem.assertIdentifier=error
org.eclipse.jdt.core.compiler.problem.enumIdentifier=error
org.eclipse.jdt.core.compiler.problem.forbiddenReference=warning
org.eclipse.jdt.core.compiler.source=1.8
```
## 3. 打开org.eclipse.wst.common.component

<?xml version="1.0" encoding="UTF-8"?><project-modules id="moduleCoreId" project-version="1.8.0">
    <wb-module deploy-name="storm">
        <wb-resource deploy-path="/" source-path="/target/m2e-wtp/web-resources"/>
        <wb-resource deploy-path="/" source-path="/src/main/webapp" tag="defaultRootSource"/>
        <wb-resource deploy-path="/WEB-INF/classes" source-path="/src/main/java"/>
        <wb-resource deploy-path="/WEB-INF/classes" source-path="/src/main/resources"/>
        <wb-resource deploy-path="/WEB-INF/classes" source-path="/src/test/java"/>
        <wb-resource deploy-path="/WEB-INF/classes" source-path="/src/test/resources"/>
        <property name="context-root" value="storm"/>
        <property name="java-output-path" value="/storm/target/classes"/>
    </wb-module>
</project-modules>
## 4. 打开org.eclipse.wst.common.project.facet.core.xml

把1.5改成1.8
```xml
<?xml version="1.0" encoding="UTF-8"?>
<faceted-project>
  <fixed facet="wst.jsdt.web"/>
  <installed facet="jst.web" version="3.0"/>
  <installed facet="wst.jsdt.web" version="1.0"/>
  <installed facet="java" version="1.8"/>
</faceted-project>
```

转载自: https://my.oschina.net/cloudcoder/blog/362949