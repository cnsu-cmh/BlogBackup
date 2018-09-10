---
title: oracle-强制停止job
date: 2018-05-22 17:50:49
tags: [oracle]
---
[文章来源:oracle-强制停止job](http://blog.csdn.net/u011229848/article/details/80407738)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/80407738

**1.查询出正在执行的job,**
```sql
select * from dba_jobs_running;
```


**2.停止该job**
```sql
declare
begin
DBMS_JOB.BROKEN(27,true); --27为查出的job
end;
```
<!--more-->
**3.根据sid查询出session信息**
```sql
select SID,SERIAL/# from V$Session where SID='9';
```
**4.kill session**
```sql
alter system kill session '9,43767';
```
**ora-00031:标记要终止的会话**
```sql
select spid, osuser, s.program from v$process p, v$session s where p.addr=s.paddr and s.sid = 9;
```
命令行 orakill ORCL 9208; (ORCL：实例名 9208：刚查到的线程号)

远程通过命令行开启局域网计算机的远程桌面服务(Description = RPC 服务器不可用 可能原因135 445端口未开启)

Wmic /node:"[200.200.200.207]" /USER:"Administrator" PAT

win32_terminalservicesetting WHERE (__Class!="") CALL SetAllowTSConnections 1