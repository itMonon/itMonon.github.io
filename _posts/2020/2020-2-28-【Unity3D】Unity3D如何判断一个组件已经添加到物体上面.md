---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity3D如何判断一个组件已经添加到物体上面
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
有时候我们需要动态添加一个组件，但是添加之后不知道是否已经添加上，或者为了避免重复添加组件，那怎么办呢


## 二、代码

```csharp
//比如要给物体添加一个Rigidbody组件
transform.AddComponent<Rigidbody>();
//获取物体上的一个组件
transform.GetComponent<Rigidbody>();
//那如果重复调用这行代码，或者下次调用这行代码的时候，就会重复添加一个Rigidbody组件那么怎么避免呢
//就可以用下面的这一行代码
Rigidbody m_Rig=transform.AddComponent<Rigidbody>() ?? transform.GetComponent<Rigidbody>();
```

## 三、总结
就是使用C#中的一个运算符 ??
如果 ?? 运算符的左操作数非 null，该运算符将返回左操作数，否则返回右操作数。
