---
layout: post
category: Unity3D-Plugin
title: 【DoTween插件】Unity3d Dotween插件的简单介绍及示例代码
tagline: by 恬静的小魔龙
tag: Unity3D
---

unity里面做插值动画的插件有许多，比较常见的有itween、hotween、dotween。根据大家的反馈和实际体验来说，dotween插件在灵活性、稳定性、易用性上都十分突出。这里简单介绍下它的用法，并在后文做了一些效果示例，还是不错的。

所谓”插值动画“，顾名思义就是在两个值中插入其他的值来实现动画。原理非常简单，比如想让某个物体从A地到达B地，我们只知道A和B的坐标，插值动画就可以根据”缓动函数“确定A、B间的其他值，来实现物体从A到B的”运动过程“。”缓动函数“是确定”插值“的函数，这里有全部的缓动函数效果图，图形表示，非常方便。文章最后会介绍几个常用的缓动函数，并根据代码效果来直观感受它们的不同。

DOTween的官方文档只有这一个页面，里面讲的已经非常详细了，这里在稍微叙述一下。 
实现一个dotween动画， 有”通用模式“（ The generic way）和”简单模式“（The shortcuts way），两种方式。 
通用模式的代码格式如下：

```csharp
//让myVector向量在1秒内变为 Vector3(3,4,8)向量
DOTween.To(()=> myVector, x=> myVector = x, new Vector3(3,4,8), 1);
//让myFloat变量在1秒内变为52
DOTween.To(()=> myFloat, x=> myFloat = x, 52, 1);
```
这个模式我从来就没用过…还是来看简单的写法吧。

```csharp
transform.DOMove(new Vector3(2,3,4), 1);
rigidbody.DOMove(new Vector3(2,3,4), 1);
material.DOColor(Color.green, 1);
```
上面的就不用解释了，都那么直观的写法。 
值得注意的是，dotween都有From()函数，表示物体从前面写的 位置/颜色/大小 变化到当前的状态，这个函数还是非常好用的。 
在dotween官方文档中，详细的列出的dotween可以支持的动态项目。比较常用的Transform下DOMove，DOScale，DORotate，UI下DOFade，DOColor。

下面来看一些实际应用吧。

## 示例一：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTI1MTEzOTMxMzE2?x-oss-process=image/format,png)
源代码

```csharp
sphere1.DOMoveX(20,1).SetEase(Ease.OutBack).SetRelative();
sphere2.DOMoveX(20,1).SetEase(Ease.InQuad).SetRelative();
sphere3.DOMoveX(20,1).SetEase(Ease.InOutQuad).SetRelative();
sphere4.DOMoveX(20,1).SetEase(Ease.Linear).SetRelative();
sphere5.DOMoveX(20,1).SetEase(Ease.InOutCubic).SetRelative();
```
上面的代码使sphere在1秒内向x轴相对（SetRelative()）的移动了+20的位置。中间的SetEase()函数确定了使用那个缓动函数，可以看出不同的缓动函数可以实现不同的动画过度效果。 
5个小球同时达到终点，但是运动过程却不一样，有些先快后慢，有些先慢后快，有些会超过终点再折回。

## 示例二： 
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTI1MTE0MDIzMTcz?x-oss-process=image/format,png)
源代码

```csharp
void Show()
{
    //boxes引用了图片中的那些方块Transform
    foreach(var j in boxes)
        j.DOScale(new Vector3(1,1,1),0.5f);
}
void Hide()
{
    foreach(var j in boxes)
        j.transform.DOScale(new Vector3(0,0,0),0.4f);
}
```
## 示例三： 
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTI1MTE0MTA3OTIx?x-oss-process=image/format,png)
源代码

```csharp
void ShowAnimation()
{
     StartCoroutine(Show());
}
IEnumerator Show()
{
    //items数组引用了图片中的那几个长条Transform
    foreach(var item in items)
    {
        item.DOLocalMoveX(-1000,1f,false).From().SetEase(Ease.OutBack);
        yield return new WaitForSeconds(0.02f);
    }
}
```
