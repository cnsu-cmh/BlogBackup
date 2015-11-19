---
title: java file文件类操作使用方法大全
date: 2016-11-16 00:28:24
tags: [java]
---
[文章来源:java file文件类操作使用方法大全](http://blog.csdn.net//u011229848//article/details/53180741)


# java file文件类操作使用方法大全

**构造函数**
```java
package com.zuidaima.file;
public class FileDemo {
     public static void main(String[] args){
         //构造函数File(String pathname)
         File f1 =new File("c:\\zuidaima\\1.txt");
         //File(String parent,String child)
         File f2 =new File("c:\\zuidaima","2.txt");
         //File(File parent,String child)
         File f3 =new File("c:"+File.separator+"abc");//separator 跨平台分隔符
         File f4 =new File(f3,"3.txt");
         System.out.println(f1);//c:\zuidaima\1.txt
     }
 }
```
**创建方法**

1.boolean createNewFile() 不存在返回true 存在返回false

2.boolean mkdir() 创建目录

3.boolean mkdirs() 创建多级目录
**删除方法**

1.boolean delete()

2.boolean deleteOnExit() 文件使用完成后删除

```java
package com.zuidaima.file;
import java.io.File;
import java.io.IOException;

public class FileDemo2 {
    public static void main(String[] args){
        File f =new File("d:\\zuidaima\\1.txt");
        try {
            System.out.println(f.createNewFile());//当文件存在时返回false
            System.out.println(f.delete());//当文件不存在时返回false
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
}
```
**判断方法**

<br/>1.boolean canExecute()判断文件是否可执行
<br/>2.boolean canRead()判断文件是否可读
<br/>3.boolean canWrite() 判断文件是否可写
<br/>4.boolean exists() 判断文件是否存在
<br/>5.boolean isDirectory()
<br/>6.boolean isFile()
<br/>7.boolean isHidden()
<br/>8.boolean isAbsolute()判断是否是绝对路径 文件不存在也能判断
<br/>**获取方法**
<br/>
<br/>1.String getName()
<br/>2.String getPath()
<br/>3.String getAbsolutePath()
<br/>4.String getParent()//如果没有父目录返回null
<br/>5.long lastModified()//获取最后一次修改的时间
<br/>6.long length()
<br/>7.boolean renameTo(File f)
<br/>8.File[] liseRoots()//获取机器盘符
<br/>9.String[] list()
<br/>10.String[] list(FilenameFilter filter)
<br/>**列出磁盘下的文件和文件夹**
```java
package com.zuidaima.file;
public class FileDemo3 {
     public static void main(String[] args){
         File[] files =File.listRoots();
         for(File file:files){
             System.out.println(file);
             if(file.length()>0){
                 String[] filenames =file.list();
                 for(String filename:filenames){
                     System.out.println(filename);
                 }
             }
         }
     }
 }
```
**文件过滤**

```java
package com.zuidaima.file;

import java.io.File;
import java.io.FilenameFilter;
 public class FileDemo4 {
     public static void main(String[] args){
         File[] files =File.listRoots();
         for(File file:files){
             System.out.println(file);
             if(file.length()>0){
                 String[] filenames =file.list(new FilenameFilter(){
                     //file 过滤目录 name 文件名
                     public boolean accept(File file,String filename){
                         return filename.endsWith(".mp3");
                     }
                 });
                 for(String filename:filenames){
                     System.out.println(filename);
                 }
             }
         }
     }
 }
```
**File[] listFiles()**

File[] listFiles(FilenameFilter filter)

**利用递归列出全部文件**
```java
package com.zuidaima.file;
public class FileDemo5 {
    public static void main(String[] args){
        File f =new File("e:\\zuidaima");
        showDir(f);
    }
    public static void showDir(File dir){
        System.out.println(dir);
        File[] files =dir.listFiles();
        for(File file:files){
            if(file.isDirectory())
                showDir(file);
            else 
                System.out.println(file);
        }
    }
}
```
**移动文件**

找出d盘下所有的 .java 文件，拷贝至 c:\jad 目录下，并将所有文件的类型由.java 修改为.jad 。
```java
package com.zuidaima.file;

public class Test5 {
    public static void main(String[] args){
        File f1 = new File("d:\\zuidaima\\");
        moveFile(f1);
    }

public static void moveFile(File dir){
    File[] files=dir.listFiles();
    for(File file:files){
        if(file.isDirectory())
            moveFile(file);
        else{
            if(file.getName().endsWith(".java"))
                file.renameTo(new File("c:\\jad\\"+
            file.getName().substring(0,file.getName().lastIndexOf('.'))+".jad"));
            }
        }
    }
}
```
之前自己写过一个fileUtils工具类,今晚看到老牛分享的这个博问就直接借用了，以后在这个基础上添加吧