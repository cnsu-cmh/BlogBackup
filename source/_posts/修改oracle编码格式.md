---
title: 修改oracle编码格式
date: 2018-09-04 15:31:25
tags: [oracle]
---
[文章来源:修改oracle编码格式](http://blog.csdn.net/qq_1017097573/article/details/82384022)


文章参照：[https://www.jb51.net/article/53078.htm](https://www.jb51.net/article/53078.htm)

1.查看oracle当前编码格式：
SELECT * FROM V$NLS_PARAMETERS WHERE PARAMETER = 'NLS_CHARACTERSET' ; SELECT USERENV('language') FROM DUAL;

NLS_CHARACTERSET AL32UTF8

SIMPLIFIED CHINESE_CHINA.AL32UTF8
<!--more-->
2.以sysdba身份登录

3.关闭数据库 shutdown immediate;

4.以mount打来数据库，startup mount

5.设置session
SQL>ALTER SYSTEM ENABLE RESTRICTED SESSION; SQL> ALTER SYSTEM SET JOB_QUEUE_PROCESSES=0; SQL> ALTER SYSTEM SET AQ_TM_PROCESSES=0;

6.启动数据库

alter database open;

7.修改字符集

ALTER DATABASE CHARACTER SET ZHS16GBK;

提示我们的字符集：新字符集必须为旧字符集的超集:

ALTER DATABASE character set INTERNAL_USE ZHS16GBK;

8.关闭，重新启动

shutdown immediate;

startup