---
layout: post
category: Unity3D-Plugin
title: 【DoTween】Unity DoTween里面的DoPath研究
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
发现网上对于这个DoTween的DoPath研究使用的文章很少，我就把我使用过程中出现的问题，以及总结的经验给大家分享一个下吧。

## 二、DoTween介绍
DoTween是一款动画插件，主要原理就是计算两者直接的差值，进行各种平滑移动，底层也是Unity自带的Lerp运算，但是比Unity自带的Lerp更加灵活，功能更加强大，稳定性，易用性都比较退出。
所有的API：![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418183322429.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190418183343810.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
常用的API:

     DOMove  
     DOMove(this Transform target, Vector3 endValue, float duration, bool snapping = false);
     参数：Vector3 endValue：想要到达的点
  		   float duration：持续的时间，也就是到达这个点需要的时间
    	   bool snapping = false：是否是剧烈运行
       使用方法：transform.DOMove(new Vector3(0, 0, 0), 2f, false); 
       
     DOMoveX  
     DOMoveX (this Transform target, float endValue, float duration, bool snapping = false);
     参数：float endValue：x轴移动的距离
  		   float duration：持续的时间，也就是到达这个点需要的时间
    	   bool snapping = false：是否是剧烈运行
       使用方法：transform.DOMove(3f, 2f, false); 
       
     DOMoveY
     DOMoveZ
     DOLocalMove
     DOLocalMoveX
     DOLocalMoveY
     DOLocalMoveZ
     DOLocalRotate
     用法都差不多，就不多做介绍了

## 三、DOPath和DoLocalPath的使用

```csharp
public static TweenerCore<Vector3, Path, PathOptions> DOLocalPath(this Transform target, Vector3[] path, float duration, PathType pathType = PathType.Linear, PathMode pathMode = PathMode.Full3D, int resolution = 10, Color? gizmoColor = null);
public static TweenerCore<Vector3, Path, PathOptions> DOLocalPath(this Transform target, Path path, float duration, PathMode pathMode = PathMode.Full3D);
```
参数介绍：
<font color="red">Vector3[] path：</font>路径上面的点，根据点行程路径
<font color="red">float duration：</font>持续的时间，就是行走完这条路径需要的时间
<font color="red">PathType pathType = PathType.Linear：</font>路径的类型，有两种类型，一种是直线类型，一种是曲线类型
<font color="red">PathMode pathMode = PathMode.Full3D：</font>路径的形式模式，有四种类型，Ignore：忽略路径模式，因此使用观察行为，Full3D：常规的3D浏览模式，TopDown2D：2D的从上向下路径，Sidescroller2D：2D弯曲路径
<font color="red">int resolution=10：</font>分辨率，分辨率越高路径越清晰
<font color="red">Color? gizmoColor = null：</font>设置路径的颜色，或者默认为null
<font color="red">Path path：</font>这个需要初始化路径类，<font color="blue">public Path(PathType type, Vector3[] waypoints, int subdivisionsXSegment, Color? gizmoColor = null);</font>设置路径的类型，点坐标，细分X段，还有颜色


OK，演示一下怎么用

```csharp
//第一种使用方法
Vector3[] m_Path = new Vector3[10];
for(int i=0; i<10;i++)
{
	m_Path[i]=new Vector3(i,i+1,i+2);
}
transform.DoLocalPath(m_Path,20f,PathType.Linear,PathMode.Full3D);
//第二种使用方法
Vector3[] m_Path = new Vector3[10];
for(int i=0; i<10;i++)
{
	m_Path[i]=new Vector3(i,i+1,i+2);
}
DG.Tweening.Plugins.Core.PathCore.Path m_DoTweenPath = new DG.Tweening.Plugins.Core.PathCore.Path(PathType.Linear,m_Path,10,Color.red);
transform.DoLocalPath(m_DoTweenPath, 10, PathMode.Full3D);
```

## 四、回调函数
有时间有用到路径移动完之后调用函数，这个怎么做呢
其实DoTween已经提供了此类API：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190419085543712.png)
<font color="red">OnComplete：</font>当DoTween动画调用完成后调用
<font color="red">OnKill：</font>当DoTween动画被杀死的那一刻调用
<font color="red">OnPause：</font>当DoTween动画暂停的那一刻调用
<font color="red">OnPlay：</font>当DoTween动画开始的那一刻调用
<font color="red">OnRewind：</font>当重复DoTween动画的那一刻调用，这个主要适用于路径loop的情况
<font color="red">OnStart：</font>当DoTween动画第一次播放时调用，跟OnPlay的区别是，OnPlay在每一次暂停再开始都会调用，OnStart只调用一次
<font color="red">OnStepComplete：</font>当DoTween完成一个循环周期时候调用
<font color="red">OnUpdate：</font>当DoTween运行的时候一直调用
<font color="red">OnWaypointChange：</font>当路径点变化的时候调用，主要用于DoPath函数

下面说一下怎么用
<font color="red">OnComplete</font>的使用：

```csharp
//比如这个DoTween动画的意识是从transform从当前位置移动到new Vector3(10, 10, 10)这个位置
transform.DOMove(new Vector3(10, 10, 10), 2f, false); 
//如果我们想将transform从当前位置移动到Vector3(10, 10, 10)位置后再回到0,0,0位置怎么办呢
transform.DOMove(new Vector3(10, 10, 10), 2f, false).OnComplete(delegate () { transform.DOMove(new Vector3(0, 0, 0), 2f, false); });
//分开写就是
public void Move()
{
   transform.DOMove(new Vector3(0, 0, 0), 2f, false);
}
transform.DOMove(new Vector3(10, 10, 10), 2f, false).OnComplete(Move());
```
总结一下：
回调函数的使用，就是在DoTween调用动画后面点出想要的回调函数（委托），然后给回调函数（委托）一个函数方法的参数就行了，也可以<font color="red">delegate(){}</font>里面写方法也是可以的


其中有一个比较特殊的回调函数（委托），就是<font color="red">OnWaypointChange</font>
public static T OnWaypointChange<T>(this T t, TweenCallback<int> action) where T : Tween;
为啥特殊呢，因为别人都是传进去一个函数变量就行了，这货是要传一个有参的函数变量，那么怎么使用呢
其实也是很简单的，不过最好还是再去看看我的另一篇文章
【C#委托的介绍】https://blog.csdn.net/q764424567/article/details/89385012

```csharp
public void Move()
{
   transform.DOMove(new Vector3(0, 0, 0), 2f, false);
}
Vector3[] m_Path = new Vector3[10];
for(int i=0; i<10;i++)
{
	m_Path[i]=new Vector3(i,i+1,i+2);
}
transform.DoLocalPath(m_Path,20f,PathType.Linear,PathMode.Full3D).OnWaypointChange(p => Move());
```
没错，就是这么简单，这个主要是可以传递有参函数变量


## 五、DOPath运行时，LookAt路径
这一段主要讲一下，如果在路径运行的时候，一直面向将要移动的路径的方向

```csharp
	//注视函数，DoLookAt函数比较简单，跟Unity自带的LookAt差不多，就不多讲了
	int i = 0;
    public void MoveOver(Vector3[] m_Path)
    {
        i += 1;
        transform.DOLookAt(m_Path[i], 1f, AxisConstraint.Y, null);
    }
    transform.DOLocalPath(m_Path, 25f, PathType.Linear, PathMode.Full3D).OnWaypointChange(p => { MoveOver(m_Path); });
```
这样就OK了


总结一下：
这个主要是用到了DoTween的回调函数（委托），OnWaypointChange，这个回调函数主要是用于DoPath这个函数，当DoPath函数中移动到下一个点的时候就调用这个函数，就会自动注视到下一个点的位置，就可以一直面向路径的方向了

