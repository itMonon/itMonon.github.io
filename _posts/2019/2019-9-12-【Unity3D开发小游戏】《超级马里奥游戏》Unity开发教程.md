---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《超级马里奥游戏》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
超级马里奥游戏于1985年为任天堂娱乐系统发布，成为有史以来最受欢迎的电子游戏之一。

当涉及到好的游戏设计时，这个游戏是一个很好的例子。在本教程中，我们将创建游戏的一个小测试场景，并尽可能地关注核心机制。我们将使用Unity3D引擎

和往常一样，每件事都会一步地解释，并且尽可能简单，这样每个人都能理解它。

## 二、版本
==Unity5.0.0f4==

## 三、正文
### 1.主照相机设置
现在我们可以选择主照相机在Hierarchy使用典型的蓝色背景色 (红色=107, 绿色=140, 蓝色=255)，调整大小而位置所以游戏看上去就像：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912151509687.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 2.艺术风格
为了防止版权问题，我们不能在本教程使用原超级马里奥图片。相反，我们将画我们自己的Sprites，使他们看起来像原来的游戏。

在我们开始之前，让我们创建一个新的Sprites文件夹中的项目区:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912151631824.png)
== 这就是我们把Sprite放在这里的地方。这是一个花哨的游戏开发词，用来表示图像。)==

### 3.石头
**石头的图片**
我们首先画一个16x16像素石材绘图工具中的图像，如：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1zdXBlci1tYXJpby1icm9zLWdhbWUvc3RvbmUucG5n?x-oss-process=image/format,png)
==注意：右击图像，选择另存为。并将其保存在项目的Assets/Sprites文件夹。==

现在，我们可以在项目区然后在Inspector修改导入设置:
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091215254274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注：Pixels to Unit值为16意味着16个像素的大小等于游戏世界中一个单元的大小。我们将使用这个值作为我们所有的纹理。因此，如果我们想将两个16x16雪碧放置在一起，我们可以使用以下位置(1, 0), (2, 0)诸若此类。

现在我们可以从项目区进入场景为了把它添加到游戏世界中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912152625134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
让我们在Hierarchy选择这个物体，在Inspector并将其坐标改为(0, 0, 0):
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912152718940.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
==注意：我们想制作一个2D游戏，所以我们并不真正关心Z坐标。它将是0对于我们游戏中的所有对象(除了相机)。在2D游戏中我们只关心X(水平坐标)和Y(垂直坐标)。==

**分选层Sorting Layer**
因为我们是在一个二维游戏世界，会有一些情况下，几个图像是画在对方之上，例如，当马里奥站在灌木丛前。我们总是希望灌木丛在后台，这样马里奥就会被画在前面。(否则我们就看不到他了).

让我们告诉Unity，石头总是应该在幕后。这就是分选层我们可以更改石头的排序层，如果我们查看检查器中的Sprite Renderer组件：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912152749307.png)
让我们选择添加排序层.。从分选层列表，然后添加Background层，并将其移动到第一个位置，如下所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912152805653.png)
==注意：Unity从上到下画层，因此背景中的任何内容都将位于列表的顶部。==

现在我们可以重新选择石头并分配我们的新Background排序层：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912152832516.png)
**石物理**
现在的石头只是一个形象，仅此而已。这不是物理世界的一部分，马里奥将无法在上面行走或任何东西。我们需要添加一个Collider2D让它成为物理世界的一部分，这意味着事物将与石头相撞，而不是跌落或穿过石头。

我们可以添加一个Collider2D通过在Inspector选择添加组件->物理2D->Box Collider 2D:
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091215342937.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在石头是物理世界的一部分，就这么简单。

**添加更多的石头**
现在我们可以添加更多的石头到我们的场景。我们去看看Hierarchy在其中，我们右键单击当前的石头，然后选择Duplicate:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912153602121.png)
我们将把新的石头放置在(x=1，y=0):
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912153753787.png)
让我们重复这个工作流程，直到我们有两排21块石头(y=0处一行，y=-1处一列)..重要的是始终将它们放置在圆角坐标上，如(2, 0)从来没有(2.003, 0.005)..如果我们按下Play:
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091215380931.png)

## 3.灌木丛和云
就像在原来的超级马里奥兄弟游戏，我们也将添加一些灌木丛和云的背景。与往常一样，我们将从绘制图像开始：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1zdXBlci1tYXJpby1icm9zLWdhbWUvY2xvdWQucG5n?x-oss-process=image/format,png)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1zdXBlci1tYXJpby1icm9zLWdhbWUvY2xvdWRzLnBuZw?x-oss-process=image/format,png)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1zdXBlci1tYXJpby1icm9zLWdhbWUvYnVzaC5wbmc?x-oss-process=image/format,png)
==注意：右击每个图像，选择但作为.。把它们全部保存在项目中资产/Sprites文件夹。==

我们将使用以下方法导入设置对于所有图像：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912153943862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在我们可以把Sprites 从项目区拖入到场景中，把他们安置在我们想让他们去的地方：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912154015384.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
因为它们是背景的一部分，所以我们在Hierarchy然后分配Background再次排序层：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912154035759.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注意：如前所述，背景排序层确保这些对象总是绘制在其他所有内容的后面。换句话说，马里奥总是会被吸引到他们面前。

我们的团结超级马里奥兄弟。游戏慢慢开始看起来像一个！

## 5.管道
**图片**
这些绿色管道是超级马里奥兄弟公司的重要组成部分，所以让我们画一个：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1zdXBlci1tYXJpby1icm9zLWdhbWUvcGlwZS5wbmc?x-oss-process=image/format,png)
==注意：右击图像，选择但另存为.。并将其保存在项目的Assets/Sprites文件夹。==

下面是导入设置我们的管道：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912155328231.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注意：即使它是一个32x32像素的Sprites，我们仍然使用相同的像素转到单位时时刻刻都很有价值。

现在我们可以把Sprites从我们的项目区进入场景..我们将在世界的右侧增加两条管道：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912155358785.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**管物理**
管道应该是物理世界的一部分，这意味着马里奥不应该直接穿过它们。相反，他应该与他们碰撞。要使它们成为物理世界的一部分，我们所要做的就是在Hierarchy选择物体然后在Inspector单击添加组件->物理二维->Box Collider 2D..如果我们看看场景然后，我们可以看到绿色对撞机盒周围的每一个管道：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912155515448.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们可以看到，管道的下部比我们的Collider要薄一点：
这没什么大不了的，但让我们慢慢来，用一个巧妙的小技巧来创造一个完美的对Collider。一种选择是只使用多边形Collider，这会带来一些负面影响。

更优雅的解决方案(至少在这种情况下)将另一个Collider添加到我们的管道中，然后对齐第一个Collider，使其适合管道的上部和第二个Collider，使其适合管道的下部：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091216205562.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在管道的物理是完全真实的。像这样的小调整才能让游戏感觉正确晚些时候。

. . .未完待续
