---
title: 浅谈oracle中row_number() over()分析函数用法
date: 2017-09-25 17:08:14
tags: [oracle]
---
[文章来源:浅谈oracle中row_number() over()分析函数用法](http://blog.csdn.net/u011229848/article/details/78087198)

<br/>row_number()over(partition by col1 order by col2)表示根据col1分组，在分组内部根据col2排序，而此函数计算的值就表示每组内部排序后的顺序编号（组内连续的唯一的）。
<br/>与rownum的区别在于：使用rownum进行排序的时候是先对结果集加入伪劣rownum然后再进行排序，而此函数在包含排序从句后是先排序再计算行号码。
<br/>row_number()和rownum差不多，功能更强一点（可以在各个分组内从1开始排序）。
<br/>rank()是跳跃排序，有两个第二名时接下来就是第四名（同样是在各个分组内）
<br/>dense_rank()也是连续排序，有两个第二名时仍然跟着第三名。相比之下row_number是没有重复值的。
<br/>oracle 分析函数 row_number(),返回一个整数值(>=1);
<br/>语法格式:
<br/>1.row_number() over (order by col_1[,col_2 ...])
<br/>作用:按照col_1[,col_2 ...]排序,返回排序后的结果集，
<br/>此用法有点像rownum,为每一行返回一个不相同的值：
<br/>select rownum,ename,job,
<br/>row_number() over (order by rownum) row_number
<br/>from emp;
<br/>ROWNUM ENAME JOB ROW_NUMBER
<br/>---------- ---------- --------- ----------
<br/>1 SMITH CLERK 1
<br/>2 ALLEN SALESMAN 2
<br/>3 WARD SALESMAN 3
<br/>4 JONES MANAGER 4
<br/>5 MARTIN SALESMAN 5
<br/>6 BLAKE MANAGER 6
<br/>7 CLARK MANAGER 7
<br/>8 SCOTT ANALYST 8
<br/>9 KING PRESIDENT 9
<br/>10 TURNER SALESMAN 10
<br/>11 ADAMS CLERK 11
<br/>12 JAMES CLERK 12
<br/>13 FORD ANALYST 13
<br/>14 MILLER CLERK 14
<br/>如果没有partition by子句, 结果集将是按照order by 指定的列进行排序；
<br/>with row_number_test as(
<br/>select 22 a,'twenty two' b from dual union all
<br/>select 1,'one' from dual union all
<br/>select 13,'thirteen' from dual union all
<br/>select 5,'five' from dual union all
<br/>select 4,'four' from dual)
<br/>select a,b,
<br/>row_number() over (order by b)
<br/>from row_number_test
<br/>order by a;
<br/>正如我们所期待的,row_number()返回按照b列排序的结果,
<br/>然后再按照a进行排序,才得到下面的结果:
<br/>A B ROW_NUMBER()OVER(ORDERBYB)
<br/>-- ---------- --------------------------
<br/>1 one 3
<br/>4 four 2
<br/>5 five 1
<br/>13 thirteen 4
<br/>22 twenty two 5
<br/>2.row_number() over (partition by col_n[,col_m ...] order by col_1[,col_2 ...])
<br/>作用:先按照col_n[,col_m ...进行分组,
<br/>再在每个分组中按照col_1[,col_2 ...]进行排序(升序),
<br/>最后返回排好序后的结果集:
<br/>with row_number_test as(
<br/>select 22 a,'twenty two' b,'/*' c from dual union all
<br/>select 1,'one','+' from dual union all
<br/>select 13,'thirteen','/*' from dual union all
<br/>select 5,'five','+' from dual union all
<br/>select 4,'four','+' from dual)
<br/>select a,b,
<br/>row_number() over (partition by c order by b) row_number
<br/>from row_number_test
<br/>order by a;
<br/>这个例子中，我们先按照c列分组，分为2组('/*'组,'+'组)，
<br/>再按照每个小组的b列进行排序(按字符串首字母的ascii码排),
<br/>最后按照a列排序,得到下面的结果集:
<br/>A B ROW_NUMBER
<br/>-- ---------- ----------
<br/>1 one 3
<br/>4 four 2
<br/>5 five 1
<br/>13 thirteen 1
<br/>
<br/>22 twenty two
<br/>
<br/>本篇文章来源于 Linux公社网站(www.linuxidc.com) 原文链接：http://www.linuxidc.com/Linux/2011-04/34251.htm