---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity3D判断得到的字符串是否可以转化为数字
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
在项目中可能会用到将得到的字符串转化为数字的要求，但是如何判断用户输入的字符串是否就是数字呢
就需要加一个判断的方法
可能用到比较多，就提出来写一下

## 二、代码

```csharp
public bool IsNumberic(string str, int result)
    {
        bool isNum;
        isNum = int.TryParse(str, out result);
        return isNum;
    }
```
调用

```csharp
int num=0;
string str="123";
if(IsNumberic(str,num))
{
	//输入的是数字，num是转化后的int值
}
else
{
	//输入的不是数字
}
```

