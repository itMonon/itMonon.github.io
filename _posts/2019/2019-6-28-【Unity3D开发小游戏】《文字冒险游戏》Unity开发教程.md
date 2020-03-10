---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《文字冒险游戏》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、基本程序设计（故事卡）
游戏会为玩家呈现一个“故事卡”。故事卡上包含一些文字，其中一部分是用于描述玩家当前的状态，另外一部分是在当前情况下玩家可以做出的一系列选择。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628142702319.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
根据玩家的不同选择，剧情也会按照不同的分支向前发展，并持续出现新的卡片与选择，直到最终的卡片不再有新的选择，则游戏结束。
 
制作一张“故事卡”很简单。根据上诉需求，我们新建StoryCard脚本，脚本代码如下：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628142929365.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
 StoryCard在检视面板中显示如下：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628142937751.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
 
您需要在StoryCard脚本的Description字段中输入卡片描述（玩家可以在屏幕上看到的文本）、玩家在选项按钮上看到的文字（目前该程序最多支持4个按钮）、以及按下对应按钮后跳转的故事卡（即为分支，稍后会介绍）。
 
StoryCard脚本还包含两个故事状态属性“States to Set True”以及“States to Set False”，这两个属性分别有何作用呢？下面就来看看。
## 二、故事状态与分支


其实整个系统可以完全使用“故事卡”来制作，但仅使用“故事卡”的话，游戏流程就变得很枯燥无味。假设有某个选项，玩家点击了该按钮，但该选项所导致的后果并不会立即在剧情中呈现，而是在随后的剧情中缓慢展开。假使仅使用“故事卡”，就需要立即从该选项开始出现分支，并且直至该选项导致的最终影响出现之前，都要在所有剧情分支上重复同样的步骤。
 
因此，我们需要一个更好的解决办法。
 
前面提出了“故事状态”的概念用于存储状态值，这其实只是一个布尔值容器。卡片在被激活时可以根据需要对特定的状态进行赋值。新建StoryState脚本，代码如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628143027659.png)
StoryState脚本在检视面板中显示如下图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628143039293.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

该方法解决了记住状态值的问题，下面通过“剧情分支（Story Branch）”来应用这些状态值。一个分支会引用一个状态以及剧情发展的两个不同方向（可以是“故事卡”或者剧情分支），被引用的状态值用于决定剧情走向何种结果。
## 三、游戏管理器
这个管理器用于承载整个剧情的发展，其作用是将目前的“故事卡”状态更新到UI上，在不同按钮按下时做出对应的动作，并引导剧情前进。这个管理器也需要一个“故事卡”来作为故事的开端。以下是GameManager脚本的内容：


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628143059991.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628143111344.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

其中SetCurrentStoryItem函数用于设置当前显示的剧情节点。SetCardDetails函数用于设置“故事卡”的细节，例如剧情描述，选项按钮及描述等。UpdateButtons函数用于更新所有选项按钮及其响应事件。
 
GameManager脚本在检视面板中显示如下图：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628143123199.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
 
