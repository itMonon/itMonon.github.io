---
layout: post
category: Unity3D-Daily
title: 【Unity3D】UI菜单列表，滑动展示UI
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
物体或者UI的在平面上的旋转展示的代码实现，这个功能也是用的比较多的模块，可以将这个代码做成模板，在以后的项目中都会用到。

## 二、参考文章
【实现VR中物体或UI的旋转显示】http://www.manew.com/thread-110573-1-1.html


## 三、正文
本篇文章将讲解如何实现UI的旋转，主要是通过DoTween插件进行旋转的

### 实现过程
#### 1. 首先需要一个父物体上面有N个子物体，脚本挂在父物体上
子物体就这么排列就行
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190107093028380.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
挂载到父物体上面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190107091951468.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
父物体上面挂载UIRotate脚本，后面会编写脚本的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190107092010329.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

然后修改Canvas的Render Mode改为World Space
这样就可以修改UI的Z轴方向的值，实现旋转

#### 2. 新建一个Canvas
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190107092336306.png)
改这个Canvas的Render Mode改为Screen Space-Overlay
然后新建一个Button

#### 3. 编写脚本UIRotate
编写脚本文件

需要使用doTween插件达到物品平滑移动的效果.
原理:使用360度除以子物体个数就可以得出他们之间的夹角,并通过cos和sin来计算出子物体的x和z的值(请忽略Y轴,因为是水平上的一个圆内显示)

```csharp
using DG.Tweening;
using UnityEngine;

public class UIRotate : MonoBehaviour
{
    private int halfSize;
    private GameObject[] gameObjects;
    /// <summary>
    /// 圆半径
    /// </summary>
    private int r = 300;
    /// <summary>
    /// 相间角度
    /// </summary>
    private int angle;

    private void Start()
    {
        //初始化数组
        var childCount = transform.childCount;
        //计算出中点
        halfSize = (childCount - 1) / 2;
        //求出圆内角度
        angle = 360 / childCount;
        //初始
        gameObjects = new GameObject[childCount];
        for (var i = 0; i < childCount; i++)
        {
            gameObjects[i] = transform.GetChild(i).gameObject;
            SetPosition(i);
            SetDeepin(i);
        }
    }    

    /// <summary>
    /// 设置物体位置
    /// </summary>
    private void SetPosition(int index)
    {
        float x = 0;
        float z = 0;
        if (index < halfSize)
        {
            int id = halfSize - index;
            x = r * Mathf.Sin(angle * id);
            z = -r * Mathf.Cos(angle * id);
        }
        else if (index > halfSize)
        {
            int id = index - halfSize;
            x = -r * Mathf.Sin(angle * id);
            z = -r * Mathf.Cos(angle * id);
        }
        else
        {
            x = 0;
            z = -r;
        }
        Tweener tweener = gameObjects[index].GetComponent<RectTransform>().DOLocalMove(new Vector3(x, 0, z), 1);
    }

    private void SetDeepin(int index)
    {
        //计算图片深度也就是z轴的距离,离摄像机的远近
        int deepin = 0;
        if (index < halfSize)
        {
            deepin = index;
        }
        else if (index > halfSize)
        {
            deepin = gameObjects.Length - (1 + index);
        }
        else
        {
            deepin = halfSize;
        }
        gameObjects[index].GetComponent<RectTransform>().SetSiblingIndex(deepin);
    }

    /// <summary>
    /// 向后翻页
    /// </summary>
    public void OnNext()
    {
        var length = gameObjects.Length;
        for (var i = 0; i < length; i++)
        {
            var temp = gameObjects[i];
            gameObjects[i] = gameObjects[length - 1];
            gameObjects[length - 1] = temp;
        }
        for (var i = 0; i < length; i++)
        {
            SetPosition(i);
            SetDeepin(i);
        }
    }
}

```
#### 4.给Button绑定事件
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019010709274738.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)



### 效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190107092941512.gif)

### Demo的源代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190107093441246.png)
https://pan.baidu.com/s/1q4ekKHHq_7yR1pC_KCzDmw
按需下载


![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2dpZi8xaFJlSGFxYWZhZXJPZDQ2M3BydGYxQWJibXFBMXhKSzUwUmN0MEZ4ZWFLSjJ6djBxc1lPUGF5RzZnMllLbnpMUTVnbUkxbnQ5UFVQaWI2cUtPQjVnOFEvNjQw?x-oss-process=image/format,png)

