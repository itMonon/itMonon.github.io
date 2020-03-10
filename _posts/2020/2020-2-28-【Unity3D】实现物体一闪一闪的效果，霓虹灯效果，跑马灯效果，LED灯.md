---
layout: post
category: Unity3D-Daily
title: 【Unity3D】实现物体一闪一闪的效果，霓虹灯效果，跑马灯效果，LED灯
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 前文
最近有一个需求是要实现物体闪烁的效果，就像地下出现珍宝一样- -，运用还是比较广的，下面的例子只是一个参考，还有很多花式玩法等待大家开发。
## 思路
这个就比较简单了，就是设一个全局变量控制闪烁的间隔，然后控制物体的MeshRenderer的开关就可以实现，其他更炫酷的效果，就等待大家思考了，比如控制粒子播放时间然后消失出现，效果也还好
## 代码
```csharp
using UnityEngine;
using System.Collections;
using UnityEngine.UI;

public class ShowHide : MonoBehaviour
{
    //创建一个常量，用来接收时间的变化值
    private float shake;
    //通过控制物体的MeshRenderer组件的开关来实现物体闪烁的效果
    private MeshRenderer BoxColliderClick;
    // Use this for initialization
    void Start()
    {
        BoxColliderClick = gameObject.GetComponent<MeshRenderer>();
    }

    // Update is called once per frame
    void Update()
    {
        shake += Time.deltaTime;
        //Debug.Log(shake);
        //取余运算，结果是0到被除数之间的值
        //如果除数是1 1.1 1.2 1.3 1.4 1.5 1.6 
        //那么余数是0 0.1 0.2 0.3 0.4 0.5 0.6
        if (shake % 1 > 0.5f)
        {
            BoxColliderClick.enabled=true;
        }
        else
        {
            BoxColliderClick.enabled=false;
        }
    }
}
```
