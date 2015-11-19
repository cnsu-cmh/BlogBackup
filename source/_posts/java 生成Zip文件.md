---
title: java 生成Zip文件
date: 2016-06-15 13:29:29
tags: [java]
---
[文章来源:java 生成Zip文件](http://blog.csdn.net//u011229848//article/details/51680615)

```java
//读取需要压缩文件  也可以修改成f:/sql.txt

File souceFile = new File("D:\\sql.txt") ;

//生成写入压缩文件
ZipOutputStream out = new ZipOutputStream(new FileOutputStream("D:\\outfile.zip"));
out.putNextEntry(new ZipEntry(souceFile.getName()));

FileInputStream in = new FileInputStream(souceFile);
int b;
byte[] by = new byte[1024];
while ((b = in.read(by)) != -1)
{//将需要压缩文件数据写到压缩文件中
out.write(by, 0, b);
}

in.close();
out.close() 
```