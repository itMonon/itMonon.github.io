---
layout: post
category: Unity3D-Daily
title: 【Unity3D】LineRender组件的研究
tagline: by 恬静的小魔龙
tag: Unity3D
---

##参考文章
[Unity3D研究院之游戏对象的访问绘制线与绘制面详解（十七）](http://www.xuanyusong.com/archives/561)

##前言
发现网上很多教程都是如何用LineRender组件画线，但是这个组件还有很多其他的功能属性也是很有趣的，下面就让我们来看看吧

##用途
LineRender组件主要的用途就是画线，将这个组件加载到对象上，然后设置路径，跟线的材质，就能在Game视图下显示线段了。

##画线
要了解在Unity中的画线方式，可以参考我另一篇文章
https://blog.csdn.net/q764424567/article/details/78630798

##使用LineRender画线
-  在一个对象上加上LineRender组件

![这里写图片描述](https://img-blog.csdn.net/20180628100612749?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

-  附上材质Materials

![这里写图片描述](https://img-blog.csdn.net/2018062810070815?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

-  设置一下路径Positions

![这里写图片描述](https://img-blog.csdn.net/20180628100744961?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

-  效果就是这个样子的

![这里写图片描述](https://img-blog.csdn.net/20180628100817743?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##LineRender组件的属性
- Cast Shadows
投影，可以选择
On 开
Off 关 
Two Sided两个侧面
Shadows Only只显示阴影

- Recevice Shadows
接收阴影效果

- Motion Vectors
移动的轨迹
Camera Motion Only 只有相机移动
Per Object Motion 每一个对象移动也会跟着移动
Force No Motion强制移动

- Materials
可以设置线段的材质，可以设置成一个纯色材质，像这样的
![这里写图片描述](https://img-blog.csdn.net/20180628102158122?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
也可以制作一个带透明的箭头
![这里写图片描述](https://img-blog.csdn.net/20180628102254515?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180628102313920?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这张图片也上传上来吧
![这里写图片描述](https://img-blog.csdn.net/20180628102349987?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这张是没有水印的
https://pan.baidu.com/s/165kbJjzorUOeMMt0GJN_qA
注意导入图片的格式设置成下面这样
![这里写图片描述](https://img-blog.csdn.net/20180628104205540?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
不然可能效果显示不出来

- Lightmap Parameters
这个的话就是可以用自己的光照参数
![这里写图片描述](https://img-blog.csdn.net/2018062810303989?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- Positions
路径设置，这个可以用代码控制

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class nav : MonoBehaviour
{
    private NavMeshAgent m_Agent;
    public GameObject m_Target;
    private LineRenderer m_LineRender;
    // Use this for initialization
    void Start()
    {
        m_Agent = GetComponent<NavMeshAgent>();
        m_LineRender = GetComponent<LineRenderer>();
        //m_Agent.SetDestination(m_Target.transform.position);
    }

    // Update is called once per frame
    void Update()
    {
        //设置目标点
        m_Agent.SetDestination(m_Target.transform.position);
        //储存路径
        Vector3[] path = m_Agent.path.corners;
        //设置顶点的数量
        m_LineRender.positionCount = path.Length;
        //设置线段
        for (int i = 0; i < path.Length; i++)
        {
            m_LineRender.SetPositions(path);
        }
    }
}

```

- Use World Space
就是使用世界坐标系还是使用自身坐标系

- Loop
重复

- Width
设置线段的宽细

- Color
设置线段的颜色，可以影响到材质

- Corner Vertices
角顶点，就是转弯的点有几个，这个可以不用设置

- End Cap Vertices
后盖顶点

- Alignment
对齐方式，Local本地，就是根据自身的坐标系对齐，View视图对齐

- Texture Mode
材质模式

- Light Probes
灯光探针

- Reflection Probes
反射探针