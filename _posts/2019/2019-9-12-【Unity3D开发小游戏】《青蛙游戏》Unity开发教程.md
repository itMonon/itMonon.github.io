---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《青蛙游戏》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
我们的教程将解释如何在Unity中制作2D青蛙游戏。弗罗格早在1981年就发行了，但由于它的街机性质，它在今天仍然很有趣，并且很容易在Unity中开发。

这将是如此容易，我们将几个像素艺术纹理，只有几种颜色。

像往常一样，一切都会尽可能简单地解释，这样每个人都能理解它。

下面是最后游戏的一个小预览：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1mcm9nZ2VyLWdhbWUvdW5pdHlfZnJvZ2dlci5naWY)
## 二、版本
==Unity5.0.0f4==

## 三、正文
### 1.摄像机设置
如果我们选择主照相机在层次性然后我们可以设置背景色黑色并调整大小而位置如下图所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912165832223.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 2.背景设置
我们将选择一个简单的和未来派的艺术风格，我们的游戏，只有两种颜色。背景将由几个方块和线组成，它们都是1像素宽。我们将使用以下背景图像，但可以随意使用以下工具绘制自己的Paint.net:
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1mcm9nZ2VyLWdhbWUvYmFja2dyb3VuZC5wbmc?x-oss-process=image/format,png)
==注意：右击图像，选择另存为。，导航到项目的资产文件夹并将其保存到新的Sprites文件夹。==

让我们在项目区:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912165929371.png)
然后看一看检验员在那里我们可以更改导入设置若要调整大小和压缩，请执行以下操作：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091216594456.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

注：Pixels Per Unit设为16这意味着16x16像素将适合在游戏世界的一个单位。我们将使用这个值的所有纹理，因为青蛙将是16x16像素后(这应该是一个单位在游戏世界)。

好了，现在我们可以从项目区进入层次性为了让它成为游戏世界的一部分：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912170303813.png)
.如果我们按下Play那么我们已经可以看到它的名字了：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912170318850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注：底部的橙色方块是车辆行驶的街道。中间的绿地就像草，青蛙可以站在那里不用担心。上面的黑色区域将是水，这是青蛙必须跳到平台上才能通过它的地方。顶部的绿地将是必须达到的目标。

我们正在制作一个2D游戏，背景应该总是画在青蛙和汽车后面。为了实现这一点，我们可以用分类层 (用于更复杂的游戏)或者层序财产。我们来看看检验员改变背景层序财产-1:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912170330451.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
==注意：Unity将游戏对象从最低值绘制到最高值，因此前景中的任何内容都应该具有比其他对象更高的值。==

### 4.创造青蛙
**青蛙形象**
没有青蛙就没有青蛙。我们将制作一个大的图像与所有的动画切片在其中。我们需要下列动画：
- 走上去
- 走下
- 向右走
- 向左走
==注意：每个动画将由两个切片组成。如果我们在一个循环中播放这两个切片，那么我们将有一个动画。==

随意画任何你喜欢的青蛙。下面是包含所有动画切片的青蛙图像：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1mcm9nZ2VyLWdhbWUvZnJvZy5wbmc?x-oss-process=image/format,png)
注意：右击图像，选择另存为。并将其保存在项目的资产/Sprites文件夹。

我们将使用以下方法导入设置为此：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912170426399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**青蛙图像切片**
我们的青蛙图像实际上是由几个小片组成的动画。我们可以通过Sprite Mode选择  Multiple。之后，我们可以单击Sprite Editor按钮：.
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912170651332.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
然后我们会选择切片并将类型设置为格网使用以下设置，然后按下切片按钮：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912170726209.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
之后，我们可以关闭切片编辑器并按下应用在未导入设置警告。

我们刚刚将青蛙图像分割成几个16×16像素的图像，我们现在可以在项目区 (他们是青蛙形象的孩子):
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912170736626.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
...未完待续
