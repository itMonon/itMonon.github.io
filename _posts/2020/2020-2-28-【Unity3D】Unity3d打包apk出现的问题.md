---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity3d打包apk出现的问题
tagline: by 恬静的小魔龙
tag: Unity3D
---

**Android Studio无法找到tool.jar解决方法！**
**Failed to build apk.See the Console for details.**
**CommandInvokationFailure:Failed to build apk.**


这里写图片描述
![这里写图片描述](http://img.blog.csdn.net/20170928173336199?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcTc2NDQyNDU2Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



![这里写图片描述](http://img.blog.csdn.net/20170928173354970?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcTc2NDQyNDU2Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



![这里写图片描述](http://img.blog.csdn.net/20170928173408267?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcTc2NDQyNDU2Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)




困扰两天的问题终于解决了，以为是SDK的问题，换了无数个SDK，结果都不行。
然后以为是SDK tool的问题，又换了好多tool都不行
最后就认真看错误，一行行分析，才发现原来是sdktool.jar这个东西出现错误了。
然后就在网上找sdktool.jar这个文件
最后就发现了这么一篇帖子

**Android Studio无法找到tool.jar解决方法！**
http://blog.csdn.net/sunylat/article/details/48263897


发现我的情况也类似，也是装完Android Studio之后出现问题，然后就去修改安装路径。
把环境变量里面的JAVA_HOME的变量值最后加上\
Path的变量值改为路径\bin

然后就完美打包了！！！


记录下来这个错误吧。。以后不要再犯了，也让跟我出现一样错误的小伙伴有个参考解决方案吧。
