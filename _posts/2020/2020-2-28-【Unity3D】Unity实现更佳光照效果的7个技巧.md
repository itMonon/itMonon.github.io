---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity实现更佳光照效果的7个技巧
tagline: by 恬静的小魔龙
tag: Unity3D
---

适当为游戏场景添加光照效果，能够有效增强场景氛围，让玩家体验更佳。今天将为大家分享在Unity中调整光照特效的7个技巧，让整个游戏场景氛围更引人入胜。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141040620.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
## 1.使用线性颜色空间
在为场景添加光照效果之前， 尽量将颜色空间设为线性（Linear）。线性颜色空间会更接近真实环境的渲染效果。Unity默认采用Gamma Color Space ，点击Edit > Project Settings > Player，在Other Settings下找到Color Space属性，并将其设为Linear。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141058406.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*Gamma与Linear效果对比（无后处理）*
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141118298.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*Gamma与Linear效果对比（含后处理）*

## 2.使用全局光照（GI）
使用全局光照（GI）能够实现更加逼真的光照。全局光照系统能够对光照在表面反射或折射到其它表面（间接光照）的方式进行建模，而非限定光照只能从光源照射到某个表面。 

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141148856.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*实时全局光照开关对比*

可以在光照设置面板中启用全局光照，依次点击Window > Lighting > Settings，打开Lighting面板，在Scene标签下勾选Realtime Global Illumination，设置Indirect Resolution可以改变实时全局光照的分辨率。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141216101.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)


烘焙全局光照（Baked GI）和预计算实时GI都存在一些限制，二者均只计算静态对象的光照，所以动态对象无法反射光照到其它对象，反之亦然。但动态对象可以利用光照探头（Light Probes）来接受静态对象反射的光照。
 
## 3.光照颜色协调
设置光照时必须关注其颜色对场景整体氛围的影响，以创造更加美妙而和谐的光照。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141239690.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
 *2个主色，蓝色与橙色互补*
 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141257811.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*3个相似的主色，从绿到黄*
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/201906281413184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
 *2个主色，蓝色与橙色互补*
 
 不同的光照颜色会为场景带来完全不同的氛围：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141341688.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
## 4 尽量使用浅色
光照尽量使用浅色，不要使用饱和度过高的颜色，浅色光照看起来更加自然，也更令人舒适。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141426357.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
饱和度高的光照与浅色光照对比


## 5.不断调整设置
不断尝试改变光照方向及阴影，查看并对比不同的效果。以找到最合适的设置。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141448564.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)


## 6.调整环境颜色
尝试调整环境颜色（Ambient Color）来改变阴影颜色，从而获得更加逼真而自然的阴影效果。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141506630.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
 依次点击Window > Lighting > Settings打开Lighting面板，在Scene标签下将Environment Lighting Source设为Color，然后将Ambient Color 设为合适的颜色。
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141519475.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

## 7.使用后处理特效
使用Unity提供的后处理特效包，可以让整个场景的光照效果更加强烈。可以从Asset Store资源商店下载该后处理特效资源包。下面是在Unity 5.6中使用后处理特效的示例：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141536559.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190628141542614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*后处理特效是否启用的对比*
