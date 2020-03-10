---
layout: post
category: Unity3D-Opti
title: 【Unity3D性能优化】关于Draw Call
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 目标
- 学习怎么减少Draw Call，让你的游戏运行更加流畅
- 尽管我的图形界面如此简单，为什么我的游戏还是那么延迟呢？
- 为什么我的游戏加载那么长时间？
- 为什么界面间的切换如此的慢？
- 为什么我的游戏的FPS如此的低?
- 我已经把所有的（Texture）纹理和（Sprite）精灵都压缩了！为什么还是那么延迟？
- 为什么我的游戏仍然崩溃？
- 为什么在玩我的游戏时，电池消耗如此的快？
- 为什么在玩我的游戏时，手机那么烫？

让我们一起面对它，在游戏发开中我们都遇到过这些问题。我们将试着分析新的图形,图像压缩,新代码，这有用吗？这反而会浪费我们大量时间和成本。最终，我们尝试用一些奇葩的解决方案或者直接放弃。

在这里，我们将尽可能的减少我们的过失，我宁可说这是一个失误，也不愿意说这个一个错误，然而主要元凶就是**Draw Call**。

在我们解决Draw Call过多的问题之前，先来理解一下什么是Draw Call。

**那么，Draw Call是什么呢？**
Draw Call是一条命令，由CPU向GPU发送的一条命令，去渲染一个网格（Mesh）。这条命令只指定一个网格（Mesh）是否被渲染/绘不绘制任何材质（Material）信息（伙计，再忍受我一段时间，往下阅读将会变得更简单，我保证）。

在获得命令之后，GUP获得了渲染状态的值（材质（Material）、纹理（Texture）、着色器（Shader）等等），且在你的屏幕中所有的顶点数据通过一些代码逻辑转换成漂亮的像素（当然我希望它是漂亮的）。

 

渲染命令基本上做一些数量众多的小任务，例如在屏幕上计算成千上万的顶点和绘制成千上万的像素。

 

**Note**
每一个网格（Mesh）使用一个不同的材质（Material）将需要一个单独的Draw Call。

 

**Draw Call是如何影响我们游戏的？**
让我们来看一个例子来理解它。我打算使用一个简单的UI面板（Panel）去帮助你更容易的理解这个概念。

  

**步骤一：根据你的想法来创建UI**

  我是这样创建的，如下图所示：
![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48ERZygBp9rk100CqInkK92ZfEgsRxe8VrPec10aJDqpzQ4zjfmWgRaia7qqknqVzY3ic8Rc5zrLxHFg/0)
 
 

如上所示，这是非常基本只使用了少数圆形和矩形。

 

精灵（Sprite）,我用如下所示:

 
![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48ERZygBp9rk100CqInkK92ZfdMsoOIxpIZpshwomKVcGNiccF6Hia4Yp9OlSmhKjpAcgJzLoPSKFeBA/0)
 

步骤二：查看Draw Call

  按下Play键开始游戏，并且点击“State”按钮，在游戏视图的右上角，如下图所示：
![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48ERZygBp9rk100CqInkK92ZrHHvCVlpw6zCkpdicDnYUXdqxh0HLLwjyNdhGEL1NTcpdNRYK9sec6w/0)

你将会弹出一些游戏运行时关于图形渲染的重要数据，如下图所示：
![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48ERZygBp9rk100CqInkK92ZBenrEKIeQetnWf2ic3qQu0MJwMVjtGLib1icgWDzX8gqRavXmWnwBYDUA/0)
 

对于我们来说，最主要的数据是“**Batches**”。降低“**Batches**”的数值就等同于降低了Draw Call的次数

 

想要了解更多关于“Stats”窗口信息的伙计，可以进入下面链接进行深入学习：

http://docs.unity3d.com/Manual/RenderingStatistics.html

 

**步骤三：打开Frame Debugger窗口**

NoteUnity5.0以上的版本才有该功能
![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48ERZygBp9rk100CqInkK92Zibh7yOyYG92jA3CWsdOEBztGUBMSshp9N22s0XFTqIYKpPtPTh7C0Uw/0)

 

**Unity官方文档：**

  使用Frame Debugger时，游戏会暂停，然后Unity会将当前正在执行的一帧的内容缓存下来，其中所有Draw Call你都可以进行前进与后退操作，从而能够从Draw Call级别分析开销。

 

想更加深入的理解Frame Debugger的伙计，可以进入下面链接：

 

http://docs.unity3d.com/Manual/FrameDebugger.html

 

那么让我们点击“Enable”对Draw Call进行分析吧
![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48ERZygBp9rk100CqInkK92ZOKKhBqbc0pIL8oye1Al2ZBJMtJicvbsey493NhUVFeEwnQBSDanjt5Q/0)

 

 点击“Enable”之后，程序将会暂停，并且显示一些绘制屏幕所需要的“Batches”的数量，对于我的所创建的UI来说，在Batches上显示的数字为10。你的也许和我的不一样，这取决于你的屏幕（在和我的UI一样的情况下）。你可以滚动每一个Draw Call去查看每一次调用所产生的信息。

 

 

**OK，我可以看到一些Draw Calls，我为什么要在乎它呢？**
就像我之前说的，Draw Calls取决于你的计算机硬件，虽然这只是个简单的UI，却花费了大约10个Draw Calls去完成屏幕的绘制。

现在想象一下，在实际游戏开发中需要花费多少次Draw Calls？

想象一下在每一帧都有成千上万的网格那得需要多少个Draw Calls啊！！因此，减少Draw Call的次数将可以减少GPU的开销。

“一个现代的桌面游戏在每一帧中可以处理大约500-5000个Draw Calls。而手机设备只能处理大约40-60次Draw Calls，最新的手机最多也只能处理120-160个Draw Calls”

因此，Draw Calls也许是一个很大的麻烦！！记住，帧速率是老大！！

 

如果你的帧速率还不错，那就不需要担心Draw Calls了。

Draw Calls的数量严重影响性能，在很大程度上依赖于使用者的硬件设备。

**噢...我现在知道了，原来Draw Calls是罪魁祸首！但是有什么好的解决方案吗？**
幸运的是，在Unity中有一个名为“Sprite Packer”的内置工具解决了我们的烦恼。

**Unity官方文档：**

“为了获得最佳的性能，最好的方式就是把一个个Sprite打包成图集，Unity提供一个Sprite Packer的功能去自动生成图集”

现在我们简单的把工程中的几个Sprite打包成图集。

让我们看看到底能帮助我们减少多少Draw Call。

**步骤一：选择你想要打包的Sprite**

事实上，你应该把在同一个屏幕上所有Sprite都打成一个包。

**步骤二：给这个包命名一个标签（对于每一个想要打包在一起的Sprite都做如下设置），如下图所示：**

![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48ERZygBp9rk100CqInkK92Zy8tGWYAcmd68qjWa5eQ6S1iaIKJ1h8uWzknMuJeB1dA2c4KMP98MlQA/0)

 

在这里，我的标签是“MainScreen”，你可以按照你想要的命名来。

**步骤三：打开Sprite Paker窗口，并且完成打包**



Sprite Paker，如下图所示：
![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48ERZygBp9rk100CqInkK92Zf0nqW3BLU1zN27rcCAGPVkjLA64RsMPiaCJxrn57bCaIJyTqC9Up44w/0)

点击**Pack**按钮

这便把所有Packing Tag设置为MainScreen的Sprite都打包成了一个图集。

如上图所示，可以通过下拉菜单去找到相应的图集。

如果一些Sprite没有打包到图集里，则会进一步打包到子图集中。

你也可以选择一些打包算法。

你可以参考下面的链接进一步了解：

   http://docs.unity3d.com/Manual/SpritePacker.html

**步骤四：运行游戏！！**



你有看到什么改变吗？



在Stats弹出的窗口中查看“**Batches**”数据
![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48ERZygBp9rk100CqInkK92ZKXUkXibA94aAQibOl465ecmTEx9s7xgL6Q8dBklwKMz0icHBT0eic7sZSg/0)
我的“Batches”居然由10变为了3！！Draw Calls减少了7个！！这便优化了2倍的性能！！也减少了GPU的负担！！

想象一下现实世界中，由500降到200（下降两倍的意思）这将会是一个很大的提升。特别是对于手机来说。

这便给予渲染有了巨大的提升。

**额外福利**
这不是真的把？

只需要花费5分钟去正确设置即可，因此你值得拥有。

通常，渲染是一个繁琐的任务，因此减少Draw Call将会减少渲染的负担。有一些如**Texture Packer**的第三方工具使用了先进的打包算法，跟Unity自带的Sprite Pack比有一定的优势。

**你可以通过以下链接去深入了解:**

    

https://simonschreibt.de/gat/renderhell/

https://www.codeandweb.com/texturepacker

http://docs.unity3d.com/Manual/OptimizingGraphicsPerformance.html
