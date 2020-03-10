---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《炸弹人游戏》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言

我们的炸弹人游戏灵感源于1983年的原始炸弹人，听起来像一个很古老的游戏，但是确实是一个很有趣的游戏。

我们将使用几个有趣的Unity功能，如刚体，Colliders 和Sprite Editor。我们还将学习如何正确地使用Unity的强大的Mecanim动画系统。

## 二、版本
Unity 5.0.1f1

## 三、让我们开始吧
### 背景色
我们在Hierarchy面板中选择主照相机，在Inspector面板 ，改变一下Background颜色。最初的炸弹人游戏使用绿色背景颜色。#397 c00 (r=57，G=124，B=0)..我们可以用那种颜色，或者任何我们喜欢的颜色。我们还将调整大小属性，以便以后游戏看起来足够大：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911173242230.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 创建级别
**不可摧毁的块**
在我们的游戏中，我们将有几种类型的积木。首先，我们需要一个无法摧毁的街区。我们将使用如下工具创建32 x 32 px纹理：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1ib21iZXJtYW4tZ2FtZS9ibG9ja191bmRlc3Ryb3lhYmxlLnBuZw?x-oss-process=image/format,png)
好的，让我们在我们的项目区:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911173320768.png)
我们可以修改块的导入设置:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911173335559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注：Pixels Per Unit单位像素价值32这意味着32 x 32像素将适合在游戏世界的一个单位。我们将使用这个值的所有纹理，因为轰炸机将是32 x 32 Px稍后，我们希望他是一个准确的单位在游戏世界。

之后，我们把Block图片从Project拖入到Hierarchy面板中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911173404956.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911173449645.png)
**块物理**
现在，块只是游戏世界中的一个图像，一个纯粹的视觉效果，仅此而已。我们希望它成为物理世界的一部分，这样轰炸机就不能穿过它。我们可以通过选择Box Collider 2D：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911173512230.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在，街区是物理世界的一部分，就像一堵墙。

这里还有一件事要做。在我们的游戏中，我们将有两种类型的积木：可摧毁的和不可摧毁的积木。我们需要一种方法来找出某个块是否可以被摧毁。有很多方法可以这样做，比如编写脚本或使用标记。我们将保持简单，并使用Static属性用于不可销毁的事物：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911173528520.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注意：我们通常使用Static属性来告诉Unity，某些游戏对象永远不会更改。通过这种方式，Unity可以对它们进行优化。而且，由于我们的不可摧毁的块永远不会改变，使其静态才对。
**可摧毁块**
好的，让我们在游戏中再加一个街区。和以前一样，我们将从画一个开始：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1ib21iZXJtYW4tZ2FtZS9ibG9ja19kZXN0cm95YWJsZS5wbmc?x-oss-process=image/format,png)
我们将使用以下方法导入设置为此：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911173653212.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
然后我们会把它再一次拖到Hierarchy，然后选择添加组件->Physics 2D->Box Collider 2D：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911173733472.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**使用块创建级别**
现在我们可以使用我们的两个块来创建一个简单的级别。我们可以在Hierarchy然后选择Duplicate:

我们可以复制块的频率随我们的意愿，并将他们的位置，以创建一个简单的水平。我们只需记住将每个街区放置在圆角坐标处，就像(1, 2)而不是像(1.002, 1.9998).

以下是一个完整级别的样子：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911173814663.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注意：我们也将所有的区块分组成一个空的。砌块GameObject，我们可以在上面的菜单中创建GameObject游戏对象->创建空..但这是可选的，因为它只是为了保持我们的层次结构更干净一点。

### 炸弹人

好吧，让我们来创造我们的炸弹人角色。我们的角色将能够无所事事，向左，向右，上下移动。我们需要5个动画来使这成为可能。听起来会有很多工作要做，但像往常一样，团结会为我们处理所有复杂的事情。

. . .未完待续
