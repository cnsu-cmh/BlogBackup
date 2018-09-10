---
title: StringUtils
date: 2016-11-16 00:30:41
tags: [java]
---
[文章来源:StringUtils](http://blog.csdn.net/u011229848/article/details/53180890)


StringUtils 方法的操作对象是 java.lang.String 类型的对象，是 JDK 提供的 String 类型操作方法的补充，并且是 null 安全的(即如果输入参数 String 为 null 则不会抛出  NullPointerException ，而是做了相应处理，例如，如果输入为 null 则返回也是 null 等，具体可以查看源代码)。

除了构造器，StringUtils 中一共有130多个方法，并且都是 static 的，所以我们可以这样调用 StringUtils.xxx()

下面分别对一些常用方法做简要介绍：

1.public static boolean isEmpty(String str)
   <br/>判断某字符串是否为空，为空的标准是 str==null 或 str.length()==0
   <br/>下面是 StringUtils 判断是否为空的示例：
<!--more-->
    <br/>StringUtils.isEmpty(null) = true
    <br/>StringUtils.isEmpty("") = true
    <br/>StringUtils.isEmpty(" ") = false //注意在 StringUtils 中空格作非空处理
    <br/>StringUtils.isEmpty("   ") = false
    <br/>StringUtils.isEmpty("bob") = false
    <br/>StringUtils.isEmpty(" bob ") = false

 

2.public static boolean isNotEmpty(String str)
   <br/>判断某字符串是否非空，等于 !isEmpty(String str)
   <br/>下面是示例：

      <br/>StringUtils.isNotEmpty(null) = false
      <br/>StringUtils.isNotEmpty("") = false
      <br/>StringUtils.isNotEmpty(" ") = true
      <br/>StringUtils.isNotEmpty("         ") = true
      <br/>StringUtils.isNotEmpty("bob") = true
      <br/>StringUtils.isNotEmpty(" bob ") = true

3.public static boolean isBlank(String str)
   <br/>判断某字符串是否为空或长度为0或由空白符(whitespace) 构成
   <br/>下面是示例：
     <br/>StringUtils.isBlank(null) = true
     <br/>StringUtils.isBlank("") = true
     <br/>StringUtils.isBlank(" ") = true
     <br/>StringUtils.isBlank("        ") = true
     <br/>StringUtils.isBlank("\t \n \f \r") = true   //对于制表符、换行符、换页符和回车符

     <br/>StringUtils.isBlank()   //均识为空白符
     <br/>StringUtils.isBlank("\b") = false   //"\b"为单词边界符
     <br/>StringUtils.isBlank("bob") = false
     <br/>StringUtils.isBlank(" bob ") = false

4.public static boolean isNotBlank(String str)
   <br/>判断某字符串是否不为空且长度不为0且不由空白符(whitespace) 构成，等于 !isBlank(String str)
   <br/>下面是示例：

      <br/>StringUtils.isNotBlank(null) = false
      <br/>StringUtils.isNotBlank("") = false
      <br/>StringUtils.isNotBlank(" ") = false
      <br/>StringUtils.isNotBlank("         ") = false
      <br/>StringUtils.isNotBlank("\t \n \f \r") = false
      <br/>StringUtils.isNotBlank("\b") = true
      <br/>StringUtils.isNotBlank("bob") = true
      <br/>StringUtils.isNotBlank(" bob ") = true

5.public static String trim(String str)
   <br/>去掉字符串两端的控制符(control characters, char <= 32) , 如果输入为 null 则返回null
   <br/>下面是示例：
      <br/>StringUtils.trim(null) = null
      <br/>StringUtils.trim("") = ""
      <br/>StringUtils.trim(" ") = ""
      <br/>StringUtils.trim("  \b \t \n \f \r    ") = ""
      <br/>StringUtils.trim("     \n\tss   \b") = "ss"
      <br/>StringUtils.trim(" d   d dd     ") = "d   d dd"
      <br/>StringUtils.trim("dd     ") = "dd"
      <br/>StringUtils.trim("     dd       ") = "dd"

6.public static String trimToNull(String str)
   <br/>去掉字符串两端的控制符(control characters, char <= 32) ,如果变为 null 或""，则返回 null
   <br/>下面是示例：
      <br/>StringUtils.trimToNull(null) = null
      <br/>StringUtils.trimToNull("") = null
      <br/>StringUtils.trimToNull(" ") = null
      <br/>StringUtils.trimToNull("     \b \t \n \f \r    ") = null
      <br/>StringUtils.trimToNull("     \n\tss   \b") = "ss"
      <br/>StringUtils.trimToNull(" d   d dd     ") = "d   d dd"
      <br/>StringUtils.trimToNull("dd     ") = "dd"
      <br/>StringUtils.trimToNull("     dd       ") = "dd"

7.public static String trimToEmpty(String str)
   <br/>去掉字符串两端的控制符(control characters, char <= 32) ,如果变为 null 或 "" ，则返回 ""
   <br/>下面是示例：
      <br/>StringUtils.trimToEmpty(null) = ""
      <br/>StringUtils.trimToEmpty("") = ""
      <br/>StringUtils.trimToEmpty(" ") = ""
      <br/>StringUtils.trimToEmpty("     \b \t \n \f \r    ") = ""
      <br/>StringUtils.trimToEmpty("     \n\tss   \b") = "ss"
      <br/>StringUtils.trimToEmpty(" d   d dd     ") = "d   d dd"
      <br/>StringUtils.trimToEmpty("dd     ") = "dd"
      <br/>StringUtils.trimToEmpty("     dd       ") = "dd"

8.public static String strip(String str)
   <br/>去掉字符串两端的空白符(whitespace) ，如果输入为 null 则返回 null
   <br/>下面是示例(注意和 trim() 的区别)：
      <br/>StringUtils.strip(null) = null
      <br/>StringUtils.strip("") = ""
      <br/>StringUtils.strip(" ") = ""
      <br/>StringUtils.strip("     \b \t \n \f \r    ") = "\b"
      <br/>StringUtils.strip("     \n\tss   \b") = "ss   \b"
      <br/>StringUtils.strip(" d   d dd     ") = "d   d dd"
      <br/>StringUtils.strip("dd     ") = "dd"
      <br/>StringUtils.strip("     dd       ") = "dd"

9.public static String stripToNull(String str)
   <br/>去掉字符串两端的空白符(whitespace) ，如果变为 null 或""，则返回 null
   <br/>下面是示例(注意和 trimToNull() 的区别)：
      <br/>StringUtils.stripToNull(null) = null
      <br/>StringUtils.stripToNull("") = null
      <br/>StringUtils.stripToNull(" ") = null
      <br/>StringUtils.stripToNull("     \b \t \n \f \r    ") = "\b"
      <br/>StringUtils.stripToNull("     \n\tss   \b") = "ss   \b"
      <br/>StringUtils.stripToNull(" d   d dd     ") = "d   d dd"
      <br/>StringUtils.stripToNull("dd     ") = "dd"
      <br/>StringUtils.stripToNull("     dd       ") = "dd"

10.public static String stripToEmpty(String str)
    <br/>去掉字符串两端的空白符(whitespace) ，如果变为 null 或"" ，则返回""
    <br/>下面是示例(注意和 trimToEmpty() 的区别)：
     <br/>StringUtils.stripToNull(null) = ""
     <br/>StringUtils.stripToNull("") = ""
     <br/>StringUtils.stripToNull(" ") = ""
     <br/>StringUtils.stripToNull("     \b \t \n \f \r    ") = "\b"
     <br/>StringUtils.stripToNull("     \n\tss   \b") = "ss   \b"
     <br/>StringUtils.stripToNull(" d   d dd     ") = "d   d dd"
     <br/>StringUtils.stripToNull("dd     ") = "dd"
     <br/>StringUtils.stripToNull("     dd       ") = "dd"

以下方法只介绍其功能，不再举例：
11.public static String strip(String str, String stripChars)
   <br/>去掉 str 两端的在 stripChars 中的字符。
   <br/>如果 str 为 null 或等于"" ，则返回它本身；
   <br/>如果 stripChars 为 null 或"" ，则返回 strip(String str) 。

12.public static String stripStart(String str, String stripChars)
   <br/> 和11相似，去掉 str 前端的在 stripChars 中的字符。

13.public static String stripEnd(String str, String stripChars)
    <br/>和11相似，去掉 str 末端的在 stripChars 中的字符。

14.public static String[] stripAll(String[] strs)
    <br/>对字符串数组中的每个字符串进行 strip(String str) ，然后返回。
    <br/>如果 strs 为 null 或 strs 长度为0，则返回 strs 本身

15.public static String[] stripAll(String[] strs, String stripChars)
    <br/>对字符串数组中的每个字符串进行 strip(String str, String stripChars) ，然后返回。
    <br/>如果 strs 为 null 或 strs 长度为0，则返回 strs 本身

16.public static boolean equals(String str1, String str2)
    <br/>比较两个字符串是否相等，如果两个均为空则也认为相等。

17.public static boolean equalsIgnoreCase(String str1, String str2)
    <br/>比较两个字符串是否相等，不区分大小写，如果两个均为空则也认为相等。

18.public static int indexOf(String str, char searchChar)
    <br/>返回字符 searchChar 在字符串 str 中第一次出现的位置。
    <br/>如果 searchChar 没有在 str 中出现则返回-1，
    <br/>如果 str 为 null 或 "" ，则也返回-1

19.public static int indexOf(String str, char searchChar, int startPos)
    <br/>返回字符 searchChar 从 startPos 开始在字符串 str 中第一次出现的位置。
    <br/>如果从 startPos 开始 searchChar 没有在 str 中出现则返回-1，
    <br/>如果 str 为 null 或 "" ，则也返回-1

20.public static int indexOf(String str, String searchStr)
    <br/>返回字符串 searchStr 在字符串 str 中第一次出现的位置。
    <br/>如果 str 为 null 或 searchStr 为 null 则返回-1，
    <br/>如果 searchStr 为 "" ,且 str 为不为 null ，则返回0，
    <br/>如果 searchStr 不在 str 中，则返回-1

21.public static int ordinalIndexOf(String str, String searchStr, int ordinal)
    <br/>返回字符串 searchStr 在字符串 str 中第 ordinal 次出现的位置。
    <br/>如果 str=null 或 searchStr=null 或 ordinal<=0 则返回-1
    <br/>举例(*代表任意字符串)：
      <br/>StringUtils.ordinalIndexOf(null, *, *) = -1
      <br/>StringUtils.ordinalIndexOf(*, null, *) = -1
      <br/>StringUtils.ordinalIndexOf("", "", *) = 0
      <br/>StringUtils.ordinalIndexOf("aabaabaa", "a", 1) = 0
      <br/>StringUtils.ordinalIndexOf("aabaabaa", "a", 2) = 1
      <br/>StringUtils.ordinalIndexOf("aabaabaa", "b", 1) = 2
      <br/>StringUtils.ordinalIndexOf("aabaabaa", "b", 2) = 5
      <br/>StringUtils.ordinalIndexOf("aabaabaa", "ab", 1) = 1
      <br/>StringUtils.ordinalIndexOf("aabaabaa", "ab", 2) = 4
      <br/>StringUtils.ordinalIndexOf("aabaabaa", "bc", 1) = -1
      <br/>StringUtils.ordinalIndexOf("aabaabaa", "", 1) = 0
      <br/>StringUtils.ordinalIndexOf("aabaabaa", "", 2) = 0

22.public static int indexOf(String str, String searchStr, int startPos)
    返回字符串 searchStr 从 startPos 开始在字符串 str 中第一次出现的位置。
    举例(*代表任意字符串)：
      <br/>StringUtils.indexOf(null, *, *) = -1
      <br/>StringUtils.indexOf(*, null, *) = -1
      <br/>StringUtils.indexOf("", "", 0) = 0
      <br/>StringUtils.indexOf("aabaabaa", "a", 0) = 0
      <br/>StringUtils.indexOf("aabaabaa", "b", 0) = 2
      <br/>StringUtils.indexOf("aabaabaa", "ab", 0) = 1
      <br/>StringUtils.indexOf("aabaabaa", "b", 3) = 5
      <br/>StringUtils.indexOf("aabaabaa", "b", 9) = -1
      <br/>StringUtils.indexOf("aabaabaa", "b", -1) = 2
      <br/>StringUtils.indexOf("aabaabaa", "", 2) = 2
      <br/>StringUtils.indexOf("abc", "", 9) = 3

23.public static int lastIndexOf(String str, char searchChar)
    基本原理同18

24.public static int lastIndexOf(String str, char searchChar, int startPos)
    基本原理同19

25.public static int lastIndexOf(String str, String searchStr)
    基本原理同20

26.public static int lastIndexOf(String str, String searchStr, int startPos)
    基本原理同22

另附：

String 的 split(String regex)   方法的用法
如果我们需要把某个字符串拆分为字符串数组，则通常用 split(String regex) 来实现。

例如：
```java
   String str = "aa,bb,cc,dd";     
   String[] strArray = str.split(",");      
   System.out.println(strArray.length);     
   for (int i = 0; i < strArray.length; i++) {     
        System.out.println(strArray[i]);     
   }  

    String str = "aa,bb,cc,dd";   
    String[] strArray = str.split(",");    
    System.out.println(strArray.length);   
      for (int i = 0; i < strArray.length; i++) {   
           System.out.println(strArray[i]);   
}
```
 

结果为：
4
aa
bb
cc
dd

如果，
String str = "aa.bb.cc.dd";
String[] strArray = str.split(".");

则结果为：0

为什么结果不是我们所想的呢，原因是参数 String regex 是正则表达式 (regular expression) 而不是普通字符串，而 "." 在正则表达式中有特殊含义，表示匹配所有单个字符。如果要那样拆分，我们必须给 "." 进行转义，String[] strArray = str.split(".") 修改为 String[] strArray = str.split("\\.") 即可。
另外有关 StringUtils 的详细 API 请参见官方网站: http://commons.apache.org/lang/api/org/apache/commons/lang/StringUtils.html 