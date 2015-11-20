---
title: spring在非controlle项目中获取当前request
date: 2017-11-08 10:43:51
tags: [java,spring]
---
[文章来源:spring在非controlle项目中获取当前request](http://blog.csdn.net/u011229848/article/details/78475956)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/78475956

## spring在非controlle项目中获取当前request
```java
public static HttpServletRequest getCurrentRequest() throws IllegalStateException {
    ServletRequestAttributes requestAttrs = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
    if (requestAttrs == null) {
        throw new IllegalStateException("当前线程中不存在 Request 上下文");
    }
    return requestAttrs.getRequest();
}
```