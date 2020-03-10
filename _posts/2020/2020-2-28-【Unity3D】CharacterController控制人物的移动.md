---
layout: post
category: Unity3D-Daily
title: 【Unity3D】CharacterController控制人物的移动
tagline: by 恬静的小魔龙
tag: Unity3D
---


Unity3d使用CharacterController控制行走
-------------------------

 1. 使用Input.GetAxis("Horizontal") 和 "Vertical"得到垂直和水平方向的值
 2. 使用CharacterController.SimpleMove(Vector3)参数表示运动的方向和速度 单位可以认为是 m/s


代码如下：
 

```
private CharacterController cc;
    public float speed = 4;
    
    void Start()
    {
        cc = GetComponent<CharacterController>();
    }

    
    void Update()
    {
        float h = Input.GetAxis("Horizontal");
        float v = Input.GetAxis("Vertical");
        if (Mathf.Abs(h)>0.1f||Mathf.Abs(v)>0.1)
        {
            Vector3 targetDir = new Vector3(h, 0, v);
            transform.LookAt(targetDir+transform.position);
            cc.SimpleMove(transform.forward * speed);
        }  
    }
```

注：

 1. speed 是控制人物移动的速度
 2. float h 获取的是操纵杆输入和键盘输入，值为（-1到1）的值，x轴正方向为1，负方向为-1，也就是说A键为-1，D键为1
 3. float v获取的是操纵杆输入和键盘输入，值为（-1到1）的值，y轴正方向为1，负方向为-1，也就是说W键为1，S键为01
 4. targetDir 是键盘输入之后获取到的方向，将目标用SimpleMove方法向获取到方向移动
 5. transform.lookat 是让目标旋转到获取到的方向
 6. transform.forward 是让目标向正前方移动