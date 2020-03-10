---
layout: post
category: Unity3D-Daily
title: 【Unity3D】手机连接Profiler教程篇
tagline: by 恬静的小魔龙
tag: Unity3D
---

我的办法比较简单粗暴蛤，具体步骤如下：

   1.首先你得有安卓SDK
    

   2.然后进入安卓SDK路径下的platform文件夹里，如我的事这样的路径：D:\Andriod\AndroidSDK1\platform-tools

   3.这时候找到adb.exe文件，点击右键发送到桌面快捷方式。

   4.回到桌面，找到adb快捷方式点击右键-属性，且在目标栏后面添加 logcat -s Unity（注意logcat的前面要有空格），如下图所示：
    

![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48HwSd8AtmDmfrBhLQ9YCJAZH3KyLTcwZ5rBmtTeRdtWQ9X2e9VhWjgkxkPD9qePTiaUs1FFXyBUWXg/0?wx_fmt=png)
    5.回到Unity中，点击File - Build Setting 在发布到安卓平台上把Development Build 和 Autoconnect Profiler勾选上，且主要当前发布平台为Android，如下图所示：
    ![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48HwSd8AtmDmfrBhLQ9YCJAZl69HicIblUPjXuqoVPoBLnt9GS6853NkYySZaDIqlEDMlglicmbDKYtw/0?wx_fmt=png)
     6.在Unity中打开Window - Profiler，在 Active Profiler中选择如下图所示：

![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48HwSd8AtmDmfrBhLQ9YCJAZmsD6Mr3ssUWBh8wHegPAdWWw0xNWcia3XZeUibibh2pNZjdtChBUdexHg/0?wx_fmt=png)


   7.好了，这时候按下Ctrl + B 或者 File - Build & Run 即可。
