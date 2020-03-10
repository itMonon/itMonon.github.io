---
layout: post
category: Unity3D-Plugin
title: 【DoTweenPro】Unity3d DoTweenPro研究
tagline: by 恬静的小魔龙
tag: Unity3D
---

# Unity3d DoTweenPro研究

本篇文章是记录在使用DoTweenPro插件中的一些心得体会：

- **DoTweenAnimation组件介绍**
- **DoTweenAnimation组件属性详细介绍**
- **DoTweenPath组件介绍**
- **DoTweenPath组件属性详细介绍**
- **一些心得体会**
- **遇到的坑**

##下载链接
http://dotween.demigiant.com/pro.php

-------------------

##DoTweenAnimation组件介绍
![DoTweenAnimation组件](https://img-blog.csdn.net/20180518165726980?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
 - AddManager    `添加控制器` 
 - Move\AutoPlay\AutoKill    `移动动画\自动播放\自动销毁` 
 - Duration    `持续时间`
 - Delay    `延迟时间`
 - IgnoreTImeScale    `忽略时间缩放`
 - Ease    `移动的方式`
 - Loops    `循环次数`
 - ID    `动画ID`
 - TO/From    `向目标移动\从目标过来`
 - Snapping    `激烈的`
 - Relative    `相对目标`
 - Events    `事件`

## DoTweenAnimation组件属性详细介绍

> AddManager 

添加控制器，这个主要是为了控制动画的播放的一个属性，可以设置动画什么时候播放，什么时候停止，具体怎么控制，文章后面会讲到
> Move

这个主要是设置控制动画的类型，有Move、LocalMove、Rotate、LocalRotate、Color、Fade、Scale、Text这几种形式。Move的话就是当前对象的移动，跟LocalMove主要的区别在于，Move是按照世界坐标系移动的，而LocalMove是按照自身坐标系移动的

>AutoPlay、AutoKill

自动播放、自动销毁

>Duration

持续时间，这个可以理解为动画的播放速度，持续时间越短播放速度越快，持续时间越大播放速度越慢，但是跟控制动画播放速度不一样的地方时，这个不好控制。。。
>Delay 

延迟时间，就是延迟多少秒之后开始播放动画(没错，单位就是秒)
>IgnoreTImeScale 

忽略时间缩放，就是该移动到这个位置需要多长时间，但是设置的时间不够，就会加快播放播放，但是勾选这个之后就不会了，该什么时间播放到那个位置就在那个位置
>Ease 


缓动函数，Ease.InSine 表示正弦加速动作 
Ease.OutSine 表示正弦减速动作 
Ease.InOutSine, 表示正弦加速减速动作 
指定动画效果在执行时的速度，使其看起来更加真实
>Loops

循环次数，-1表示一直循环，0表示不循环，1表示循环一次
>ID 

动画的ID，主要用于在一个物体上的多个动画片段分开，用ID来控制一个物体上某个动画
>TO/From

向目标去，还是从目标来，没啥好说的。。
>Snapping

对，激烈的，就是会抖动，并且动画频率很大，就这样
>Relative 

相对目标，就是动画相对于动画对象，如果不勾选的话，改变对象的位置，动画对象的路线不会改变。如果勾选的话，改变对象的位置，动画路线也会跟着移动
>Events 

事件。有OnStart、OnPlay、OnUpdate、OnStep、OnComplete这五个事件。主要用于在动画刚开始，或者动画播放中，或者动画运行、动画结束的时候调用函数。比如说你的车想边移动边转角度的话，就可以设置两个动画，然后一个动画OnUpdate调用另一个动画，就可以了。

##DoTweenPath组件介绍
![DoTweenPath组件](https://img-blog.csdn.net/20180518173343765?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
 >1、Scene View Commands 

（1）SHIFT+CTRL：add a waypoint 
加一个轨迹点 
（2）SHIFT+ALT：remove a waypoint 
移除一个轨迹点
 
 >2、INfo 

Path Length：轨迹长度

>3、Tween Options 

（1）AutoPlay：自动播放 
（2）AutoKill：播放完自动销毁动画 
（3）Duration：动画时长 
SpeedBased：If selected ,the duration will count as units/degreex second;如果被选上，该时间作为单位时间。
（4）Delay：延时 
（5）Ease：这是一个枚举，可以理解为动画播放速率曲线 
![这里写图片描述](https://img-blog.csdn.net/20180518174224695?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
相关网址： 
http://dotween.demigiant.com/documentation.php 
以及：http://robertpenner.com/easing/easing_demo.html

（6）Loops：循环次数，动画循环播放的次数 
-1：表示 一直循环 
0：表示只播放一次 
当该值>1时，检视面板出现LoopType，顾名思义就是指循环类型： 
Restart：重新开始，后面的动画循环播放会从起点重新开始； 
Yoyo：来回播放 
Incremental：增加的，后面的path移动动画会在上一次循环的终点的基础上进行播放

（7）ID：动画ID 
（8）UpdateType：这个枚举有三个值，指更新类型 
Normal：正常更新，Update() 
Late：晚于更新，LateUpdate() 
Fixed：固定更新，FixedUpdate()
>4、Path Tween Options 

（1）Path Type：轨迹线类型 
Linear：线型的 
Catmull Rom：曲线 
（2）Close Path：封闭曲线，将起点和终点相连 
（3）Lock Rotation：锁旋转，xyzw
>5、Path Editor Options：轨迹编辑参数，就不介绍了
>6、ResetPath：重置轨迹 
>7、Events： 

（1）OnStart：开始时 
（2）OnPlay：播放时 
（3）OnUpdate：更新时 
（4）OnStep：单步完成时 
（5）OnComplete：完成时 
（6）OnCreated：动画创建时 
事件顺序为：OnCreated->OnStart->OnPlay->OnUpdate（一直执行，直到完成），动画过程中单步完成时执行OnStep，整个动画完成后执行OnComplete
>8、WayPoints：移动轨迹点 

其中右边的Copy to clipboard，将坐标复制至剪贴板 
![这里写图片描述](https://img-blog.csdn.net/20180518174727735?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
加减按钮即指增加减少坐标点

以上便是对于DOTweenPath组件的一个基本介绍。

##接下来，DOTween对于轨迹移动提供的接口： 
>（1）DOPath： 
![这里写图片描述](https://img-blog.csdn.net/20180518174825683?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
>（2）DOLocalPath 
![这里写图片描述](https://img-blog.csdn.net/20180518174832776?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
以上两个函数的参数再上面已经介绍过，这里就不再重复说明了。

DOTween插件，大家请自行下载。
