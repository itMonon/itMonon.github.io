---
layout: post
category: Unity3D-Plugin
title: 【ITween】Unity3d iIween插件的研究
tagline: by 恬静的小魔龙
tag: Unity3D
---

<h1>首先是插件的下载路径</h1>
官方下载地址
 http://itween.pixelplacement.com/index.php 
 ![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTEwMTQxMzAwNTMy?x-oss-process=image/format,png)
 点击Get iTween小图标 ->Download iTween

或者 AssetStores商店中直接下载

<h1>如何把iTween插件加入到项目中</h1>


在项目中建立Plugins目录， 然后将下载的iTween.cs放到Plugins目录即可。

如果需要编辑路径, 使用import package->custom package菜单功能加入iTweenPath.unitypackage。


<h1>iTween 支持的功能：</h1>
控制音频：AudioFrom、AudioTo、AudioUpdate、Stab
控制相机：CameraFadeAdd、CameraFadeDepth、CameraFadeDestroy、CameraFadeSwap、CameraFadeFrom、CameraFadeTo、CameraTexture
变色控制：ColorFrom、ColorTo、ColorUpdate
绘制相关：DrawLine、DrawLineGizmos、DrawLineHandles、DrawPath、DrawPathGizmos、DrawPathHandles
淡入淡出：FadeFrom、FadeTo、FadeUpdate
视角控制：LookFrom、LookTo、LookUpdate、LookType
移动控制：MoveAdd、MoveBy、MoveFrom、MoveTo、MoveUpdate
路径操作：PutOnPath、PointOnPath
旋转操作：RotateAdd、RotateBy、RotateFrom、RotateTo、RotateUpdate
缩放操作：ScaleAdd、ScaleBy、ScaleFrom、ScaleTo、ScaleUpdate
震动控制：ShakePosition、ShakeRotation、ShakeScale
其他：Count、PathLength、EaseType(衰减类型)、FloatUpdate、Hash、Init、Pause、Resume、Stop、StopByName、PunchPosition、PunchRotation、PunchScale、RectUpdate、ValueTo、Vector2Update、Vector3Update


<h1>移动模型时候用到的几个核心方法</h1>
iTween.MoveTo()： 让模型移动到一个位置，它的底层函数是通过动态的修改模型每一帧的transform.position完成的，所以它会百分之百到达目标点，不会出现误差。

iTween.MoveFrom():它和上面的一样，iTween.MoveTo()是将模型移动到目标位置，而iTween.MoveFrom()是将模型从目标位置移动到原始位置。

iTween.MoveAdd() 和iTween.MoveBy()底层实现一样，大家可以去看源码。处理移动时采用的是transform.Translate也就是API的平移，这样在处理移动的时候可能会出现一些误差，但是效果好点。

iTween.MoveUpdate():和iTween.MoveTo()差不多，只是它需要放在循环或者Update()中。

有了核心的移动方法后，我们就来了解iTween强大的核心参数，与事件。移动方法的参数都差不多，所以这里我就以MoveTo来做例子。直接上代码。

Move.cs绑定在需要移动的游戏对象身上。



```
using UnityEngine;
using System.Collections;
 
public class Move : MonoBehaviour
{	
 
	void Start()
	{
 
		//键值对儿的形式保存iTween所用到的参数
		Hashtable args = new Hashtable();
 
		//这里是设置类型，iTween的类型又很多种，在源码中的枚举EaseType中
		//例如移动的特效，先震动在移动、先后退在移动、先加速在变速、等等
		args.Add("easeType", iTween.EaseType.easeInOutExpo);
 
		//移动的速度，
		args.Add("speed",10f);
		//移动的整体时间。如果与speed共存那么优先speed
		args.Add("time",1f);
		//这个是处理颜色的。可以看源码的那个枚举。
		args.Add("NamedValueColor","_SpecColor");
		//延迟执行时间
		args.Add("delay", 0.1f);
		//移动的过程中面朝一个点
		args.Add("looktarget",Vector3.zero);
 
		//三个循环类型 none loop pingPong (一般 循环 来回)	
		//args.Add("loopType", "none");
		//args.Add("loopType", "loop");	
		args.Add("loopType", "pingPong");
 
		//处理移动过程中的事件。
		//开始发生移动时调用AnimationStart方法，5.0表示它的参数
		args.Add("onstart", "AnimationStart");
		args.Add("onstartparams", 5.0f);
		//设置接受方法的对象，默认是自身接受，这里也可以改成别的对象接受，
		//那么就得在接收对象的脚本中实现AnimationStart方法。
		args.Add("onstarttarget", gameObject);
 
		//移动结束时调用，参数和上面类似
		args.Add("oncomplete", "AnimationEnd");
		args.Add("oncompleteparams", "end");
		args.Add("oncompletetarget", gameObject);
 
		//移动中调用，参数和上面类似
		args.Add("onupdate", "AnimationUpdate");
		args.Add("onupdatetarget", gameObject);
		args.Add("onupdateparams", true);
 
		// x y z 标示移动的位置。
 
		args.Add("x",5);
		args.Add("y",5);
		args.Add("z",1);
 
		//当然也可以写Vector3
		//args.Add("position",Vectoe3.zero);
 
		//最终让改对象开始移动
		iTween.MoveTo(gameObject,args);	
	}
 
    //对象移动中调用
	void AnimationUpdate(bool f)
	{
		Debug.Log("update :" + f);
	}
	//对象开始移动时调用
	void AnimationStart(float f)
	{
		Debug.Log("start :" + f);
	}
	//对象移动时调用
	void AnimationEnd(string f)
	{
		Debug.Log("end : " + f);
	}
 }

```
比较重要的参数是这个：
<h3>easyType</h3>
args.Add("easeType", iTween.EaseType.easeInOutExpo);
这个参数是指移动的特效，先震动在移动、先后退在移动、先加速在变速、等等
具体效果在下面这个链接中可以查看
http://www.robertpenner.com/easing/easing_demo.html




<h3>looktarget</h3>
args.Add("looktarget",Vector3.zero);
移动的过程中面朝一个点


在看看iTween中的寻路算法，其实非常非常的简单，我们几乎不用做任何事情。如下图所示，我们能清楚的看到编辑了一个简单的寻路，我们通过iTween 来实现小人跑步到终点。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMTEwMTQyMTU2Nzcw?x-oss-process=image/format,png)

<h1>按路径移动</h1>

<h2>第一种写法</h2>

```
var path = GameObject.Find("Plane").GetComponent("iTweenPath").GetPath("myPath");
iTween.MoveTo(gameObject, iTween.Hash(//"position", Vector3(0, 0, 0),
"path", path,
"time", 20,
"easetype", "linear"));
```




<h2>第二种写法</h2>

Path.cs  绑在主角身上即可。

```
using UnityEngine;
using System.Collections;
 
public class Path : MonoBehaviour {
 
	//路径寻路中的所有点
	public Transform [] paths;
 
	void Start () 
	{
		Hashtable args = new Hashtable();
	    //设置路径的点
		args.Add("path",paths);
		//设置类型为线性，线性效果会好一些。
		args.Add("easeType", iTween.EaseType.linear);
		//设置寻路的速度
		args.Add("speed",10f);
		//是否先从原始位置走到路径中第一个点的位置
		args.Add("movetopath",true);
		//是否让模型始终面朝当面目标的方向，拐弯的地方会自动旋转模型
		//如果你发现你的模型在寻路的时候始终都是一个方向那么一定要打开这个
		args.Add("orienttopath",true);
 
	    //让模型开始寻路	
		iTween.MoveTo(gameObject,args);
	}
 
	void OnDrawGizmos()
	{
		//在scene视图中绘制出路径与线
		iTween.DrawLine(paths,Color.yellow);
 
		iTween.DrawPath(paths,Color.red);
 
	}
 
}
```
注意这两个参数：

args.Add("orienttopath",true);
//是否让模型始终面朝当面目标的方向，拐弯的地方会自动旋转模型
args.Add("looktarget",Vector3.zero);
//移动的过程中面朝一个点

这两个参数不能共用，如果用一个另一个要去掉




运行后即可看到主角自动寻路的效果。


<h2>在Scene视图中显示路线</h2>

```
public Transform[] paths;

void OnDrawGizmos()
    {
        if (paths.Length > 0)
        {
            iTween.DrawPath(paths);
        }
    }
```
<h2>在移动结束后调用函数</h2>

```
//移动结束时调用，参数和上面类似
        args.Add("oncomplete", "Test");
        args.Add("oncompletetarget", gameObject);

//移动结束时调用的函数
void Test()
    {
    }
```

如果你仔细阅读到这里你可能会想到，iTween做的东西有点像 Mathf.Lerp() Vector3.Lerp() lookAt()等等这类的方法。假设不使用iTween这个类就用源生的API其实也可以实现上述的所有效果。只有iTween帮我们封装的更好一些，平滑过渡的效果更好一些，而且还能增加一些特效。只是这些特效与动画全都是iTween通过数学的方法计算出来。因为底层它们使用的也是简单的 移动旋转API中的方法。我觉得寻路的话可以使用Unity自带的方法（因为是官方提供的），处理一些简单的动画使用iTween还是挺不错的，因为更加形象。
