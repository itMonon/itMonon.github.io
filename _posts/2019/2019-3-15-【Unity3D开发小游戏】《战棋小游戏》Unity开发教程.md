---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《战棋小游戏》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
这次想要做的一个小游戏，或者说一个小Demo，其实是一个简单且传统的战棋战斗场景。初步的设计是：在2D世界里创建一张由六边形地块组成的战斗地图，敌我双方依据体力在地图上轮流行动并向对方发动攻击，先消灭掉所有敌人的一方将获得胜利。

这一辑将比上一辑的内容更简单，但完成后会是一个功能较完整且可以玩耍的Demo。

我使用的Unity版本是2018.2.7，但是其实并没有用到2018的任何新功能。


## 二、Github地址
Github：https://github.com/elsong823/HalfSLG
**可以自己下载源代码*
## 三、正文

-------
预计将分为以下几篇：

1、创建战场
根据预定尺寸生成战场地图，并随机一些障碍物。

2、添加对战双方
向战场中添加作战单位，作战单位可被点选，并进行移动。

3、战场逻辑
作战双方按照顺序依次行动，可进行移动、攻击。

4、添加战场UI
添加可以随时显示战况的Hud、为作战单位添加血条等。

5、扩展作战单位
丰富作战单位的类型，添加职业，并加入若干不同类型的技能。

6、扩展战场地图
丰富战场地图，加入地形及道具等元素。

7、规范战斗配置
可以通过规范化的数据结构配置战场、职业、技能、道具等。

-----

## 一、创建战场
 **|  目标**
生成一个规定尺寸的战场，战场上的格子均为六边形，并且暂定两种格子类型：普通格子和障碍格子。

|  在开始之前我做了以下事情
1.创建了新的工程。
2.新建场景并保存在场景文件夹下。
3.删除默认创建的平行光，调整场景光照设置，弃用了天空盒等。
4.将相机调整为正交相机。
因为是从零点五开始系列，认为读者有一定的Unity引擎使用基础，就不对这些操作进行介绍了。

**|  创建战场地图**
我没有采用把数据和显示捆绑在一起的做法，而是将数据和显示分离。
我的思路是用一个战斗创建器来创建战斗，一场完整的战斗信息至少应包含地图及对战双方的信息。而Unity搭建的战斗场景则可以理解为一场战斗的显示器。

**|  数据部分（主要）**
战斗数据：地图尺寸，包含所有格子的二维数组。
格子数据：格子类型，所在行列，所在空间坐标。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRkYzUxNDhjMi5wbmc?x-oss-process=image/format,png)
**|  显示部分（主要）**
战场：一个用于组织所有格子对象的节点对象。
格子对象：一个能显示六边形瓦片的2D渲染器，并根据格子类型改变颜色。

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRkZmI1ZTAyMi5wbmc?x-oss-process=image/format,png)
当需要将战斗表现出来时，需要将以上数据装入一个用于显示的“战场”，这个战场就是在Unity世界里创建的。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRlMDkyN2VkZS5wbmc?x-oss-process=image/format,png)
**|  在Unity中创建战场**
目前战场的结构还非常简单，只需要一个组织格子单位的节点。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRlMjI3NTllZS5wbmc?x-oss-process=image/format,png)
**|  创建格子**
创建一个格子脚本，并挂在一个新创建的空对象上。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRlNmE2NjViZi5wbmc?x-oss-process=image/format,png)
为这个空对象添加一个子对象，称之为瓦片，用来显示一个六边形。

因为这里我只想做一个2D的Demo，因此我们只需要一个SpriteRenderer组件即可。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRlN2FkMjYyMi5wbmc?x-oss-process=image/format,png)
使用预先准备的一张六边形图片作为地块显示，一个格子对象就做好了。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRlOGNiYjFkMi5wbmc?x-oss-process=image/format,png)
为了区分普通和障碍，暂定普通格子为白色，障碍格子为灰色。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRlYTI5YjY3ZS5wbmc?x-oss-process=image/format,png)
**|  部分代码**
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRlYzZlMjJkMi5wbmc?x-oss-process=image/format,png)
铺设普通格子
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRlYjNhYjFkZi5wbmc?x-oss-process=image/format,png)
随机放一些障碍格子

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRlZWEwYzk2OS5wbmc?x-oss-process=image/format,png)
战场根据格子信息，显示格子
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRlZjkxNTg1NS5wbmc?x-oss-process=image/format,png)
格子根据不同类型显示不同颜色
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRmMDgyMzMyOS5naWY)
初始随机多张地图，并切换查看。


## 二、添加战场地图功能
**|  目标**
实现一些后面所需的操作地图的基础功能，如：
1、点击战场后高亮显示被点中的格子；
2、点击两个位置实现从A到B的导航并在地图上显示路径；
3、设定一个移动半径，点中格子后显示出可移动范围。

实现后的效果如下图：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMDQvNWMwNjE0NDViOTJjMC5naWY)
点击格子变红
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMDQvNWMwNjE0NWE1MGQyZi5naWY)
红色向蓝色导航(黄色为路径，青色为探索过但没有采用的格子)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMDQvNWMwNjE0NmQ0NzNmYy5naWY)
半径为2个单位的可移动范围

**|  点击格子变红**
我采用的方法是：
1、获取屏幕点击位置的世界坐标，并将其转换到格子的Root节点下；
2、用这个坐标推断出点击格子所在的行、列范围；
3、遍历这些推测格子的中心，找到距离最近的格子，即为点中的格子。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMDQvNWMwNjE0OTQ0M2I4Ni5naWY)
点击屏幕后推断的九个格子

**|  调整地图瓦片渲染器的位置**
为了方便计算格子的位置，这里调整了瓦片渲染器的位置，保证它在地块对象的中心，这样在设置、获取、计算坐标时，少一步转换的操作，也更容易被理解。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMDQvNWMwNjE0YTc5YmJlMC5wbmc?x-oss-process=image/format,png)
**|  导航**
这里采用的导航是A星算法，网上的介绍很多，就不再赘述了。在这只对六边形地图上两格之间最短移动距离的计算做个简单说明。

六边形格子的路程计算

与四边形地图差别不大，六边形地图也可理解为先做行移动，再做列移动。但它的差异是：在做行移动的同时，其列也可以在一定范围发生变化，因为它可以斜着走。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMDQvNWMwNjE0YmE3MDgxOS5wbmc?x-oss-process=image/format,png)
单元格坐标在仅计算行移动量的同时，列可移动范围是一个三角形。

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMDQvNWMwNjE0YzUxNzgyOC5wbmc?x-oss-process=image/format,png)
起始点所在奇、偶行的差别，对覆盖三角形区域的计算是有影响的。
**注：黑字表示格子的列，红字表示当前格子与出发格子的列差值。**

综上，六边形地图下两格子之间的最近路程可以理解为：从起始位置先纵向移动，如果移动到与目标同行，但是目标格子又不在可到达格子范围内的话，再横向移动若干单位即可，如图：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMDQvNWMwNjE0ZGE3ZjEyMi5wbmc?x-oss-process=image/format,png)
需要注意的是，奇数、偶数行与首行是否“缩进”了半个格子有关。

**|  调整数据结构**
导航功能交给一个新的工具类：地图导航器来完成，但是原来地图中格子信息是直接保存在战斗数据对象中，现在我将地图数据从战斗数据中拆分出来，便于导航器集中处理数据。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODExMjIvNWJmNjRlMDkyN2VkZS5wbmc?x-oss-process=image/format,png)
旧 战斗数据的结构

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMDQvNWMwNjE1M2I5Y2NkMC5wbmc?x-oss-process=image/format,png)
新 战斗数据结构
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMDQvNWMwNjE1NDcyMWY3OS5wbmc?x-oss-process=image/format,png)

**|  根据半径显示可移动范围**
预设半径显示可移动范围，也可转换为在进行行偏移的同时，求列覆盖的最小和最大值。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMDQvNWMwNjE1NTRhY2RlMC5wbmc?x-oss-process=image/format,png)
如在计算移动半径为2的覆盖区域时，假设从中心点开始，推算下方的区域，本质上是：
1、行移动量为0时，覆盖纵坐标偏移[-2~2]的区域。
2、行移动量为1时，覆盖纵坐标偏移[-1,0] + [-1, 1]的区域，其中[-1, 2]为行移动量为1的格子覆盖区间，[-1, 1]为再横向移动一单位时的偏移增量。
3、行移动量为2时，覆盖纵坐标偏移[-1, 1]的区域。

在计算偏移区域时，使用了上述提到的计算两格子之间距离的方法。


## 三、添加对战双方

**|  目标**
向战场中添加战斗单位，完成简单的战斗循环，看起来的样子是：
1、战场中的对战双方轮流行动，可进行移动、攻击；
2、攻击将对敌人造成伤害；
3、没有生命值的战斗单位会被从战场中移除；
4、当一方被全部消灭时，战斗结束。

实现后的效果如下图：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU3YTUzMWQ5Ni5naWY)
**|  准备工作**
在开始之前，我们先做一些准备工作。

显示格子坐标
为格子添加Text Mesh Pro组件以显示格子坐标，方便调试。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU3YzUxZjRjNy5wbmc?x-oss-process=image/format,png)
**增加地图功能：放置出生点**
我不想看到战斗单位在刚进入战场的时候是随机摆放位置的，因此我需要为它们提供一些出生点。这样当战斗单位初入战场时，会向战斗地图请求一个本方可用的出生点，如果请求成功则加入战场，并设定在那个位置上；如果请求失败则不会进入战场，避免出现乱占位置的情况。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU3ZDE0YjdkMS5wbmc?x-oss-process=image/format,png)
想象中出生点的位置，最上、最下排奇数位置放置出生点
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU3ZGQ4ODI4Zi5wbmc?x-oss-process=image/format,png)
实际生成的情况（绿色为出生点）

**增加地图功能：寻找最近可用格子**
指定一个起点和一个终点，返回一个环绕终点的距离起点最近、且可用的格子。这主要是为了战斗单位在确定攻击目标后，需要选择一个它身边的格子作为移动的目标格子（目前假定所有战斗单位的攻击半径都为1）。

这里选择了一种比较偷懒的方法，就是将导航位置直接设定在目标单位的身上，如果导航成功，则将到达终点的前一个格子作为目标格子，这样不仅确定了目标格子，同时还将导航路径一并算出。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU3ZmY3MmU5MC5naWY)
点击起点和终点进行测试(红：起点，蓝：终点，灰：障碍，青色：目标格子)

准备工作到此为止，下面开始加入战斗单位。


**|  添加战斗单位**
因为是六边形瓦片地图组成的战棋游戏，因此我将战斗单位也表示成六边形，目前来看两者的Prefab并没有什么差别，通过设置Order值来确保战斗单位显示在地图格子的上方。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4MTUwMzhmNC5wbmc?x-oss-process=image/format,png)
为了更好的区分战场双方战斗单位的差异，我们给它们设置不同的颜色。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4MjQ4MzQzYS5wbmc?x-oss-process=image/format,png)
虽然从显示方式来看，战斗单位与地图格子并没有什么差别，可是如果从数据角度出发，两者的差别可就大了。为了更好的介绍战斗单位，让我们从上至下来梳理一下整个战场与战斗系统吧。


**|  战场与战斗信息**
一个战场就是一场完整的战斗。每一个战场目前都包含三大部分：战场地图、对战双方以及战斗过程。

战场地图
地图在之前的文章中已经做了说明，这里不再赘述。

对战双方 对战双方的单位是战斗组，这里用战斗组编号区分各组，而不是仅用两个枚举来简单表示，是考虑到有很多组同时存在且同时对战的情况。

真正发生战斗的被称为战斗单位，每个战斗组由若干战斗单位组成。战斗数据、战斗组与战斗单位之间的关系如下图。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4MzRlOTUxNy5wbmc?x-oss-process=image/format,png)
**战斗过程**
战斗打响时，从两个战斗组进入战场，到双方轮流移动、攻击，最终分出胜负，发生的一切事情，都是战斗过程，这个后面会详细说明。

**|  战斗的流程**
战斗流程包含了战斗的核心逻辑，是战斗能正常进行且完成的规则，我们用下图来描述一场战斗的基本流程。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4NGMzNzAwZi5wbmc?x-oss-process=image/format,png)
**|  将数据与显示分离**
这里还是采用了将数据与显示分离的处理方法，先看一张数据处理的流程图吧。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4NWYyNDY1NS5wbmc?x-oss-process=image/format,png)
图中很关键的一个内容是战斗过程数据，上面提及它其实是包含了自战斗单位进入战场，到战斗最终完结之间的所有过程数据。

其实，当开始一场自动战斗时，战斗计算器会瞬时计算完整场战斗的过程及结果，但这些结果只是数据，并没有呈现给玩家。

当我们需要把这场战斗呈现出来时，把这份数据传递给一个对应的显示(播放)器即可。就好像后端和前端的分工一样，一个负责产生数据，一个负责将数据呈现。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4NzNlZjdhMy5wbmc?x-oss-process=image/format,png)
数据与对应的显示器
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4ODExZDAzOS5wbmc?x-oss-process=image/format,png)
战斗数据的显示器
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4OWYzZTQxOC5wbmc?x-oss-process=image/format,png)
地图格子显示器
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4OTIwMTI3Zi5wbmc?x-oss-process=image/format,png)
战斗单位显示器

**|  顺序分步呈现数据**
我这里使用协同函数(Coroutine)的嵌套来分步呈现战斗过程。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4YjY2OGJhOC5wbmc?x-oss-process=image/format,png)
战场显示器开启逐步呈现战斗过程(战斗单位的行动)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4YzBkZGMzZC5wbmc?x-oss-process=image/format,png)
战斗单位显示器根据自己的动作数据呈现具体动作，如：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4Y2RkZDIzZC5naWY)
进入战场
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4ZTkxYzM3Yy5naWY)
选择目标并移动（青色框：发动攻击方，黄色框：攻击方的目标）
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4ZGNiMDQ3NS5naWY)
选择目标并攻击（青色框：发动攻击方，黄色框：被攻击方）

**|  分离的意义**
走吧，走吧，人总要学着自己长大。

人是这样，数据也是。

其实直接使用一个继承与MonoBehaviour的脚本，把各种需要的数据都装在里面，直接挂在Prefab上，然后用一个控制器一边算一边呈现给玩家，实现起来非常容易。

但是，考虑到后台可能有多场战斗同时在进行；且后期可以在短时间内进行多场战斗、收集数据来做战斗数值平衡。将数据分离，让数据可以自行计算，就变得十分重要了。

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxODEyMjEvNWMxYzU4ZmM5ZGQ4MS5naWY)
20x20地图下，10 vs 10的千场战斗结果计算，可以在很短的时间内完成

## 四、加入玩家控制
**|  目标**
分配一个角色给玩家手动操作，每回合玩家有一次行动机会，行动的规则是：可从移动、攻击、待命中选择一次操作，当玩家选择移动后，还可选择一次攻击或待命的操作；玩家也可以不进行移动直接选择攻击或待命，这也会结束当前行动回合。

实现后的效果如下图：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMDEvNWMyYjcwMmQ4NWQ5NC5naWY)
这次要做的非常少，因为之前已经写好了战斗逻辑，只需要把手动操作加入即可。

**|  准备工作**
首先，为战斗单位设置一个是否为手动操作的标记。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMDEvNWMyYjcwM2MzM2I1Zi5wbmc?x-oss-process=image/format,png)

其次，为战斗单位设定一个手动操作状态，标记这个战斗单位是否可以移动、攻击。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMDEvNWMyYjcwNDc5MzdhYS5wbmc?x-oss-process=image/format,png)
最后，新增一种新的战斗单位操作状态和动作，分别是等待玩家操作状态及等待手动操作的动作。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMDEvNWMyYjcwNTYwZWMwNi5wbmc?x-oss-process=image/format,png)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMDEvNWMyYjcwNjE2NmRhMS5wbmc?x-oss-process=image/format,png)
当轮到玩家操作时，战斗的单位会显示绿色边框。

**|  手动操作的逻辑**
大概思路是：战场维护一个战斗单位的行动队列，每当轮到一个单位行动时，如果这个单位是自动操作的，那么它会自己决定目标，移动并攻击，上回我们将数据与渲染进行了分离，因此它只是产生了一次行动数据（Action）。
如果这个单位是手动操作的，那它会产生一个等待手动输入的行动数据，同时返回等待玩家操作状态。

当战场发现自己收到了一个等待玩家操作的状态，就明白其实刚才的家伙并没有生成任何有意义的战斗数据，因此它必须通知自己的渲染器（那个用来显示的模块，上回我们介绍过）：嘿，帮我问问他到底想干嘛？

渲染器通过UI与玩家进行交互，获得玩家要移动、攻击或是待命等“有用”的战斗信息后，再回头通知战场“这个家伙已经行动过了”，战场再让下一个单位行动。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMDEvNWMyYjcwNmY2YmMzOS5wbmc?x-oss-process=image/format,png)
简易的行动流程图

由于下一回我们才会加入战场UI，这里我们先用Unity自带的GUI代替。

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMDEvNWMyYjcwN2E1NDZkYy5wbmc?x-oss-process=image/format,png)
用GUI显示一个列表，接收玩家的操作

## 五、添加常用的界面

**|  目标**
使用Unity自带的UGUI替换之前的GUI来实现一些常用界面：
1、包含开始战斗按钮及战斗结束时提示文字的主界面。
2、玩家手动操作时，辅助选择移动、攻击及待命的弹出面板。
3、点选地图、战斗单位时，弹出的详情展示面板。

实现后的效果如下图：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY4OGI4NTRkNi5wbmc?x-oss-process=image/format,png)
战斗结束时显示的友情提醒
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY4OTZjMmJiMi5wbmc?x-oss-process=image/format,png)
行动选择菜单
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY4YTU3YjAzOS5wbmc?x-oss-process=image/format,png)
战场单位信息面板

不难看出，当UGUI碰撞上专业的素材后，一个个绝美的界面瞬间跃然屏上。这也给了那些常说“程序员不懂美”的家伙们一记响亮的耳光。

**|  关于如何制作界面**
这些界面都是用UGUI制作的，并没有什么难度，相信上手过UGUI或NGUI的同学，只要碰到精美的纹理贴图，都能轻松完成。

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY4YjA2NGU2Ni5wbmc?x-oss-process=image/format,png)
使用的是一套高雅灰主题的专业UI纹理素材
本文不会对UGUI的使用做详细介绍，我们将重点聊聊对界面的管理。
因为只要找到了方法，做出漂亮的界面就只是时间问题罢了。
而至于证明“程序员也能凭自己的能力作出专业界面”这件事，相信上面已经做到了。


受项目大小及时间所限，这里会使用一套轻量级的界面管理方式。而在此之前，让我们先做一些前期准备工作。

**|  分配相机**
为界面的绘制单独分配一个相机(界面相机)，并调整界面相机和战场相机上的Clear Flags、CullingMask和Depth设置。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY4YmUzMTdkNS5wbmc?x-oss-process=image/format,png)
<center>界面相机与战场相机的设置</center>


整个界面系统的根画布(ScreenCanvas)，是一个类型为ScreenSpace-Camera的Canvas，它的父节点ScreenUIRoot上有专门渲染界面的界面相机。

**|  设置层级**
ScreenCanvas下设5个节点，分别对应5个层级：背景层、基础层、弹出层、顶层和Debug层.
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY4Y2VhZDcxYy5wbmc?x-oss-process=image/format,png)
<center>界面的层级结构</center>

各层的功能为：
**背景层**：装饰性的、非功能性的界面。
**基础层**：常驻的界面（主界面、角色头像、快捷操作栏等）。
**弹出层**：点击后弹出的界面（各功能界面）。
**顶层**：强制显示在最上层的界面（Tips界面或走马灯等）。
**Debug层**：开发时辅助调试用。

这些层级从下到上放置，遮挡关系是上层遮挡下层。当然，其顺序、层数和名称可根据实际需求进行调整。

需要注意的是，这5个节点(Transform)并非是必须的，它们只是为了Debug时能更直观的查看层级间的关系，真正用于用于区分层级的是Canvas上的**SortingLayer**和**OrderInLayer**属性。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY4ZGNjZWNmOS5wbmc?x-oss-process=image/format,png)
SortingLayers设置中的层级与对应的节点
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY4ZTg2N2U2Ni5wbmc?x-oss-process=image/format,png)
不同界面上Canvas所设置的Layer和Order值

**|  界面管理流程**

比起代码，我觉得还是看图来的更直观些。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY4ZjkzNDg5OS5qcGc?x-oss-process=image/format,png)
<center><font color=gray size=2>界面管理流程


整个界面管理可以简单拆解为四个部分：打开界面、关闭界面、层级刷新及界面刷新，下面我们依次介绍。

**|  打开界面**
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY5MDgyMWEwMS5wbmc?x-oss-process=image/format,png)
打开界面的逻辑很简单，但需要注意的是，为了更好的使用内存，界面管理器维护了两个界面缓存区：**常驻缓存**和**临时缓存**。打开界面时如果需要加载新的界面，先去这两个缓存区中查看一下是否有缓存过的界面，从缓存区加载比重新读取新的界面效率要高。

**|  关闭界面**
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY5MTQzYTIxNy5wbmc?x-oss-process=image/format,png)
当我们要关闭界面时，界面管理器会根据它的“**存储策略**”决定其被关闭后的去留，有些会被放入到常驻缓存区，有些会被放到临时缓存区，而有些则会被直接移除。

>这就好比两性交往中女生犯错通常当时就会被原谅；男生犯错通常需要好好表现一段时间才有可能被原谅；而单身狗连犯错的机会都没有。

**|  层级刷新**
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY5MWZlOWVkYi5wbmc?x-oss-process=image/format,png)
上面介绍了打开和关闭界面。其实它们都可以被看成是在“**显示界面**”。因为关闭界面也可以理解为“被这个界面遮挡覆盖的界面，可能需要显示了”。

因此，当有界面被打开或关闭后，界面管理器会**从上到下**的让各层刷新自己的**显示状态**及对**屏幕的遮挡状态**，并将这个遮挡状态向下传递，用作后面层级的显示判断。

**|  界面刷新**
我们知道界面的刷新和显示是有代价的，因为它们会对CPU及GPU的性能造成开销。因此我为每个界面设置了**是否遮挡了屏幕**、不可见时是否仍然刷新及**Dirty属性**。

如果一个界面遮挡了屏幕，那么它下面的界面首先应该被“隐藏”以减小渲染的压力；其次如果不可见的界面没有被设置为“**不可见时仍然刷新**”，则当需要**刷新**它时（界面数据发生了变化），也只是被打上Dirty标记，并在下次需要**显示**的时候再刷**新**。

**|  界面的生命周期函数**
上面流程图中的蓝色部分，是界面的生命周期函数，它们会在适当的时间被界面管理器、层级或界面自身调用。
Init：初始化，界面首次被加载后调用。
**OnPush**：界面被显示前，加入层级时被调用。
**OnShow**：界面被显示时调用。
**UpdateView**：界面需要被刷新时调用。
**OnHide**：界面被隐藏时调用。
**OnPopup**：界面被关闭，从层级中移除时调用。
**OnExit**：界面不需要被缓存，被Destroy前调用。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY5NDViYjIzMy5wbmc?x-oss-process=image/format,png)
<font color=gray size=2>一次完整的界面打开、刷新、关闭过程</font>



**|  界面的存储策略**
界面被关闭后会根据预设的存储策略决定去留，这里定义的存储策略有以下三种。

**自动移除**：很少用到的界面，每次关闭时会被直接**Destroy**掉。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY5NTgzZjllYS5naWY)
<center><font color=gray size=2>自动移除策略</font></center>

**临时缓存区**：较为常用的界面，在关闭时我们把它放入一个有深度设置的缓存区，这个缓存区当收入一个新的界面时，会判断缓存量是否已超过预设深度；如果超过了**预设深度，**会将**最早**缓存的界面弹出并**Destroy**掉。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY5NjQ5MzFjMS5naWY)
<center><font color=gray size=2>临时缓存区策略（深度为2）</font></center>

**常驻缓存区**：需要频繁开关的界面，在关闭时我们会把它们放入一个没有深度设置（缓存个数限制）的缓存区。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY5NmViM2Q2MC5naWY)
<center><font color=gray size=2>常驻缓存区策略（仅唯一存在界面可被设置为常驻）

**|  界面配置**
我使用**ScriptableObject**对象作为界面配置的载体。每次创建新的界面时，需要同样创建一个**配置对象**，并将两者进行关联。界面自身及使用者可以通过读取这个对象获取界面的**配置信息**。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY5N2IwODkzOC5wbmc?x-oss-process=image/format,png)
<center><font color=gray size=2>界面配置文件的属性构成

当然我们可以稍微修改Editor，添加一些辅助工具帮助我们快速生成界面配置文件。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMTUvNWMzZDY5ODVlZjdiMC5wbmc?x-oss-process=image/format,png)
<font color=gray size=2>一个简单的配置文件刷新器

## 六、为战斗单位添加血条，加入伤害文字特效
**|  目标**
为战斗单位添加血条，加入伤害文字特效。

实现后的效果如下图：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMzAvNWM1MTE3OGQ0Y2Y0OS5wbmc?x-oss-process=image/format,png)
*极其精致的血条*
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMzAvNWM1MTE3OWVjZTk3Mi5naWY)
*与3A游戏同款的伤害文字特效*
**|  添加血条**
添加一个简单的血条，主要由三部分构成：红色的底，绿色的条和生命值。

偷个懒，直接使用SpriteRenderer + TextMesh Pro来完成它。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMzAvNWM1MTE3YjI0MGIxNy5wbmc?x-oss-process=image/format,png)
*一个简单的血条*

每次生命值变化时需要更新两个东西：绿条的长度和生命值。

更新文字很容易，直接设置即可；更新绿条的长度呢？只要调整它在x方向上的缩放比例就行了。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMzAvNWM1MTE3Y2Y2ODE4NS5naWY)
*通过Scale X来控制绿条的长度*

但是需要注意将Sprite的锚点设置为Left，这样仅调整x方向的缩放比例就能达到目的；如果锚点为Center的话，还需要同时调整Position X。

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMzAvNWM1MTE3ZGFkZjM4OC5wbmc?x-oss-process=image/format,png)
*血条所用图片的导入设置*


**|  添加伤害文字特效**
好吧，再偷个懒，直接用一个TextMesh Pro配合一个Animator即可...
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMzAvNWM1MTE3ZmQ3NWQ3Yi5wbmc?x-oss-process=image/format,png)
*一个简单的伤害文字特效*

做法很简单，将文字的动画直接做在一个Animation Clip中，然后用Animator Controller控制播放它们就行了。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMzAvNWM1MTE3ZTk0Mjk1ZC5wbmc?x-oss-process=image/format,png)
*录制动画*

在这里我做了两个动画：一个普通伤害的动画，和一个暴击伤害的动画（后面会用到）。当然，它们之间的显示效果差别非常大。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMzAvNWM1MTE4MjU5N2I1My5naWY)
*普通伤害文字*
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMzAvNWM1MTE4MDk1YTVlMy5naWY)
*暴击伤害文字*

后面就非常简单了，在Animator面板中设置动画播放规则，比如通过不同的Trigger来播放普通伤害特效或暴击伤害特效，然后再在代码中根据伤害类型设置对应的Trigger即可。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMzAvNWM1MTE4M2M5YmQzMS5wbmc?x-oss-process=image/format,png)
*用Trigger来区分这个Animator应该播放哪个动画*
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAxMzAvNWM1MTE4NDk5YWY5MS5wbmc?x-oss-process=image/format,png)
*播放动画*

## 七、扩展作战单位
丰富战斗元素，加入并实现手动释放不同类型的技能。
 **|  目标** 
加入一些常见、简单的技能类型，如：

1、单体远程
对远程一个敌方单位进行攻击。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzMDU5NjViZS5naWY)
*远程攻击单体目标*

2、单体远程带范围效果
对远程一个敌方单位进行攻击，同时对其周围一定距离内所有敌方单位造成相同伤害。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EyY2U4MjYzNi5naWY)
*单体远程带范围效果*

3、以自身为中心的范围技能
以自身为中心，对周围一定距离内所有敌方单位造成伤害。

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzMWMzMTAwMS5naWY)
*以自身为中心的范围技能*

4、远程指定范围的技能
远程指定攻击一定范围内的所有敌方单位。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzM2M5ZjE5YS5naWY)
*远程指定范围的技能*

5、单体恢复
恢复自己或一个友方单位的HP值。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzNDllNjVhYy5naWY)
*单体恢复*

需要提前声明的是，本文主要记录的是在手动释放技能时，操作展示上的一些关键事项；技能计算的逻辑请见代码；AI释放不同类型技能也将放在下回。


**|  增加效果显示**
我为地块、战斗单位设置了不同的显示状态以便更直观的获取操作反馈。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzNTgwZjlhNy5wbmc?x-oss-process=image/format,png)
*战斗单位、地块的显示状态*

无论是地块还是战斗单位，都是通过简单的状态机来实现不同显示效果的切换。

**|  地块的效果显示**
由于地块会包含多种状态共存的情况，比如上面的远程范围攻击：某些地块会被同时设置为技能释放范围和技能效果覆盖范围。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzNjY4NmU2Yi5wbmc?x-oss-process=image/format,png)
*部分地块既在技能释放范围内，又在技能效果范围内*

为了解决这种情况，地块的显示状态判断使用了位运算。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzNzY1NjRjMy5wbmc?x-oss-process=image/format,png)
*使用位运算来控制显示状态*

**|  战斗单位的效果显示**
战斗单位不涉及多种状态同时存在的情况，处理起来就简单多了。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzODU3YmEzYy5wbmc?x-oss-process=image/format,png)
*战斗单位的显示状态*

**|  技能信息**
与之前的界面配置一样，我仍然使用ScriptableObject作为技能信息的载体，因为这样实现起来最快捷。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzOWJhMTdmNS5wbmc?x-oss-process=image/format,png)
*记录技能信息的ScriptableObject*

由于目前包含的技能类型较少，数值计算也十分简单，因此只需要少量的属性就足够了，这里就不再赘述了。

**|  手选技能的操作规则**
其实，这些技能的计算逻辑并不复杂，麻烦一些的是不同类型技能在手动操作时的规则及显示逻辑。
我在这里制定了简单的操作规则：

1、对于单体近战、单体远程、单体恢复技能：
选择技能后显示技能释放范围，标出范围内外的单位；点击可选单位则释放技能。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzYWU0ZjM2My5wbmc?x-oss-process=image/format,png)
*对单体目标技能的操作*

2、对于单体远程带范围效果的技能：
选择技能后显示技能释放范围，标出范围内外的单位；点击可选单位后展示技能效果范围，标出范围内的单位；再次点击该单位后释放技能。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzY2Q1ZDNiZC5wbmc?x-oss-process=image/format,png)
*对单体远程带范围效果技能的操作*

3、对于以自身为中心的范围技能：
选择技能后显示技能效果范围，标出范围内外的单位；点击任意单位后释放技能。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzZDk4MDdkMC5wbmc?x-oss-process=image/format,png)
*对以自身为中心的范围技能的操作*

4、对于远程指定范围的技能：
选择技能后显示技能释放范围；点击范围内任意地块，显示技能效果范围，标出范围内外的单位；再次点击相同地块释放技能。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzYmU0N2FiYi5wbmc?x-oss-process=image/format,png)

*对远程指定范围技能的操作*

5、操作任意类型技能时，点击鼠标右键为取消。

**| 简单的技能分析**
为了实现上述操作逻辑，我在点击使用技能后加入了一个简单的技能分析步骤，它会遍历场上所有战斗单位，并根据释放者及技能类型将他们划分为可被选中的、队伍不符的、距离不符的和状态异常的四类，这样后面的操作逻辑实现起来就简单多了。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzZWI4NDk5MC5wbmc?x-oss-process=image/format,png)
*点击使用技能后的流程*
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2EzZjk1NGFkMC5wbmc?x-oss-process=image/format,png)
*技能分析结果*

**|  增加技能操作界面**
我微调了战斗单位的操作面板，为攻击按钮增加了一个选择技能的二级面板。添加了一个一级面板透明处理的小设置来区分层级；以及在不同屏幕位置点击时的面板弹出位置的优化，以防止面板弹出到屏幕以外无法操作。由于这不是本次介绍的重点，就不在这赘述了。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAyMjAvNWM2Y2E0MGIyYmM5OC5naWY)
*技能选择面板*

## 八、加入AI系统（上）
建立超级简单的AI系统。
 **|  目标** 
加入一个超级简单的AI系统，会自动释放不同类型的伤害技能。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAzMTEvNWM4NWMzODM1NjE1MS5naWY)

*自动释放技能的AI*

需要提前说明的是，建立简单的AI系统预计将拆分为三篇更新。

第一篇(本篇)通过加入一些简单的AI逻辑，保证战斗单位可以自动选择(伤害)技能、自动作战，进而顺利的完成一场战斗。

第二篇会进一步丰富AI的决策系统，让它的表现更具期待性，使战斗变得更加有趣。

此外，我邀请了我的好友Aillieo，拜托他按照自己的方式也设计一个AI系统。

因此，我会在第三篇介绍他所设计的AI系统，并对这三篇做一个整体的总结。


**|  非常简单的AI系统**
个人以为，有意思的AI系统可以简单的定义为：

> 让人觉得符合逻辑，却又在一定程度上超出了预期。

如何实现一个非常简单的AI系统呢？为了让问题变得再简单些，我将AI的行为拆解成固定的三个步骤：
1、确定攻击目标；
2、向攻击目标移动；
3、使用技能。

**|  确定攻击目标**
将”合理“的目标设定为攻击目标，是件并不太容易的事情。

这里我且不谈那些优秀的游戏是怎么做的，因为我也不知道。只说说我目前所使用的方法：仇恨系统。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAzMTEvNWM4NWMzOTg4NDYwNi5wbmc?x-oss-process=image/format,png)

*AI使用仇恨列表确定攻击目标*

每当一个战斗单位在战场中被敌人攻击时，他就会偷偷的在自己的小本本里记下攻击者的名字，以及他们的罪行。

当轮到他行动时，他就会掏出自己攥了很久的小本本，按照之前它们揍自己的程度进行降序排列，然后按照这个名单，判断自己反击的可能性。

这里，没有反击的可能性，指的是：如果目标已经被人包围，自己却又是一个近战角色无法靠近，那他就会嘟囔着“哼饶你一条狗命”，然后继续看下一个人。

直到确定这个家伙可以被自己攻击到，他就会合上小本本，把他的名字刻上自己的心头，然后准备开始下一个步骤：向他移动。

**|  向目标单位移动**
向目标移动就很简单了，通过A-Star算法找到移动路径后，行动即可。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAzMTEvNWM4NWMzYjI2OGVmOC5wbmc?x-oss-process=image/format,png)
*确定目标后沿路径移动*

但是这里有一个小问题：应该选择哪个格子作为移动的终点呢？

特别是当攻击者是某些远程攻击单位，比如游戏中常见的魔法师或者弓箭手，每次都走到目标旁边去攻击，感觉上就有点像“送外卖”。

其实解决方法也很简单，在导航时仍然选择目标所在位置做为导航终点，但在距离终点一定距离时，停止导航并返回导航路径即可。这个停止距离，就是远程攻击单位的射程，或者手动设定的某个值。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAzMTEvNWM4NWMzYzFjMTA4Yi5wbmc?x-oss-process=image/format,png)

*射程为2的小红，导航停止在距离小蓝两个单位的格子上*

这与“真正的爱情，能跨越一切障碍”是一个道理。

当然，如果这个人儿并不在天边，而在触手可及的地方，那他根本就不用移动，直接进入下面的环节吧。

*|  对目标使用技能*
光说，不练，假把式。

好容易走到了他(她)的身边，总得有所表示吧？

试想一个场景：你很喜欢一个女孩儿，在表白的关键时刻，你有一百种表达方法，但你却只能选择一种，究竟哪种才是最有效的呢？

如果是真实的生活，答案很简单：看运气。

但是游戏则不同，你可以用S/L大法(存、读档大法)来不断重试，直到找出效果最好的那一种！

决策将要使用的技能也可以是一样的。

这里我且不谈那些优秀的游戏是怎么做的，因为我也不知道。只说说我目前所使用的方法：简单的计算所有可用技能的释放回报。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAzMTEvNWM4NWMzZDQxMDgwOC5wbmc?x-oss-process=image/format,png)
*计算技能得分并确定所使用的技能*

计算技能释放得分的公式异常复杂，由于这并不是一篇学术性论文，因此这里不做详细的解释和说明，只把公式列出即可：

技能释放得分 = 技能造成的总伤害 ÷ 技能消耗的能量值

也就众所周知的：
![从零点五开始用Unity做半个2D战棋小游戏（八）](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAzMTEvNWM4NWMzZWUyMmFkNC5wbmc?x-oss-process=image/format,png)


天啊，好麻烦。

但是，在得到了按照释放得分降序排列的可用技能列表后，带着何种的心情、用着怎样的姿势、使用哪个技能的问题，就变得十分容易了。

可能我们只需要注意下远程范围技能的释放点选择问题即可。
![从零点五开始用Unity做半个2D战棋小游戏（八）](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAzMTEvNWM4NWM0MjUyYTQzOS5wbmc?x-oss-process=image/format,png)
*释放影响半径为2的远程范围技能*

对于远程范围技能，我们当然可以使用一些方法，找到覆盖最多目标的释放点。

但为了省事儿，我这里是这么处理的：当目标超过技能释放距离时，尝试找到释放技能时，可以覆盖到目标单位的点，然后从这里随便选一个即可。当然，如果目标本身就在技能释放半径内，就选它为释放中心了。

![从零点五开始用Unity做半个2D战棋小游戏（八）](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAzMTEvNWM4NWM0MDI3ZDc4OC5wbmc?x-oss-process=image/format,png)
*红色区域为覆盖半径为2的技能在释放时，可以伤害到蓝色格子的释放点*

**|  能量值**
当然，为了帮助AI计算出哪个技能的释放得分更高，我为每个战斗单位都增加了一个能量值的属性（你也可以认为它是魔法值）；为每个技能增加了释放的能量消耗；同时还为游戏增加了每次行动时恢复10个单位能量的设定。但是由于这些逻辑都很简单，这里就不赘述了。

最后，我们来回顾下整个行动流程吧：
1、打谁；
2、去哪打；
3、怎么打。

![从零点五开始用Unity做半个2D战棋小游戏（八）](https://imgconvert.csdnimg.cn/aHR0cDovL2dhZGltZy0xMDA0NTEzNy5pbWFnZS5teXFjbG91ZC5jb20vMjAxOTAzMTEvNWM4NWM0MzZkYzM0ZC5qcGc?x-oss-process=image/format,png)
*完整的AI行动流程*


**|  写在最后**
至此，建立超级简单的AI系统篇就介绍到这了。如你所见，这里只是实现了非常简单的AI行动逻辑，并没有体现出各种类型AI的不同，我们下期将尝试着解决这个问题。
