---
layout: post
category: Unity3D-Daily
title: 【Unity3D】UGUI 图片实现进度条动画
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
今天分享一个UGUI 图片实现进度条动画的方法，配合上资源异步加载，可以作为场景加载动画

下面就先看一下效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830152945110.gif)
## 二、资源下载
图片资源：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830153027245.png)
项目资源：
https://download.csdn.net/download/q764424567/11644403


## 三、教程
1、首先设置界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830154246710.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830154257223.png)
2、设置Image的属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830154318561.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
主要是为了控制Fill Amount，来实现进度条的进度推进


3、编写代码Loading.cs

```csharp
using UnityEngine;
using UnityEngine.UI;

public class Loading : MonoBehaviour
{
	//进度条 image
    public Image m_Image;
    //显示的进度文字 100%
    public Text m_Text;
    //控制进度
    float m_CurProgressValue = 0;
    float m_ProgressValue = 100;

    void Update()
    {
        if (m_CurProgressValue < m_ProgressValue)
        {
            m_CurProgressValue++;
        }
        //实时更新进度百分比的文本显示 
        m_Text.text = m_CurProgressValue + "%";
        //实时更新滑动进度图片的fillAmount值  
        m_Image.GetComponent<Image>().fillAmount = m_CurProgressValue / 100f;
        if (m_CurProgressValue == 100)
        {
            m_Text.text = "OK";
            //这一块可以写上场景加载的脚本
        }
    }
}

```
4、 拖入插槽中
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019083015453751.png)

OK，按下Play，去看下效果吧

