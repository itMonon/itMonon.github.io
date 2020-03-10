---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity3d打包APK出现的问题—无法找到tool.jar
tagline: by 恬静的小魔龙
tag: Unity3D
---

1.Unable to merge android manifests.See the console for more details. See the console for details

![这里写图片描述](http://img.blog.csdn.net/20171008185750949?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcTc2NDQyNDU2Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)




![这里写图片描述](http://img.blog.csdn.net/20171008185802795?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcTc2NDQyNDU2Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


解决方案一：http://blog.csdn.net/sunylat/article/details/48263897
启动时候发现，总是提示无法找到tool.jar
这可能是环境变量的问题，将环境变量设置成下面的样子就可以了
JAVA_HOME = C:\j2sdk\
CLASSPATH = .;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar
Path = C:\j2sdk\bin;

在这个变量后面加入“\”，可以正常工作的值为"C:\j2sdk\”

解决方案二：可能是JDK的版本太高导致无法打包成功
查看一下自己的JKD版本是不是9.0的。如果是的话，从网上下载jdk1.7.0版本的jkd
安装成功之后，将环境变量设置成这个1.7.0版本的路径，然后再打包 就成功了。。。
