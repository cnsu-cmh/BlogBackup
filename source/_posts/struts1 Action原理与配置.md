---
title: struts1 Action原理与配置
date: 2016-11-18 10:05:32
tags: [java,struts1]
---
[文章来源:struts1 Action原理与配置](http://blog.csdn.net/u011229848/article/details/53212986)


今天临时调到另外一个项目帮忙，三天时间修改一个功能，e(⊙o⊙)…，刚down下项目一看是struts1+Hibernate3，平时一直采用spring+springMvc+springData+Hibernate，之前只接触过struts2还好久没用了，只好先来看一下struts1的action配置了，又因为时间有限有限只好摘抄他人文章了，哈哈

首先介绍下struts1工作原理：

1.初始化：struts框架的总控制器ActionServlet是一个Servlet，它在web.xml中配置成自动启动的

Servlet，在启动时总控制器会读取配置文件(struts-config.xml)的配置信息，为struts

中不同的模块初始化相应的对象。(面向对象思想)

2.发送请求：用户提交表单或通过URL向WEB服务器提交请求，请求的数据用HTTP协议传给web服务器。

3.form填充：struts的总控制器ActionServlet在用户提交请求时将数据放到对应的form对象中的成员

变量中。

4.派发请求：控制器根据配置信息对象ActionConfig将请求派发到具体的Action，对应的formBean一并

传给这个Action中的excute()方法。

5.处理业务：Action一般只包含一个excute()方法，它负责执行相应的业务逻辑(调用其它的业务模块)

完毕后返回一个ActionForward对象。服务器通过ActionForward对象进行转发工作。

6.返回响应：Action将业务处理的不同结果返回一个目标响应对象给总控制器。

7.查找响应：总控制器根据Action处理业务返回的目标响应对象，找到对应的资源对象，一般情况下

为jsp页面。

8.响应用户：目标响应对象将结果传递给资源对象，将结果展现给用户。

下面具体介绍一下struts1配置并配合简单的例子简单易懂：

Action, ActionForm, ActionForward ，这三个对象构成了Struts 的核心。
Struts 最核心的控制器是ActionServlet ，该Servlet 拦截用户请求，井将用户请求转入到Struts 体系内。
<!--more-->
一、配置ActionServlet
ActionServlet 是一个标准的Servlet ，在web.xml 文件中配置，该Servlet 用于拦所有的HTTP 请求。
在web.xml 文件中配置ActionServlet 应增加如下片段:

<servlet>
<!-- ActionServlet 的名 -->
<servlet-name>actionSevlet</servlet-name>
<!-- 配置Servlet 的实现类 -->
<servlet-class>
org.apache.struts.action.ActionServlet
</servlet-class>
<!-- 指定Struts 的第一个配置文件 -->
<init-param>
<!-- 指定配置文件的映射 -->
<param-name>config</param-name>
<param-value>/WEB-INF/struts-con工fgl.xml</param-value>
</init-param>
<!-- 指定Struts 的第二个配置文件 -->
<init-param>
<!-- 指定配置文件的映射 -->
<param-name>config/wawa</param-name>
<param-value>/WEB-INF/struts-config2.xml</param-value>
</init-param>
<!-- 将ActionServlet配置成自启动Servlet -->
<load-on-startup>2</load-on-startup>
</servlet>

二、配置ActionForm
配置ActionForm ，必须包含ActionForm 类才行。Struts 要求ActionForm 必须继承Struts 的基类: org.apache.struts.action.ActionForm，ActionForm 的实现非常简单，该类只是一个普通的JavaBean，只要为每个属性提供对应的setter 和getter 方法即可。根据前面的讲解， ActionForm 用于封装用户的请求参数，而请求参数是通过JSP 页面的表单域传递过来的。因此应保证ActionForm 的参数与表单域的名字相同。
注意: JavaB ean 的参数是根据getter 、setter 方法确定的。如果希望有一个A 的属性，则应该提供getA 和setA 的方法。
（1）ActionForm的实现
ActionForm 的属性必须与JSP 页面的表单域相同。本示例的表单包含如下两个表单域:
• usemame
• password
因此， ActionForm 必须继承org.apache.struts.action.ActionForm，并为这两个域提供对应的setter 和getter 方法，下面是ActionForm 的源代码:

import org.apache.struts.action.ActionForm;
public class LoginForm extends ActionForm {
private String username;
private String password;

// 表单域username对应的setter 方法
//*/*
/* @return the username
/*/
public String getUsername() {
return username;
}

//*/*
/* @param username
/* the username to set
/*/
public void setUsername(String username) {
this.username = username;
}

//*/*
/* @return the password
/*/
public String getPassword() {
return password;
}

//*/*
/* @param password
/* the password to set
/*/
public void setPassword(String password) {
this.password = password;
}

}

（2）ActionForm 的配置
所有的ActionForm 都被配置在struts-config.xml 文件中，该文件包括了一个form-beans 的元素，该元素内定义了所有的ActionForm，每个ActionForm 对应一个form-bean 元素。
为了定义LoginForm. 必须在struts-config.xml文件中增加如下代码:

<!-- 用于定义所有的ActionForm -->
<form-beans>
<!-- 定义ActionForm，至少指定两个属性: name , type-->
<form-bean name="loginForm" type="lee.LoginForm" />
</form-beans>

三、配置Action
Action 的配置比ActionForm 相对复杂一点，因为Action 负责管理与之关联的ActionForm. 它不仅需要配置实现类，还需要配置Action 的path 属性，该属性用于被用
户请求。对于只需在本Action 内有效的Forward. 还应在Action 元素内配置局部Forward。
（1）Action 的实现
通过ActionForm. 可使Action 无须从HTTP 请求中解析参数。因为所有的参数都被封装在ActionForm中，下面是Action 从AcitionForm 取得请求参数的源代码:

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.apache.struts.action.Action;
import org.apache.struts.action.ActionForm;
import org.apache.struts.action.ActionForward;
import org.apache.struts.action.ActionMapping;
public class LoginAction extends Action {
// 必须重写该核心方法，该方法actionForm 将表单的请求参数封装成值对象
public ActionForward execute(ActionMapping mapping, ActionForm form,
HttpServletRequest request, HttpServletResponse response)
throws Exception {
//将ActionForm强制类型转换为LoginForm
LoginForm loginForm = (LoginForm) form;
// 从ActionForm中解析出请求参数: username
String username = loginForm.getUsername();
// 从ActionForm中解析出请求参数: password
String pass = loginForm.getUsername();
//后面的处理与前一个示例的Action 相同。
...
}
}

该Action 从转发过来的ActionForm 中解析请求参数，对应的ActionForm 则由ActionServlet 在接收到用户请求时，负责实例化。
实际的过程是: ActionServlet 拦截到用户请求后，根据用户的请求，在配置文件中查找对应的Action ， Action 的name 属性指定了用于封装请求参数的ActionForm; 然后ActionServlet 将创建默认的ActionForm 实例，并调用对应的setter 方法完成ActionForm的初始化。
ActionServlet 在分发用户请求时，也将对应ActionForm 的实例一同分发过来。
（2）Action 的配置
Action 需要配置如下几个方面。
• Action 的path: ActionServlet 根据该属性来转发用户的请求，即将用户请求转发与之同名的Action 。同名的意思是:将请求的.do 后缀去掉，匹配Action 的path属性值。
• Action 的name: 此处的name 属性并不是Action 本身的名字，而是与Action 关联的ActionForm。因此该name 属性必须是前面存在的ActionForm 名。
• Action 的type: 该属性用于指定Action 的实现类，也就是负责处理用户请求的业
务控制器。
• 局部Forward: Action 的转发并没有转发到实际的JSP 资源，而是转发到逻辑名，即Forward 名。在Action 内配置的Forward 都是局部Forward (该Forward 只在该Action 内有效)。
下面是该Action 的配置代码:

<!-- 该元素里配置所有的Action -->
<action-mappings>
<!-- 配置Action. 指定了path ， name ， type 等属性 -->
<action path="/login" type="lee.LoginAction" name="loginForm">
<!-- 配置局部Forward -->
<forward name="welcome" path="/WEB-INF/jsp/welcome.jsp" />
<forward name="input" path="/login.jsp" />
</action>
</action-mappings>

四、配置Forward
正如前面所讲， Forward 分局部Forward 和全局Forward 两种。前者在Action 里配置，仅对该Action 有效:后者单独配置，对所有的Action 都有效。
配置Forward 非常简单，主要需要指定以下三个属性。
• name: 该Forward 的逻辑名。
• path: 该Forward 映射到的JSP 资源。
• redirect: 是否使用重定向。
局部Forward 作为Action 的子元素配置;全局Forward 配置在global-forwards 元素里。
下面是配置全局Forward 的代码:

<!-- 配置全局Forward -->
<global-forwards>
<!-- 配置Forward对象的name 和path 属性 -->
<forward name="error" path="/WEB-INF/jsp/error.jsp" />
</global-forwards>

上面的配置代码中，配置了一个全局Forward，该Forward 可以被所有的Action 访问。通常，只将全局资源配置成全局Forward。当每个Action 在转发时，首先在局部Forward 中查找与之对应的Forward，如果在局部Forward 中找不到对应的Forward 对象，才会到全局Forward 中查找。因此，局部Forward 可以覆盖全局Forward。
下面提供了该应用的struts-config.xm1文件的全部源代码:

<?xml version="1.0" encoding="UTF-8"?>
<!-- Struts 配置文件的文件头，包含DTD 等信息 -->
<!DOCTYPE struts-config PUBLIC "-//Apache Software Foundation//DTD Struts Configuration 1.2//EN" "http://struts.apache.org/dtds/struts-config_1_2.dtd">
<!--Struts 配置文件的根元素 -->
<struts-config>
<!--配置所有的ActionForm -->
<form-beans>
<!--配置第一个ActionForm，指定ActionForm的name 和type 属性 -->
<form-bean name="loginForm" type="lee.LoginForm" />
</form-beans>
<!--配置全局Forward对象 -->
<global-forwards>
<!--该Forward对象的name 属性为error. 映射资源为/WEB-INF/jsp/error.jsp -->
<forward name="error" path="/WEB-INF/jsp/error.jsp" />
</global-forwards>
<!--此处配置所有的Action 映射-->
<action-mappings>
<!--配置Action 的path. type 属性name 属性配置Action 对应的ActionForm-->
<action path="/login" type="lee.LoginAction" name="loginForm">
<!--还配置了两个局部Forward. 这两个局部Forward仅对该Action有效-->
<forward name="welcome" path="/WEB-INF/jsp/welcome.jsp" />
<forward name="input" path="/login.jsp" />
</action>
</action-mappings>
</struts-config>

配置文件详解如下：

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts-config PUBLIC
"-//Apache Software Foundation//DTD Struts Configuration 1.1//EN"
"[http://jakarta.apache.org/struts/dtds/struts-config.dtd](http://jakarta.apache.org/struts/dtds/struts-config.dtd)">
<!-- struts-config.xml中的元素必须按照上述doc指令中的dtd文档定义顺序书写，本例即遵从了dtd定义顺序 -->
<!-- struts-config是整个xml的根元素，其他元素必须被包含其内 -->
<struts-config>
<!--
名称:data-sources
描述：data-sources元素定义了web App所需要使用的数据源
数量：最多一个
子元素:data-source
-->
<data-sources>
<!--
名称：data-source
描述：data-source元素定义了具体的数据源
数量：任意多个
属性：
@key：当需要配置多个数据源时，相当于数据源的名称，用来数据源彼此间进行区别
@type：可以使用的数据源实现的类，一般来自如下四个库
Poolman，开放源代码软件
Expresso，Jcorporate
JDBC Pool，开放源代码软件
DBCP，Jakarta
-->
<data-source key="firstOne" type="org.apache.commons.dbcp.BasicDataSource">
<!--
名称：set-property
描述：用来设定数据源的属性
属性：
@autoCommit:是否自动提交 可选值：true/false
@description:数据源描述
@driverClass:数据源使用的类
@maxCount:最大数据源连接数
@minCount:最小数据源连接数
@user:数据库用户
@password:数据库密码
@url:数据库url
-->
<set-property property="autoCommit" value="true"/>
<set-property property="description" value="Hello!"/>
<set-property property="driverClass" value="com.mysql.jdbc.Driver"/>
<set-property property="maxCount" value="10"/>
<set-property property="minCount" value="2"/>
<set-property property="user" value="root"/>
<set-property property="password" value=""/>
<set-property property="url" value="jdbc:mysql://localhost:3306/helloAdmin"/>
</data-source>
</data-sources>

<!--
名称：form-beans
描述：用来配置多个ActionForm Bean
数量：最多一个
子元素：form-bean
-->
<form-beans>
<!--
名称：form-bean
描述：用来配置ActionForm Bean
数量：任意多个
子元素：form-property
属性：
@className：指定与form-bean元素相对应的配置类，一般默认使用org.apaceh.struts.config.FormBeanConfig，如果自定义，则必须继承 FormBeanConfig
@name：必备属性！为当前form-bean制定一个全局唯一的标识符，使得在整个Struts框架内，可以通过该标识符来引用这个ActionForm Bean。
@type：必备属性！指明实现当前ActionForm Bean的完整类名。
-->
<form-bean name="Hello" type="myPack.Hello">
<!--
名称：form-property
描述：用来设定ActionForm Bean的属性
数量：根据实际需求而定，例如，ActionForm Bean对应的一个登陆Form中有两个文本框，name和password，ActionForm Bean中也有这两个字段，则此处编写两个form-property来设定属性
属性：
@className：指定与form-property相对应的配置类，默认是org.apache.struts.config.FormPropertyConfig，如果自定义，则必须继承FormPropertyConfig类
@name：所要设定的ActionForm Bean的属性名称
@type：所要设定的ActionForm Bean的属性值的类
@initial：当前属性的初值
-->
<form-property name="name" type="java.lang.String"/>
<form-property name="number" type="java.lang.Iteger" initial="18"/>
</form-bean>
</form-beans>

<!--
名称：global-exceptions
描述：处理异常
数量：最多一个
子元素：exception
-->
<global-exceptions>
<!--
名称：exception
描述：具体定义一个异常及其处理
数量：任意多个
属性：
@className:指定对应exception的配置类，默认为org.apache.struts.config.ExceptionConfig
@handler:指定异常处理类，默认为org.apache.struts.action.ExceptionHandler
@key:指定在Resource Bundle种描述该异常的消息key
@path:指定当发生异常时，进行转发的路径
@scope:指定ActionMessage实例存放的范围，默认为request，另外一个可选值是session
@type:必须要有！指定所需要处理异常类的名字。
@bundle:指定资源绑定
-->
<exception
key=""hello.error
path="/error.jsp"
scope="session"
type="hello.HandleError"/>
</global-exceptions>

<!--
名称：global-forwards
描述：定义全局转发
数量：最多一个
子元素：forward
-->
<global-forwards>
<!--
名称：forward
描述：定义一个具体的转发
数量：任意多个
属性：
@className:指定和forward元素对应的配置类，默认为org.apache.struts.action.ActionForward
@contextRelative:如果为true，则指明使用当前上下文，路径以“/”开头，默认为false
@name:必须配有！指明转发路径的唯一标识符
@path:必须配有！指明转发或者重定向的URI。必须以"/"开头。具体配置要与contextRelative相应。
@redirect:为true时，执行重定向操作，否则执行请求转发。默认为false
-->
<forward name="A" path="/a.jsp"/>
<forward name="B" path="/hello/b.do"/>
</global-forwards>

<!--
名称：action-mappings
描述：定义action集合
数量：最多一个
子元素：action
-->
<action-mappings>
<!--
名称：action
描述：定义了从特定的请求路径到相应的Action类的映射
数量：任意多个
子元素：exception,forward（二者均为局部量）
属性：
@attribute:制定与当前Action相关联的ActionForm Bean在request和session范围内的名称（key）
@className:与Action元素对应的配置类。默认为org.apache.struts.action.ActionMapping
@forward:指名转发的URL路径
@include:指名包含的URL路径
@input:指名包含输入表单的URL路径，表单验证失败时，请求会被转发到该URL中
@name:指定和当前Acion关联的ActionForm Bean的名字。该名称必须在form-bean元素中定义过。
@path:指定访问Action的路径，以"/"开头，没有扩展名
@parameter:为当前的Action配置参数，可以在Action的execute()方法中，通过调用ActionMapping的getParameter()方法来获取参数
@roles:指定允许调用该Aciton的安全角色。多个角色之间用逗号分割。处理请求时，RequestProcessor会根据该配置项来决定用户是否有调用该Action的权限
@scope:指定ActionForm Bean的存在范围，可选值为request和session。默认为session
@type:指定Action类的完整类名
@unknown:值为true时，表示可以处理用户发出的所有无效的Action URL。默认为false
@validate:指定是否要先调用ActionForm Bean的validate()方法。默认为true
注意：如上属性中，forward/include/type三者相斥，即三者在同一Action配置中只能存在一个。
-->
<action path="/search"
type="addressbook.actions.SearchAction"
name="searchForm"
scope="request"
validate="true"
input="/search.jsp">
<forward name="success" path="/display.jsp"/>
</action>
</action-mappings>

<!--
名称：controller
描述：用于配置ActionServlet
数量：最多一个
属性：
@bufferSize:指定上传文件的输入缓冲的大小.默认为4096
@className:指定当前控制器的配置类.默认为org.apache.struts.config.ControllerConfig
@contentType:指定相应结果的内容类型和字符编码
@locale:指定是否把Locale对象保存到当前用户的session中,默认为false
@processorClass:指定负责处理请求的Java类的完整类名.默认org.apache.struts.action.RequestProcessor
@tempDir:指定文件上传时的临时工作目录.如果没有设置,将才用Servlet容器为web应用分配的临时工作目录.
@nochache:true时,在相应结果中加入特定的头参数:Pragma ,Cache-Control,Expires防止页面被存储在可数浏览器的缓存中,默认为false
-->
<controller
contentType="text/html;charset=UTF-8"
locale="true"
processorClass="CustomRequestProcessor">
</controller>
<!--
名称:message-resources
描述:配置Resource Bundle.
数量:任意多个
属性:
@className:指定和message-resources对应的配置类.默认为org.apache.struts.config.MessageResourcesConfig
@factory:指定资源的工厂类,默认为org.apache.struts.util.PropertyMessageResourcesFactory
@key:
@null:
@parameter:
-->
<message-resources
null="false"
parameter="defaultResource"/>
<message-resources
key="images"
null="false"
parameter="ImageResources"/>

<!--
名称:plug-in
描述:用于配置Struts的插件
数量:任意多个
子元素:set-property
属性:
@className:指定Struts插件类.此类必须实现org.apache.struts.action.PlugIn接口
-->
<plug-in
className="org.apache.struts.validator.ValidatorPlugIn">
<!--
名称:set-property
描述:配置插件的属性
数量:任意多个
属性:
@property:插件的属性名称
@value:该名称所配置的值
-->
<set-property
property="pathnames"
value="/WEB-INF/validator-rules.xml,/WEB-INF/vlaidation.xml"/>
</plug-in>

</struts-config>
一、为struts配置web.xml
1,配置ActionServlet(only one)，使其接收应用程序收到的所有请求
分为两步，a:使用servlet元素配置servlet实例，做servlet-mapping
<web-app>
<servlet>
<servlet-name>storefront</servlet-name>
<servlet-class>完全限定的类名</servlet-class>
</servlet>
<servlet-mapping>
<servlet-name>storefront</servlet-name>
<url-pattern>/*.do</url-pattern>
</servlet-mapping>
</web-map>
2,配置初始化参数：init-param，以name/value表示<param-name><param-value>
config :默认为/WEB-INF/struts-config.xml
config/sub1:config/... 从附加的struts配置文件中加在资程序sub1
debug:servlet的调试detail
detail：Digester的调试detail
convertHack
3,<taglib>使用struts提供的标记库时必须配置包括
<taglib-uri>识别web应用程序所使用的标记库，必须是有效的
<taglib-location>指定了标记库描述文件的位置
4，<welcome-file-list>配置在web app中输入有效的，但不完整的url所使用的default resource；不使用servlet映射
<welcome-file>起始和结束都没有/符号
5，<error-page>
(<error-code> <location>)
<<exception-type><location>
</error-page>

二、Struts配置文件
ApplicationConfig: 包含了struts配置文件中的所有信息
1, <data-source>
<set-property property=““ value=““/>
<data-source>
2,<form-beans>
<form-bean name=“loginForm“ type=“完全限定的类名，是ActionForm的子类“>
<form-property name=““ type=““/>
</form-bean>
<form-bean
</form-beans>
3,<global-exceptions>
4,<global-forwards>

**在Struts1.3中已经取消了<data-sources>标签，也就是说只能在1.2版中配置，因为Apache不推荐在struts-config.xml中配置数据源。所以建议不要在struts中配置数据源，如果你用了hibernate或spring得话就可以在hibernate配置文件或spring文件配数据源如果都没用就到tomcat中配置**