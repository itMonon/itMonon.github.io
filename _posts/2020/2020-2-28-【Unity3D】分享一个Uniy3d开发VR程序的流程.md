---
layout: post
category: Unity3D-Daily
title: 【Unity3D】分享一个Uniy3d开发VR程序的流程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
最近做VR项目比较多，也学习了很多的东西，现在把开发的流程，已经用到的技术以及软件总结一下，分享出来供大家参考，本文内容比较基础，有什么不对的地方，希望大家能指正出来。

## 二、设备
先带大家认识一下VR设备吧，现在VR/AR设备非常的多，对于想要进行VR/AR开发的开发者真实眼花缭乱，今天就给大家简单总结一下VR/AR设备。

目前的VR/AR设备按照硬件形态可以分为三大类：
主机VR头显、手机VR眼镜和VR一体机
具体的参数介绍就参考我[VR设备盘点](https://blog.csdn.net/q764424567/article/details/83751487)这篇文章吧。

## 三、SteamVR
这个是电脑开发VR的基础，可以直接在Steam里面搜索SteamVR（库-工具），然后下载安装就行了
在这之前记得把VR设备都连接好，我用的是HTC Vive，就用HTC VIVE为例吧。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105151552536.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
线都连接好，然后两个定位器定位到正确的位置
打开SteamVR，然后进行房间设置，设置完成后就可以愉快的开发了。

## 四、正式开始

### 下载SteamVR插件
在Unity商店中，搜索SteamVR，找那个免费的安装就行了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105151940368.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)


![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105152033252.png)

### 设置"[CameraRig]"的位置
[CameraRig]相当于VR在项目中摄像机的位置，也就是人能看到的位置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105152240633.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105152320100.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105152307324.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 下载VRTK插件
链接：https://pan.baidu.com/s/1IdHcPknTZHRavg7YhvZWjA 
提取码：kl2f 
也可以直接在商店中搜索VRTK，但是商店里面这个版本没有用过，我还是用的老版本的
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018110515270267.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105152827130.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
设置一下VRTK的参数
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105152851415.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

VRTK自带的一些DEMO也可以学习一下，很全面
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018110515292938.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

### 现在就可以用手柄瞬移了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105153034808.png)

### 手柄事件
#### 1.手柄按键
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105154142472.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
1 - 菜单键
2 - 圆盘左
3 - 圆盘上
4 - 圆盘右
5 - 圆盘下
6 - 系统键（按下后手柄断开连接，再次按下手柄再次连接上）
7 - 扳机键
8 - 握持键
9 - 触摸板键

#### 2.圆盘触摸事件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105153647148.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
#### 4.触摸板轴的获取
![在这里插入图片描述](https://images2015.cnblogs.com/blog/497526/201606/497526-20160620202737334-1439461029.png)
![在这里插入图片描述](https://images2015.cnblogs.com/blog/497526/201606/497526-20160620202809209-829198118.png)
通过以上两种方式获取的VRControllerState_t，获取触摸板的轴
x = state.rAxis0.x
y = state.rAxis0.y
#### 5.手柄按键事件
通过Device.GetPressDown / GetPressUp / GetPress获取按键事件
Press是按压事件
通过Device.GetTouchDown / GetTouchUp / GetTouch获取按键事件
Touch是触摸事件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105154015153.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
#### 6.手柄自带API
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181105155335446.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)


## 四、后言
本文章只是介绍了Unity3d开发VR程序的一般流程，当然也是最简单的流程，主要是讲的一些前期开发的准备，更像是一个开发入门的教程。最后的API介绍也是最初级的事件演示，到底在项目中想要达到什么样的效果，也是要靠大家自己的想象力去开发了。
