---
title: 解决MyEclipse不能编译成Class文件
date: 2016-11-08 14:50:28
tags: [java,eclipse]
---
[文章来源:解决MyEclipse不能编译成Class文件](http://blog.csdn.net/u011229848/article/details/53082164)


[解决MyEclipse不能编译成Class文件](http://www.blogjava.net/feng0801/archive/2014/04/24/412889.html)

在开放过程中有时候工作环境不编译文件，解决方案如下：1、确保 project->build automatically 已经被选上。

2、如果选上了也不好使， 使用这一招： project->clean..->选第2个clean select project,，勾上start build immediatelly。

3、删除现在的项目,提前设置好编译文件输出路径，重新导入源文件，设置MyEclipse为保存时编译，然后在保存的时候就可以自动编译了。

4、如果项目里引了某个不用的jar包，而那个包又被你删了，就会出现不报错但怎么也编译不出来class文件的情 况，可以把所有包都删除,然后一个一个的再引入(需要的),不要一下子把所有包都引入来,没用的可能会引起不良后果。

5、想删掉某个class文件重新生成，删除class文件后，但classes目录下的文件夹被其它程序打开，比如Total Commander。此时编译也不会通过，在problems下可能会提示“con't delete classes ……”，关掉其它程序重新编译即可。

6、还有种情况是remove掉 JRE System Library，重新导入即可编译。但是什么原因导致的还不清楚。

7、把build path中所有包都remove掉，然后又add jars,add libraries把需要的加进去，居然又开始编译了。

8、project->properties->java build path->source->.../WEB-INF/src的output folder不要默认，编辑让它指向../WEB-INF/classes然后重新点击build工程即可自动编译。我的问题出在这里，我把这个编译目录给误删了。

9、再就是最重要的要看工程下面是否缺少了work目录，由于CVS控制时不把work加如版本，所以 checkout后没有这个目录,要手工加上有的工程就能自动编译了最开始的时候，我只找到了前面7个方法,但是他们都没有解决我的问题，无意中我打开了"Problems"标签,发现里面说缺少work目录，手工 加上,然后刷新项目就可以了，最后两个是我在写这个总结的时候发现的，特别是第九条对使用CVS进行版本控制的项目比较有用.classpath这个xml文件要仔细看。