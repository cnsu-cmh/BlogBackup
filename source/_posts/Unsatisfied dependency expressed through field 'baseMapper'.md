---
title: Unsatisfied dependency expressed through field 'baseMapper'
date: 2018-08-16 22:38:11
tags: [java,mybatis,mybatis-plus,springboot]
---
[文章来源:Unsatisfied dependency expressed through field 'baseMapper'](http://blog.csdn.net/u011229848/article/details/81750005)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/81750005

今天项目springboot1.5.8升级到2.0.4,以至于将Mybatis-Plus 升级，springBoot启动之后报错Unsatisfied dependency expressed through field 'baseMapper'，详细错误信息如下
<!--more-->
```
org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'userController': Unsatisfied dependency expressed through field 'userService'; nested exception is org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'userServicesImpl': Unsatisfied dependency expressed through field 'baseMapper'; nested exception is org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'userMapper' defined in file [E:\IntelliJIDEAProject\xsblog\target\classes\com\xsit\blog\mapper\UserMapper.class]: Unsatisfied dependency expressed through bean property 'sqlSessionFactory'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'sqlSessionFactory' defined in class path resource [com/baomidou/mybatisplus/autoconfigure/MybatisPlusAutoConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.apache.ibatis.session.SqlSessionFactory]: Factory method 'sqlSessionFactory' threw exception; nested exception is org.springframework.core.NestedIOException: Failed to parse mapping resource: 'file [E:\IntelliJIDEAProject\xsblog\target\classes\mapper\UserMapper.xml]'; nested exception is java.lang.NullPointerException
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:586)
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:91)
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessPropertyValues(AutowiredAnnotationBeanPostProcessor.java:372)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1341)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:572)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:495)
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:317)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:222)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:315)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:759)
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:869)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:550)
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:140)
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:762)
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:398)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:330)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1258)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1246)
	at com.xsit.blog.BlogApplication.main(BlogApplication.java:10)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.springframework.boot.devtools.restart.RestartLauncher.run(RestartLauncher.java:49)
Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'userServicesImpl': Unsatisfied dependency expressed through field 'baseMapper'; nested exception is org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'userMapper' defined in file [E:\IntelliJIDEAProject\xsblog\target\classes\com\xsit\blog\mapper\UserMapper.class]: Unsatisfied dependency expressed through bean property 'sqlSessionFactory'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'sqlSessionFactory' defined in class path resource [com/baomidou/mybatisplus/autoconfigure/MybatisPlusAutoConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.apache.ibatis.session.SqlSessionFactory]: Factory method 'sqlSessionFactory' threw exception; nested exception is org.springframework.core.NestedIOException: Failed to parse mapping resource: 'file [E:\IntelliJIDEAProject\xsblog\target\classes\mapper\UserMapper.xml]'; nested exception is java.lang.NullPointerException
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:586)
	at org.springframework.beans.factory.annotation.InjectionMetadata.inject(InjectionMetadata.java:91)
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor.postProcessPropertyValues(AutowiredAnnotationBeanPostProcessor.java:372)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1341)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:572)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:495)
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:317)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:222)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:315)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199)
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:251)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1135)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1062)
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:583)
	... 24 common frames omitted
Caused by: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'userMapper' defined in file [E:\IntelliJIDEAProject\xsblog\target\classes\com\xsit\blog\mapper\UserMapper.class]: Unsatisfied dependency expressed through bean property 'sqlSessionFactory'; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'sqlSessionFactory' defined in class path resource [com/baomidou/mybatisplus/autoconfigure/MybatisPlusAutoConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.apache.ibatis.session.SqlSessionFactory]: Factory method 'sqlSessionFactory' threw exception; nested exception is org.springframework.core.NestedIOException: Failed to parse mapping resource: 'file [E:\IntelliJIDEAProject\xsblog\target\classes\mapper\UserMapper.xml]'; nested exception is java.lang.NullPointerException
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.autowireByType(AbstractAutowireCapableBeanFactory.java:1436)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.populateBean(AbstractAutowireCapableBeanFactory.java:1323)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:572)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:495)
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:317)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:222)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:315)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199)
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:251)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1135)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1062)
	at org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor$AutowiredFieldElement.inject(AutowiredAnnotationBeanPostProcessor.java:583)
	... 37 common frames omitted
Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'sqlSessionFactory' defined in class path resource [com/baomidou/mybatisplus/autoconfigure/MybatisPlusAutoConfiguration.class]: Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.apache.ibatis.session.SqlSessionFactory]: Factory method 'sqlSessionFactory' threw exception; nested exception is org.springframework.core.NestedIOException: Failed to parse mapping resource: 'file [E:\IntelliJIDEAProject\xsblog\target\classes\mapper\UserMapper.xml]'; nested exception is java.lang.NullPointerException
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:590)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.instantiateUsingFactoryMethod(AbstractAutowireCapableBeanFactory.java:1247)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBeanInstance(AbstractAutowireCapableBeanFactory.java:1096)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:535)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:495)
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:317)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:222)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:315)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199)
	at org.springframework.beans.factory.config.DependencyDescriptor.resolveCandidate(DependencyDescriptor.java:251)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.doResolveDependency(DefaultListableBeanFactory.java:1135)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.resolveDependency(DefaultListableBeanFactory.java:1062)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.autowireByType(AbstractAutowireCapableBeanFactory.java:1421)
	... 48 common frames omitted
Caused by: org.springframework.beans.BeanInstantiationException: Failed to instantiate [org.apache.ibatis.session.SqlSessionFactory]: Factory method 'sqlSessionFactory' threw exception; nested exception is org.springframework.core.NestedIOException: Failed to parse mapping resource: 'file [E:\IntelliJIDEAProject\xsblog\target\classes\mapper\UserMapper.xml]'; nested exception is java.lang.NullPointerException
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:185)
	at org.springframework.beans.factory.support.ConstructorResolver.instantiateUsingFactoryMethod(ConstructorResolver.java:582)
	... 60 common frames omitted
Caused by: org.springframework.core.NestedIOException: Failed to parse mapping resource: 'file [E:\IntelliJIDEAProject\xsblog\target\classes\mapper\UserMapper.xml]'; nested exception is java.lang.NullPointerException
	at com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean.buildSqlSessionFactory(MybatisSqlSessionFactoryBean.java:607)
	at com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean.afterPropertiesSet(MybatisSqlSessionFactoryBean.java:390)
	at com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean.getObject(MybatisSqlSessionFactoryBean.java:630)
	at com.baomidou.mybatisplus.autoconfigure.MybatisPlusAutoConfiguration.sqlSessionFactory(MybatisPlusAutoConfiguration.java:173)
	at com.baomidou.mybatisplus.autoconfigure.MybatisPlusAutoConfiguration$$EnhancerBySpringCGLIB$$66011e08.CGLIB$sqlSessionFactory$0(<generated>)
	at com.baomidou.mybatisplus.autoconfigure.MybatisPlusAutoConfiguration$$EnhancerBySpringCGLIB$$66011e08$$FastClassBySpringCGLIB$$c01ce6e1.invoke(<generated>)
	at org.springframework.cglib.proxy.MethodProxy.invokeSuper(MethodProxy.java:228)
	at org.springframework.context.annotation.ConfigurationClassEnhancer$BeanMethodInterceptor.intercept(ConfigurationClassEnhancer.java:361)
	at com.baomidou.mybatisplus.autoconfigure.MybatisPlusAutoConfiguration$$EnhancerBySpringCGLIB$$66011e08.sqlSessionFactory(<generated>)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.springframework.beans.factory.support.SimpleInstantiationStrategy.instantiate(SimpleInstantiationStrategy.java:154)
	... 61 common frames omitted
Caused by: java.lang.NullPointerException: null
	at com.baomidou.mybatisplus.core.toolkit.LambdaUtils.createLambdaMap(LambdaUtils.java:103)
	at com.baomidou.mybatisplus.core.toolkit.LambdaUtils.createCache(LambdaUtils.java:72)
	at com.baomidou.mybatisplus.core.toolkit.TableInfoHelper.initTableInfo(TableInfoHelper.java:258)
	at com.baomidou.mybatisplus.core.injector.AbstractMethod.inject(AbstractMethod.java:80)
	at com.baomidou.mybatisplus.core.injector.AbstractSqlInjector.lambda$inspectInject$28(AbstractSqlInjector.java:56)
	at java.util.ArrayList.forEach(ArrayList.java:1249)
	at com.baomidou.mybatisplus.core.injector.AbstractSqlInjector.inspectInject(AbstractSqlInjector.java:56)
	at com.baomidou.mybatisplus.core.MybatisMapperAnnotationBuilder.parse(MybatisMapperAnnotationBuilder.java:149)
	at com.baomidou.mybatisplus.core.MybatisMapperRegistry.addMapper(MybatisMapperRegistry.java:86)
	at com.baomidou.mybatisplus.core.MybatisConfiguration.addMapper(MybatisConfiguration.java:109)
	at org.apache.ibatis.builder.xml.XMLMapperBuilder.bindMapperForNamespace(XMLMapperBuilder.java:413)
	at org.apache.ibatis.builder.xml.XMLMapperBuilder.parse(XMLMapperBuilder.java:94)
	at com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean.buildSqlSessionFactory(MybatisSqlSessionFactoryBean.java:605)
	... 74 common frames omitted


```

仔细看错误什么也没发现，最后空指针更是奇怪，把官网源码下载下来运行居然成功，作对比后发现一个问题，项目中之前引入的包中主键用的@Id 注解，升级之后用的是@TableId注解，用官网demo试了一下，还果真如此。

如果你是用的boot,个人建议mubatis,mybatisplus那一堆堆包全都不要了，只要用
mybatis-plus-boot-starter **这一个**starter**包即可。**
