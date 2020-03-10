---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity鼠标控制物体的旋转、移动、缩放等
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
以前转载了一篇关于在Unity3d中鼠标控制物体的旋转、移动、缩放的代码
然后发现错误比较多，就重新写了一个代码，自我感觉简单实用。。。特意分享出来供大家参考
原文章也贴出来吧
【Unity3d 鼠标滚动拉近模型、鼠标右键旋转模型、鼠标中键拖拽模型】
https://blog.csdn.net/q764424567/article/details/78630649

效果图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190515161216634.gif)

## 二、知识点
- Input.GetMouseButton(0)
获取鼠标输入，参数为一个int值
为0的时候获取的是左键

- Input.GetMouseButton(1)
为1的时候获取的是右键

- Input.GetMouseButton(2)
为2的时候获取的是中键（就是那个滑轮）

- Input.GetMouseButton
鼠标按压

- Input.GetMouseButtonUp
鼠标点击

- Input.GetMouseButtonDown
鼠标松开

- Camera.main.ScreenToWorldPoint
屏幕坐标转化为世界坐标

- Quaternion rotation = Quaternion.Euler(0, 0, 0);
欧拉角转化为四元数


## 三、代码分享

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MouseControlModel : MonoBehaviour
{
    //旋转最大角度
    public int yMinLimit = -20;
    public int yMaxLimit = 80;
    //旋转速度
    public float xSpeed = 250.0f;
    public float ySpeed = 120.0f;
    //旋转角度
    private float x = 0.0f;
    private float y = 0.0f;

    void Update()
    {
        if (Input.GetMouseButton(0))
        {
            //将屏幕坐标转化为世界坐标  ScreenToWorldPoint函数的z轴不能为0，不然返回摄像机的位置，而Input.mousePosition的z轴为0
            //z轴设成10的原因是摄像机坐标是（0，0，-10），而物体的坐标是（0，0，0），所以加上10，正好是转化后物体跟摄像机的距离
            Vector3 temp = Camera.main.ScreenToWorldPoint(new Vector3(Input.mousePosition.x, Input.mousePosition.y, 10));
            transform.position = temp;
        }
        else if (Input.GetMouseButton(1))
        {
            //Input.GetAxis("MouseX")获取鼠标移动的X轴的距离
            x += Input.GetAxis("Mouse X") * xSpeed * 0.02f;
            y -= Input.GetAxis("Mouse Y") * ySpeed * 0.02f;
            y = ClampAngle(y, yMinLimit, yMaxLimit);
            //欧拉角转化为四元数
            Quaternion rotation = Quaternion.Euler(y, x, 0);
            transform.rotation = rotation;
        }
        else if (Input.GetAxis("Mouse ScrollWheel") != 0)
        {
            //鼠标滚动滑轮 值就会变化
            if (Input.GetAxis("Mouse ScrollWheel") < 0)
            {
                //范围值限定
                if (Camera.main.fieldOfView <= 100)
                    Camera.main.fieldOfView += 2;
                if (Camera.main.orthographicSize <= 20)
                    Camera.main.orthographicSize += 0.5F;
            }
            //Zoom in  
            if (Input.GetAxis("Mouse ScrollWheel") > 0)
            {
                //范围值限定
                if (Camera.main.fieldOfView > 2)
                    Camera.main.fieldOfView -= 2;
                if (Camera.main.orthographicSize >= 1)
                    Camera.main.orthographicSize -= 0.5F;
            }
        }
    }

    //角度范围值限定
    static float ClampAngle(float angle, float min, float max)
    {
        if (angle < -360)
            angle += 360;
        if (angle > 360)
            angle -= 360;
        return Mathf.Clamp(angle, min, max);
    }
}

```
谢谢大家！
