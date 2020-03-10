---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity的纹理
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
Unity有一个很好的纹理系统。它是直接使用，但有一些重要的事情要知道，以创造高质量的游戏。

## 二、测试场景
为了测试一些东西，我们将创建一个新项目，创建一个名为Tex.cs的新脚本，并将其附加到主摄像机上。脚本如下所示：

```csharp
using UnityEngine;
using System.Collections;

public class Tex : MonoBehaviour {

    public Texture2D t = null;

    void OnGUI() {
        GUI.DrawTexture(new Rect(0, 0, t.width, t.height), t);
    }
}
```
我们还需要一个测试纹理，所以让我们使用一个“make a game”的标志：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS90ZXh0dXJlcy9sb2dvLnBuZw)
在将脚本附加到摄像机并将纹理添加到项目之后，我们所要做的就是将纹理拖到Tex脚本的“t”变量中，如下所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190703165534176.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
如果按下Play，我们会看到以下内容：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190703165545706.png)
看起来很奇怪。图片有点不清晰，纵横比也不太合适。它看起来很拉长，但为什么呢？

## 三、纹理尺寸
我们的标志有300x100px大小，这是我们的第一个问题的原因。Unity总是以两个相同尺寸的方式加载纹理(例如：2x2、4x4、8x8、16x32、128x256、512x1024、2048x2048等等)。如果我们的纹理没有两种相同的尺寸的话，那么Unity就会缩放它，所以它有两种相同的尺寸，这就是它看起来如此拉伸的原因。

问题是，为什么Unity要这么做？如此复杂的游戏引擎，但我们不能使用所有大小。原因是旧的图形卡只支持两种大小的纹理。由于Unity游戏应该在尽可能多的电脑上运行，它们只需要两种相同的尺寸。

让我们创建一个大小为512x128px的新徽标(这是两种相同尺寸的比例放大512/4=128)，看看在我们按下Play之后它是如何处理的：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190703170347767.png)
什么都没有伸展，看上去很好。

## 四、纹理压缩
通常，人们一旦理解了两种相同尺寸的概念，就会停止对纹理的思考。但还有更多要考虑的东西。

Unity有一个质量设置功能，就像我们从其他游戏中知道的一样。它可用于将图形设置为最快, 快地, 简约, 好的, 漂亮的，美丽的或异想天开..现在关于质量系统的问题是，在某些情况下，Unity会自动调整质量，即使我们没有告诉它调整质量(质量系统可以用脚本来使用)。最好的例子是当我们为网络或手机制作游戏时。那些平台处理不了异想天开图形，这就是Unity将它们设置为最快(或类似的)代替。

下面是我们的512x128纹理在手机或网络浏览器上的外观。最快设置)：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190703170335112.png)
啊！

首先：这很好。如果纹理会附加到球员的帽子或诸如此类的东西，我们不会真的看到质量的差别，因为这只是一个三维世界的一些细节。我们看到的唯一不同是fps的增加，这是很好的。质量体系做得很好，问题是我们没有正确配置我们的纹理。

让我们在ProjectArea中选择我们的纹理并查看inspector：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190703170414762.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
重要的是纹理类型..我们需要的是：
- 纹理：用于我们的3D模型上的材料(不一定是最高质量的)
- GUI：对于像我们的徽标这样的GUI纹理(我们希望看到的是最高质量的)
如果我们选择GUI适用于纹理类型和印刷机应用，这就是我们在手机和网络浏览器上看到的：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190703170802763.png)
干净又锋利！

这并不意味着我们应该使用GUI每样东西都要打字。如果我们这样做，我们最终将面临严重的性能问题。只管用GUI当纹理看起来非常必要时，无论选择哪个平台或质量设置，都是质量最高的。

## 五、渲染纹理

这是关于Unity中的纹理的所有知识。不过，还有一件事：我们可能会碰上渲染纹理每隔一段时间。这些基本上是占位符。在我们的场景中，它们与额外的摄像机结合使用。我们仍然通过我们的主相机看到世界，但额外的相机将世界从它看到的地方呈现到渲染纹理中。

最好的使用例子是汽车游戏中的后视镜。在这种情况下，额外的相机将在车后，后视镜将是渲染纹理。

注意：渲染纹理可以非常快地降低游戏的性能。

