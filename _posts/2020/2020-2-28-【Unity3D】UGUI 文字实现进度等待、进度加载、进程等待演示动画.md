---
layout: post
category: Unity3D-Daily
title: 【Unity3D】UGUI 文字实现进度等待、进度加载、进程等待演示动画
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
今天分享一下UGUI 文字实现进度等待、进度加载、进程等待演示动画，实现思路比较简单，效果也比较简单，仅供大家参考，谢谢

效果演示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830160209987.gif)

## 二、资源
源代码：https://download.csdn.net/download/q764424567/11644430

## 三、正文
编写脚本TextLoading.cs

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class TextLoading : MonoBehaviour
{
    public Text m_Text;
    float m_CurProgressValue2 = 0;
    float m_ProgressValue2 = 100;
    float m_CurProgressValueTemp = 0;

    void Update()
    {
        if (m_CurProgressValue2 < m_ProgressValue2)
        {
            m_CurProgressValue2++;
        }
        if (m_CurProgressValue2 == 100)
        {
            m_CurProgressValue2 = 0;
        }
        m_CurProgressValueTemp = m_CurProgressValue2 / 100f;
        if (m_CurProgressValueTemp > 0.1f && m_CurProgressValueTemp <= 0.3f)
        {
            m_Text.text = "○○○○●";
        }
        else if (m_CurProgressValueTemp > 0.3f && m_CurProgressValueTemp <= 0.5f)
        {
            m_Text.text = "●○○○○";
        }
        else if (m_CurProgressValueTemp > 0.5f && m_CurProgressValueTemp <= 0.7f)
        {
            m_Text.text = "○●○○○";
        }
        else if (m_CurProgressValueTemp > 0.7f && m_CurProgressValueTemp <= 0.9f)
        {
            m_Text.text = "○○●○○";
        }
        else
        {
            m_Text.text = "○○○●○";
        }
    }
}

```
代码比较简单，就不做注释了

