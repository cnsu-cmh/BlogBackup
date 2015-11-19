---
title: oracle数据库密码有效期设置为无限制
date: 2017-10-23 11:05:04
tags: [oracle]
---
[文章来源:oracle数据库密码有效期设置为无限制](http://blog.csdn.net/u011229848/article/details/78316693)


今早数据共享的另一家单位打电话说查数据出错，一看是oracle密码过期,默认一般都是180天，修改为不限制。
```sql
SELECT username,PROFILE FROM dba_users;--查看用户的proifle(一般是default)

SELECT /* FROM dba_profiles s WHERE s.profile='DEFAULT' AND resource_name='PASSWORD_LIFE_TIME';--密码有效期

ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;--将密码有效期修改成“无限制”。
```

