---
title: MybatisPlus QueryWrapper and or 连用
date: 2018-08-21 10:08:03
tags: [java,mybatis,mybatis-plus]
---
[文章来源:MybatisPlus QueryWrapper and or 连用](http://blog.csdn.net//u011229848/article/details/81902398)

```java
    QueryWrapper<User> userWrapper = new QueryWrapper<>();
    String type = (String) map.get("type");
    if(StringUtils.isNotBlank(type)) {
        userWrapper.eq("is_admin", "admin".equals(type) ? true : false);
    }
    String keys = (String) map.get("key");
    if(StringUtils.isNotBlank(keys)) {
        userWrapper.and(wrapper -> wrapper.like("login_name", keys).or().like("tel", keys).or().like("email", keys));
    }
```
控制台sql打印为：

```sql
SELECT
        id,
        login_name AS loginName,
        nick_name AS nickName,
        password,
        salt,
        tel,
        email,
        locked,
        is_admin AS adminUser,
        icon,
        create_by AS createId,
        create_date AS createDate,
        update_by AS updateId,
        update_date AS updateDate,
        del_flag AS delFlag,
        remarks 
    FROM
        sys_user 
    WHERE
        is_admin = 0 
        AND (
            login_name LIKE '%j%' 
            OR tel LIKE '%j%' 
            OR email LIKE '%j%' 
        )
```
切记不能丢了and

```java
    userWrapper.like("login_name", keys).or().like("tel", keys).or().like("email", keys)
    userWrapper.eq("is_admin", "admin".equals(type) ? true : false);
```
这个条件是不带括号的

WHERE
is_admin = 0
AND login_name LIKE '%j%'
OR tel LIKE '%j%'
OR email LIKE '%j%'

### **<font color="red">or(Function<This, This> func), and(Function<This, This> func) 会为func返回的条件添加括号 or(sql...) and(...)</font>**