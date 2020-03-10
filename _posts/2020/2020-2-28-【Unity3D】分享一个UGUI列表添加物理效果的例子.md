---
layout: post
category: Unity3D-Daily
title: 【Unity3D】分享一个UGUI列表添加物理效果的例子
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
最近要做一个滑动列表界面，美术的效果图为用绳子连接的短板，上面附带信息，看图的感觉似乎添加点物理效果(让绳子不规则的带动短板晃动)会显得更加真实，于是为这个界面加了些物理效果，感觉还不错，特此记录下。


## 二、原文
作者：/zt枸杞忧天
来源：腾讯游戏学院
原地址：http://gad.qq.com/article/detail/288913

## 三、正文
目标：
为UGUI滑动列表中的Element添加物理效果，模拟出绳子微微晃动的感觉。

[^_^]:
    commentted-out contents
    should be shift to right by four spaces (`>>`).
[comment]: <> (This is a comment, it will not be included)
[comment]: <> (in  the output file unless you use it in)
[comment]: <> (a reference style link.)
[//]: <> (This is also a comment.)
[//]: <iframe class="video videoframe" data-src="http://file.gad.qq.com/attachment/download?file_id=3698&video_id=1004_42e08652820a4b4fb0971c9955168a99"></iframe>


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190315104502394.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

关键Component:
Scroll View，Rigidbody2D和Distance Joint2D。

**基本思路：**
1、按照传统方式为Scroll View添加Element
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b07ea7085a.png)
2、在添加Element时，用脚本动态为最上方(第一个)的Element添加一个Rigidbody2D组件。
3、在添加Element时，用脚本动态为第二个及以后每一个Element添加一个Rigidbody2D和DistanceJoint2D组件，并设置与前一个Element的关联。
4、在需要晃动的时候，给第二个开始的每一个Element所附带的Rigidbody2D一个力即可。

**注意事项：**
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b07f741659.png)
1、不要直接为Content下的Element添加Rigidbody2D和DistanceJoint2D组件，因为Content上的Layout组件会更新他们的位置，导致晃动失败。应该把物理组件添加到Element的子节点上。
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b08180fa70.png)
2、为第一个Element添加的Rigidbody2D设置为Static，后面的Element的Rigidbody2D设置为Dynamic。
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b081f23b9c.png)
3、第二个及以后的Element添加Rigidbody2D时，应该调整gravityScale为0，避免其受到重力影响在刚添加的时候就直接掉下去了，同时添加的DistanceJoint应该打开autoConfigureDistance开关，让它能自动获取到两个Element之间的距离。
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b100c0b7c9.png)
4、设置一个激活命令，当激活时，打开第二个Element及以后的Rigidbody2D上gravityScale，设置DistanceJoint2D的drag属性，并关闭autoConfigureDistance开关，使distance不会再次更新。当然，也应该确保maxDistanceOnly开关开启，和breakForce设置为Infinity（默认）。
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b07f741659.png)
5、Element的结构如图，Connexions下的四个Transform是为了让上下Element找到对应的目标，以保证红色的绳子(Ropes：R1和R2)能够得到正确的旋转及长度设置（其实如果为了获取旋转而不调整绳子长度，直接获取上下Rigidbody2D相对位置，效果应该也是一样的）。
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b0833891bd.png)
6、绳子要设定正确的锚点，这样旋转起来效果才能正确

**关键代码：**
HHDScrollViewPhysics2D类：
Scroll View上带的脚本，留出为Scroll View添加Element的接口，添加的每一个Element都必须带有HHDScrollViewItem_TwoNode脚本
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b106646010.png)
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b107f0426a.png)
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b108e4a6bb.png)
HHDScrollViewItem_TwoNode类：
每一个Element上带的脚本
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b10a84af82.png)
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b11330307d.png)
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b100c0b7c9.png)
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b12c9f073c.png)
![在这里插入图片描述](http://gadimg-10045137.image.myqcloud.com/20170728/597b113e6b780.png)
