---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《VVVVVV游戏》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
本教程将展示如何在特里·卡瓦纳赫的启发下，制作一款简单而令人上瘾的平台游戏。VVVVV游戏。这个概念很简单，用户必须通过水平行走或倒转重力来控制一个小个子通过某种奇怪的迷宫。

我们的游戏很容易做。它只需要一个水平图像，一个尖峰图像，一个检查点图像和一个播放器图像。

像往常一样，一切都会尽可能简单地解释，这样每个人都能理解它。

以下是最后一场比赛的预览：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC12dnZ2dnYtc3R5bGUtcGxhdGZvcm1lci1nYW1lL3VuaXR5X3Z2dnZ2dl9zdHlsZV9wbGF0Zm9ybWVyLmdpZg)
## 二、版本
==Unity5.0.0f4==

## 三、正文
### 1.摄像机设置
如果我们选择主照相机在层次性然后我们可以设置背景色黑色并调整大小如下图所示：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912163508831.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 2.平面
**绘制平面**
让我们抓住我们的绘图工具的选择，并创造一个平面。我们将保持它的简单，并创建一个大的图像，添加一些背景模式，然后剪出一些区域供玩家走来走去：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC12dnZ2dnYtc3R5bGUtcGxhdGZvcm1lci1nYW1lL2xldmVsX2JsdWUucG5n?x-oss-process=image/format,png)
注意：右击图像，选择另存为.。，导航到项目的资产文件夹并将其保存到新的Sprites文件夹。

**导入平面**
现在我们可以选择项目区:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912163726528.png)
然后在Inspector修改导入设置:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912163800884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注：Pixels Per Unit设成16这意味着16x16像素将适合在游戏世界的一个单位。我们将使用这个值作为我们所有的纹理，因为V尖峰将是16x16像素，这应该是一个单位的游戏世界。

之后，我们可以从项目区进入在Hierarchy为了把它添加到游戏世界中：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912163901907.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**平面物理**
目前，我们的形象实际上只是游戏世界中的一个形象。我们可以看到它，但它什么也做不了，球员将无法在上面行走。我们将添加一个对撞机为了让它成为物理世界的一部分，这样玩家就可以在上面行走并与之碰撞。

我们可以通过选择添加组件->物理二维->多边形对撞机2D在Inspector:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912163921700.png)
如果我们看看场景然后我们就能看到结果了。团结只是试图将多边形围绕在我们复杂的水平形状上：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912163941761.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
问题是，多边形只是一个估计，它还不是我们想要的。现在，只有我们用红色突出显示的区域实际上是可步行的：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912164007709.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
Unity允许我们通过单击以下按钮修改多边形碰撞器：
![在这里插入图片描述](https://img-blog.csdnimg.cn/201909121641019.png)
之后，我们可以修改场景就像这样：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC12dnZ2dnYtc3R5bGUtcGxhdGZvcm1lci1nYW1lL2xldmVsX2JsdWVfZWRpdGNvbGxpZGVyX2luc2NlbmUuZ2lm)
让我们修改整个多边形，直到它完全符合我们的水平：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912164612662.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在水平物理是非常好的。

### 3.V钉
好吧，让我们增加我们的难度，增加红色V尖峰。我们将首先绘制一幅16x16px图像：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC12dnZ2dnYtc3R5bGUtcGxhdGZvcm1lci1nYW1lL1YucG5n?x-oss-process=image/format,png)
注意：右击图像，选择另存为。并将其保存在项目的资产/Sprites文件夹。

我们将使用以下方法导入设置对于图像：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912164734602.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
让我们从项目区进入场景把它放在(-2, -12, 0):.
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912164751181.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
==注意：如果我们把所有的V型尖峰定位在圆角位置上，看起来真的很好，比如(-2, -12, 0)而不是像(-2.03 -11.99, 0).==

让我们再一次加入对撞机，使V物体 Spike 成为物理世界的一部分。这一次我们不需要多边形对撞机，因为它的形状非常简单。相反，我们会选择添加组件->物理二维->盒对撞机2D在检验员然后使用以下属性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912165028221.png)
现在我们可以右击V尖峰Hierarchy并选择Duplicate:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912165047169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
...未完待续
