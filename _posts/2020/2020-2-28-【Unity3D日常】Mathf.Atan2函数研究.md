---
layout: post
category: Unity3D-Daily
title: 【Unity3D日常】Mathf.Atan2函数研究
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
最近有用到这个函数，就把用这个函数的小技巧记录一下，可以让我后面可以复习一下

效果：
3d效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190924104955964.gif)
2d效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190924105028562.gif)

## 二、官方API
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190924104604226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
这个意思就是返回弧度角的正切是y/x。 
返回值是x轴与起始零点和终点2D向量(x,y)的之间夹角。 
注意此函数，x为0的情况，返回正确的角度，而不是抛出被零除的异常。 


接下来就让我们来看看这个函数怎么用

## 三、用法
代码：

```csharp
using UnityEngine;

public class TestGetAnget : MonoBehaviour
{
    public Transform m_target1;
    public Transform m_target2;
    public Transform target;
    public Transform target1;

    void Update()
    {
        GetAnglev3();
        GetAngle(m_target1, m_target2);
    }

    void GetAnglev3()
    {
        Vector3 relative = target1.InverseTransformPoint(target.position);
        float angle = Mathf.Atan2(relative.x, relative.z) * Mathf.Rad2Deg;
        target1.Rotate(0, angle, 0);
    }

    void GetAngle(Transform target1,Transform target2)
    {
        Vector3 dir = target1.position - target2.position;
        float angle = Mathf.Atan2(dir.y, dir.x) * Mathf.Rad2Deg;
        m_target1.rotation = Quaternion.AngleAxis(angle, Vector3.forward);
    }
}

```
3D演示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190924110234700.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
这个时候就可以看下我们的效果了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190924104955964.gif)
2d的演示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190924105533655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190924105028562.gif)
