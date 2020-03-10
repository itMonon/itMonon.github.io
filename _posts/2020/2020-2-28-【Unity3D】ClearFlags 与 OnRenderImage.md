---
layout: post
category: Unity3D-Daily
title: 【Unity3D】ClearFlags 与 OnRenderImage
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
无论多基础、简单的知识，只要不会，就是难。。
这次的总结主要与相机上的Clear Flags及OnImageRender函数有关

**Clear Flags**

对于这个选项，我是这么理解的：每一个相机在开始绘制时，都需要对当前RenderBuffer中的颜色缓冲区(ColorBuffer)和深度缓冲区(Z-Buffer)进行是否清除的操作，这个选项控制了清除及清除后的内容。

下面将展示一下不同Clear Flags设置下的区别：
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617b2c79fdd.png)
(相机绘制一个蓝色的立方体)

**Sky box**：清除颜色缓冲区和深度缓冲区，并将颜色缓冲区设置为天空盒的颜色。
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617b72d511d.png)

（立方体以外部分的"ColorBuffer"都被天空盒填满）

**Solid Color**：清除颜色缓冲区和深度缓冲区，并将颜色缓冲区设置为一个固定的颜色。
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617b8958154.png)
（立方体以外部分的"ColorBuffer"都被选定的颜色填满）

上述两种选项是最容易理解的，只是直接清除了缓冲区内的颜色和深度数据，因此如果场景中存在多个相机，且最后一个相机被设置为上述两种ClearFlags，那么，呵呵，之前的相机绘制结果将无法直接显示(因为都被清掉了)。

**Depth Only**：保留颜色缓冲区，仅清除深度缓冲区。
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617ba3c9d07.png)
（两部相机先后绘制）
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617b94b6f81.png)
（仅清除了深度缓冲区的绘制）

很容易发现，由于拍蓝色立方体的蓝色相机后绘制，且不清除颜色缓冲区，因此蓝色相机在绘制之前，颜色缓冲区已经被红色相机所拍的结果：SolidColor + 红色立方体所填充，因此蓝色立方体只是在这基础之上进行的绘制。
并且，由于清除了深度缓冲区，蓝色立方体无论空间是否被红色立法体遮挡，总会任性的全部绘制，因此产生了错误的遮挡效果。

**Don't Clear**：毛都不清除。
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617bb4728f1.png)
（毛都不清除的绘制-保留了深度缓冲区）

同上，颜色缓冲区在保留的同时，也保留了深度缓冲区，这样蓝色相机在绘制时，就会被红色立方体的深度值所影响，因此获得了正确的遮挡效果。

对了，如果只有一个相机，并且这个相机还设置了“DepthOnly”或者“Don'tClear“的选项，也就是不清除颜色缓冲区，那么当你拖动一个物体时，就会产生这种撸多了的效果：
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617bbb3bc0a.gif)
（无影脚应该就是这么拍的吧）


OnRenderImage
那么OnRenderImage跟这个相机的ClearFlags有什么关系呢？
其实没有什么太大关系。

这个函数通常是用来做什么的？
重写这个函数是为了达到屏幕后处理特效的目的，比如全屏虚化等。

这个函数如何使用及调用时机？
首先，包含这个函数的脚本必须附着在一个相机上；
其次，一旦重写这个函数，这个函数所发生的时机，就是在这个相机完成与自己有关的全部渲染后，即将把这次渲染结果更新给当前RenderBuffer时的那一夜（刻）。

大概的触发时机如下图：
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617bd9c6d9b.png)
注意，如果你重写了这个函数，必须有一个将缓冲区传递给dest的操作，即调用Graphics.Blit(rt,  dest)。如果没有这个操作，可以想象，经过这个相机后，RenderBuffer中将会是一片漆黑。

但是：一片漆黑仅仅是因为没有把正确的颜色缓冲区设置给当前的RenderBuffer中的ColorBuffer部分，深度缓冲区仍然不受影响。

以下将讨论几种情况，希望能把问题说清楚：
测试的环境
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617bef480f1.png)
1、两个立方体的空间位置关系，红色遮挡了蓝色。
2、蓝色相机只拍蓝色立方体，红色相机只拍红色立方体。
3、相机渲染的顺序：红色 → 蓝色。
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617be79bfdc.png)
4、蓝色相机的渲染结果同时填充给屏幕左下角的Image，红色相机的渲染结果同时填充给屏幕左下二号Image。

**第一种情况**
蓝色相机Depth Only， 红色相机Solid Color = 天蓝色。
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617bfb8757d.png)
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617c04dccc1.png)
**第二种情况**
蓝色相机Solid Color = 壮汉粉， 红色相机Solid Color = 天空蓝
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617c147b612.png)
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617c1d28d82.png)
**第三种情况**
蓝色相机Solid Color = 壮汉粉，红色相机Depth Only
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617c285025b.png)
红色相机第一帧
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617c2ce840a.png)
第一帧后
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617c387dc55.png)
**第四种情况**
蓝色相机Don't Clear，红色相机Solid Color + 空OnImageRender函数
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617c4ccf49a.png)
红色相机没有做向dst传递颜色缓冲区的动作
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617c554c50d.png)
红色相机没有生成任何东西
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617c5d8982c.png)
蓝色受到了红色立方体深度值的影响

分析
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20180119/5a617c63cdf31.png)
总结：
只要心心念着颜色缓冲区 和 深度缓冲区，掐指一算就差不多了。
