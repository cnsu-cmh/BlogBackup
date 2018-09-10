---
title: finally里误用return
date: 2017-10-16 17:31:12
tags: [java]
---
[文章来源:finally里误用return](http://blog.csdn.net/u011229848/article/details/78252080)


版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/u011229848/article/details/78252080

今天看到一句话，finally 语句块在 try 语句块中的 return 语句之前执行，过了两年突然对这句话感到有些困惑，以为是执行完try之后有finally就会最终再执行finally，没成想“finally 语句块在 try 语句块中的 return 语句之前执行”才是真理。
<!--more-->
```java
package test;

public class TestFinallyReturn {
	public static void main(String[] args) {
		System.out.println(testFinally());
		
	}
	
	@SuppressWarnings("finally")
	private static int testFinally(){
		try {
			System.out.println("try");
			return 1;
		} catch (Exception e) {
			return -1;
		} finally{
			System.out.println("finally");
			return 0;
		}
		
	}

}

```
执行结果如下：

try
finally
0

由上可知，finally 语句块在 try 语句块中的 return 语句之前执行，一定要注意：

只有资源需要释放的时候，才去使用finally，不要一股脑的扔到finally里面去处理，其他就扔给Java的垃圾回收机制处理吧，否则一不小心返回一个错误的结果就麻烦了。