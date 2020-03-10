---
layout: post
category: Unity3D-Daily
title: 【Unity3D】自动寻路系统Navigation实现人物上楼梯、走斜坡、攀爬、跳跃
tagline: by 恬静的小魔龙
tag: Unity3D
---

@[toc]
## 参考文章：列表
[Unity3D深入浅出 - 导航网格自动寻路(Navigation Mesh)](http://blog.csdn.net/yuxikuo_1/article/details/44974739)
[unity3D——自带寻路Navmesh入门教程（二）](http://www.cnblogs.com/wangweixznu/p/5443071.html)
[Unity3D自动寻路系统Navigation（三）之人物上下斜坡设置](http://blog.csdn.net/qq_27678295/article/details/53730252)
[Unity手游之路<八>自动寻路Navmesh之入门](http://blog.csdn.net/janeky/article/details/17457533)
[Unity手游之路<十>自动寻路Navmesh之跳跃,攀爬,斜坡](
https://blog.csdn.net/janeky/article/details/17598113)
[NavMesh Agent](https://docs.unity3d.com/Manual/class-NavMeshAgent.html)



## 一、Navigation面板
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTYyNjE3NjA3?x-oss-process=image/format,png)

### Navigation面板中包括几个模块
**Agents**
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTYzMTAyMjkw?x-oss-process=image/format,png)
这个是可以添加多个NabigationAgents可以用不同的Agents
参数:
Name：设置烘培Agents的名字
Radius：烘培的半径，也就是物体的烘培的半径。这个值影响物体能通过的路径的大小
越小，能行走的路径越大，边缘区域越小
Height：	具有代表性的物体的高度，可以通过的最低的空间高度。这个值越小，能通过的最小高度越小。打个比方就是，1m7的人能通过1m7的洞是正常的，你可以设置height为1m，就能通过1m的高度
Step Height ：梯子的高度，这个还是要看模型阶梯的高度来设置
Max Slope：烘培的最大的角度，就是坡度了




**Areas**
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTYzNDU5MjE1?x-oss-process=image/format,png)
这个是可以设置自动寻路烘培的层
配合Nav Mesh Agents使用
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTYzNjAyODg5?x-oss-process=image/format,png)
Nav Mesh Agents->Area Mask->可以设置可以通过哪些层
**Back**
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTYzNzM2NjI4?x-oss-process=image/format,png)
这个就是设置烘培参数的
参数：
Radius：具有代表性的物体半径，半径越小生成的网格面积越大。
Height：具有代表性的物体的高度。
Max Slope：斜坡的坡度。
Ste Height：台阶高度。
Drop Height：允许最大的下落距离。
Jump Distance：允许最大的跳跃距离。
Min Region Area：网格面积小于该值则不生成导航网格。
Height Mesh：勾选后会保存高度信息，同时会消耗一些性能和存储空间。

**Object**
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTYzODM1NTUx?x-oss-process=image/format,png)
这个是设置去烘培哪个对象，比如地形之类的，就是可以行走的范围路径

**参数：**

Scene Filter:选择场景中那些对象，可以选择全部(All)，地形(Terrains),有网格对象(Mesh Renderers)
Navigation Static:可以烘培
Generate OffMeshLinks：可以跳跃的地方
两种生成OffMeshLink的方法：
第一种生成OffMeshLink的方法
添加OffMeshLink组件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190929110042600.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**参数：**
Start：设置起点
End：设置终点
两者顺序没有关系
Cost Override：这个参数还没有研究
BI Directional：同上
Activated：同上
Auto Update Position：同上
Navigation Area：同上

第二种生成OffMeshLink的方法
在Navigation面板的Object栏里面把OffMeshLink Generation选项打上勾
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTY0NzE0NjUx?x-oss-process=image/format,png)
这里要说的一点是，是不是有的人把OffMeshLink Generation选项打上勾之后Back之后还是没有出现OffMeshLink
![这里写图片描述](https://img-blog.csdn.net/20180524155214397?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这是因为你没有设置Jump Distance的值
![这里写图片描述](https://img-blog.csdn.net/20180524155258337?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这个值越大，能跳的距离越远，然后能跳的越高

Navigation Area：表示是哪个Area，这个需要先设置，系统默认是Walkble、Jump、NotWalkble三种
这个也要配合Nav Mesh Agent使用
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTY0OTU0MTI2?x-oss-process=image/format,png)
## 二、NavMeshAgent组件
### 2.1 Agent Size
**Radius**
物体的半径
**Height**
物体的高度，如果AgentHeight的值大于这个值，那么就不能通过
**Base Offset**
偏移值
### 2.2 Steering
**Speed**
物体自动寻路的速度
**Angular Speed**
转角的速度，就是转弯的速度
**Acceleration**
加速度
**Stopping Distance**
物体停下来的距离，设置为0就是跟目标点的距离为0时停下来
**Auto Braking**
是否自动停下来
### 2.3 Obstacle Avoidance
**Quality**
如果要防止一群寻路的物体围住目标点
可以设置Quality为None，即可以让寻路物体互相穿过
**Priority**
优先权
### 2.4 Path Finding自动寻路	
**Auto Traverse Off Mesh Link**
自动跳跃链接
**Auto Repath**
自动复制路径
**Area Mask**
能通过的Maks层，这个可以配合Navigation组件中Areas（设置层的）使用


## 三、NavMeshObstacle组件 障碍物组件
![这里写图片描述](https://img-blog.csdn.net/20180725114500889?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
如果想要在场景中，动态的放置障碍物，然后也不想在场景开始前就洪培好地形的话，就可以在物体上加上这个组件，然后设置好参数，将自动寻路组件NavMeshAgent的寻路避让优先级调高一点
**Shape**
障碍物的模型，有Box和Capsule两个选项，从单词意思就可以看出来什么意思就不解释了
**Center**
中心点，如果障碍模型的中心点不在模型的中心点上就可以做一些调整
**Size**
设置大小
**Carve**
Move Threshold 模型 移动某个距离后进行烘焙 
Time To Stationary 指定模型在某个位置停止一段时间 后 在进行烘焙 
Carve One Stationary 勾选后，模型移动时不会实时烘焙


## 四、实例例子

**步骤一般是这样的：**
1.在场景中摆放各种模型，包括地板，斜坡，山体，扶梯等
2.为所有的模型加上Navigation Static和OffMeshLink Generatic(这个根据需要，例如地板与斜坡相连，斜坡就不需要添加OffMeshLink)
3.特殊处理扶梯，需要手动添加Off Mesh Link，设置好开始点和结束点
4.保存场景，烘焙场景


### 例子一：简单寻路
我们要实现一个功能：点击场景中的一个位置，角色可以自动寻路过去。角色会绕过各种复杂的障碍，找到一条理论上”最短路径“。

**步骤：**
1.创建地形
2.添加角色
3.创建多个障碍物，尽量摆的复杂一点，来检查Navmesh的可用性和效率。
4.选中地形，在Navigation窗口中，设置Navigation Static
5.依次选中障碍物，在avigation窗口中，设置Navigation Static
7.Navigation窗口中，选择Bake（烘焙）界面，点击Bake按钮，进程场景烘焙，就可以烘焙出寻路网格了
8.为角色添加NavMeshAgent组件。Component->Navigation->Nav Mesh Agent
9.为角色新增一个脚本PlayerController.cs，实现点击目标，自动寻路功能

**代码：**

```csharp
using UnityEngine;
using System.Collections;

public class PlayerController : MonoBehaviour
{
    private NavMeshAgent agent;


    void Start()
    {
        //获取组件
        agent = GetComponent<NavMeshAgent>();
    }


    void Update()
    {
        //鼠标左键点击
        if (Input.GetMouseButtonDown(0))
        {
            //摄像机到点击位置的的射线
            Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
            RaycastHit hit;
            if (Physics.Raycast(ray, out hit))
            {
                //判断点击的是否地形
                if (!hit.collider.name.Equals("Terrain"))
                {
                    return;
                }
                //点击位置坐标
                Vector3 point = hit.point;
                //转向
                transform.LookAt(new Vector3(point.x, transform.position.y, point.z));
                //设置寻路的目标点
                agent.SetDestination(point);
            }
        }


        //播放动画，判断是否到达了目的地，播放空闲或者跑步动画
        if (agent.remainingDistance == 0)
        {
            animation.Play("idle");
        }
        else
        {
            animation.Play("run");
        }
        
    }
}
```

### 例子二：上下斜坡
烘焙上下斜坡的问题 
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcwMTIxNTUy?x-oss-process=image/format,png)
在用Unity的自动寻路系统的时候，如果人物不能实现按照规定到达目的地，有绝大的原因是烘焙寻路出现了问题，所以这是我们首先需要重视的地方。 
下面就是一开始我烘焙的寻路，大家可能发现问题了，就是在两个红圈的位置是没有烘焙上的，并且区域很大，当人物寻路到这里的时候很容易卡在这里。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcwMTM2MDky?x-oss-process=image/format,png)

那就让我们来设置烘焙的参数吧。 
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcwMTQ0OTkx?x-oss-process=image/format,png)
**首先来介绍一个各个参数的含义：** 
Agent Radius：烘焙的半径，其数值越小则烘焙效果越好； 
Agent Height：烘焙的高度，人物通过的高度； 
Max Slope：烘焙的最大坡度，大于这个坡度的面将不会烘焙； 
Step Height：烘焙的台阶高度，如果高度差小于设置值，将视为连接。

**烘焙参数设置**

所以我们将烘焙半径调小点就可以解决这个问题了。 
我将烘焙半径设置为0.1，烘焙效果如下图，上坡和下边的地面连接处没有烘焙上的区域就很小啦。

烘焙好的效果图
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcwMjA2MTc0?x-oss-process=image/format,png)
**斜坡角度和连接问题** 
如果上坡的角度很大，人物也会卡在上坡中，我现在设置的上坡角度是40度。如果把角度设置为30度或者以下，人物就可以很顺利的爬上斜坡啦。

**上坡角度很大**
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcwMjM5MDc4?x-oss-process=image/format,png)
如果下坡的角度很大，人物就会直接跳下斜坡，我现在设置的下破的角度是50度。可以从图中看到人物是直接跳下来的。如果把角度设置为40度或者以下，人物就可以很顺利的下斜坡啦。

**下坡角度很大**
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcwMjQ2MzU2?x-oss-process=image/format,png)

还有就是斜坡与地面和站台连接处的问题，它们的连接之间一定不能有空隙，否则人物也容易卡在空隙处。如下图中，斜坡与站台没有完全连接上，有个很小的缝隙，即使寻路也烘焙得没有问题，人物有时候也会卡在这个地方。

**斜坡连接处处理**
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcwMjI2MTEy?x-oss-process=image/format,png)
**人物容易卡在寻路的边缘处** 

因为寻路就是解决的人物通过查找最短的路径（在忽略消耗体力值前提下），并最终达到目的地的问题，所以在上下坡也经常会遇到人物会沿着斜坡一边运动，这个就可能使人物卡在烘焙好的寻路边缘处。 
我的解决办法是设置中间目标物，让其绕开寻路边缘运动，这就需要设置几个中间目标，当人物到达一个目标的时候，然后向着下一个目标运动。 
从图中可以看出设置了三个目标物，这样人物就可以顺利到达目标3啦。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcwMzEwNDQ2?x-oss-process=image/format,png)


**代码：**

```csharp
[RequireComponent(typeof(NavMeshAgent))]
public class NavigationTest : MonoBehaviour {

    public Transform targetOne;
    public Transform targetTwo;
    public Transform targetThree;

    private NavMeshAgent navAgent;

    private float distanceOne;
    private float distanceTwo;
    // Use this for initialization
    void Start () {

        navAgent = transform.GetComponent<NavMeshAgent>();

        navAgent.SetDestination(targetOne.position);
    }

    // Update is called once per frame
    void Update () 
    {

        CheckReachTarget();
    }

    void CheckReachTarget()
    {

        distanceOne = Vector3.Distance(transform.position,targetOne.position);
        distanceTwo = Vector3.Distance(transform.position,targetTwo.position);
        if (distanceOne < 1f)
        {
            navAgent.SetDestination(targetTwo.position);
        }

        if (distanceTwo<1f)
        {
            navAgent.SetDestination(targetThree.position);
        }
    }
}

```

### 例子三：简单的自动寻路
1.在Scene中新建三个Cube，如下图摆放。

![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcwNjUwMDk1?x-oss-process=image/format,png)

2.选中上图三个Cube，并在Inspector面板中选中为静态(static)下拉选项的Navigation Static，如下图。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcwNjU4Njg1?x-oss-process=image/format,png)


3.依次选择菜单栏中的Windows - Navigation ，打开后面板如下。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcwNzA5NDgw?x-oss-process=image/format,png)


单击该面板右下角的Bake按钮，即可生成导航网格，下图为已生成的导航网格。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190929110459784.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
4.下面就可以让一个运动体根据一个导航网格运动到目标位置。

首先新建一个Cube为目标位置，起名TargetCube。然后创建一个capsule(胶囊)运动体，为该胶囊挂在一个Nav Mesh Agent(Component - Navigation - Nav Mesh Agent)；最后写一个脚本就可以实现自动寻路了。脚本如下：

```csharp
using UnityEngine;
using System.Collections;

public class Run : MonoBehaviour {

    public Transform TargetObject = null;
 
    void Start () {
        if (TargetObject != null)
        {
            GetComponent<NavMeshAgent>().destination = TargetObject.position;
        }
    } 
    void Update () {
    
    }
}
```

脚本新建完成后挂载到胶囊体上，然后将TargetCube赋予给胶囊体的Run脚本，运行场景，如下图，胶囊体会按照箭头的方向运动到Cube位置。

![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcwNzM0MDM1?x-oss-process=image/format,png)

这样一个简单的自动寻路就完成了，如果要更精细的寻路，或要实现上坡，钻"桥洞"等，可根据下面介绍的相关参数进行调节。

下面介绍Navigation组件和Nav Mesh Agent组件的相关参数。

**Navigation**

Object：物体参数面板
Navigation Static：勾选后表示该对象参与导航网格的烘培。
OffMeshLink Generation：勾选后可跳跃(Jump)导航网格和下落(Drop)。
Bake：烘培参数面板　　
Radius：具有代表性的物体半径，半径越小生成的网格面积越大。
Height：具有代表性的物体的高度。
Max Slope：斜坡的坡度。
Ste Height：台阶高度。
Drop Height：允许最大的下落距离。
Jump Distance：允许最大的跳跃距离。
Min Region Area：网格面积小于该值则不生成导航网格。
Height Mesh：勾选后会保存高度信息，同时会消耗一些性能和存储空间。
Nav Mesh Agent：导航组建参数面板　　　　

Radius：物体的半径
Speed：物体的行进最大速度
Acceleration：物体的行进加速度
Augular Speed：行进过程中转向时的角速度。
Stopping Distance：离目标距离还有多远时停止。
Auto Traverse Off Mesh Link：是否采用默认方式度过链接路径。
Auto Repath：在行进某些原因中断后是否重新开始寻路。
Height：物体的高度。
Base Offset：碰撞模型和实体模型之间的垂直偏移量。
Obstacle Avoidance Type：障碍躲避的的表现登记，None选项为不躲避障碍，另外等级越高，躲避效果越好，同时消耗的性能越多。
Avoidance Priority：躲避优先级。
NavMesh Walkable：该物体可以行进的网格层掩码。

### 例子四：Navigation实现高低落差以及跳跃的做法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190929110634156.gif)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190929110656702.gif)

 不管是爬楼梯，还是跳跃，NavMesh都是通过了OffMeshLink来做的。创建OffMeshLink的方法有两种，接下来会通过制作上面的例子来进行说明：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxMDM2NTQ4?x-oss-process=image/format,png)

为了做这个例子，我们预先在场景里面准备了一些物体：摄像机是必须的，一个作为地面的Plane，然后是F1——F5几个高低落差不一样的台阶，L1和L2是楼梯模型， 控制人物主体man，还有移动的目标点target。
其中man身上必须带有NavMesh Agent组件，为了观察方便在target身上带了light组件。
 
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxMjQ5ODc1?x-oss-process=image/format,png)
 按照上一节所讲的，plane和F1——F5台阶在Navigation面板勾选Navigation Static选项，然后Bake，观察Scene视窗，会发现已经生成了我们所要的NavMesh网格，现在我们可以像上一节那样在plane上面给人物做寻路和移动了，但人物是不会爬楼梯的。

 

这时候，我们找到L1楼梯，在楼梯的开始和结束的位置放置两个点，这两个点只需要拾取它的位移的，你可以用empty Gameobject来做，我这里为了便于观察，就拿了cube来做。开始点命名为startPoint，结束点命名为endPoint。

![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxMjM4MTYz?x-oss-process=image/format,png)
 注意：startPoint和endPoint的位置要稍微比所在的平面高一点点。

 

接下来介绍第一种生成OffMeshLink的方法。选择L1楼梯，然后在Component下拉选项中选择Navigation——Off Mesh Link。

![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxMjI2Mzk4?x-oss-process=image/format,png)
 

选择后，OffMeshLink组件已经添加到了L1的身上，我们可以在Inspector面板看到：


我们把刚才放置在场景里面的startPoint和endPoint指定到OffmeshLink组件的Start和End位置，其他选项默认不改变 

 ![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxMzEyNjU5?x-oss-process=image/format,png)

再次Bake


 ![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxMzQ2ODY4?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxNDA2NTMy?x-oss-process=image/format,png)

 现在我们发现，在scene面板里面，在startPoint和endPoint之间生成了一条线，而方向是从startPoint指向endPoint的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190929110747511.gif)


 这时候，你应该可以通过移动目标点让角色开始爬楼梯了。但爬上去之后角色暂时不能跳下来，如果把目标点移动到plane上，角色会顺着楼梯爬下来。

我们使用同样的方法对L2进行生成OffMeshLink。这时候，角色应该可以爬两层楼梯了。到此第一个目标完成了。

 

接下来我们进行第二个目标的制作，首先先来分析一下我们的场景：

![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxNDM3NTQ0?x-oss-process=image/format,png)
 我们希望人物能从2.5M的高度往下跳，超过2.5M人物就不能跳了，太高会有危险。然后横向我们希望人物能跳过2米的沟。

根据这个设定，我们的场景会是这样的情况：L1和L2只能通过爬楼梯，L2和L3之间可以跳跃，L3——L5是可以往下跳的。

 

于是，我们在Navigation面板里面找到Bake栏，Drop Height（掉落高度）填2.5，Jump Distance（跳跃距离）填2，单位都是米

![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxNDUwMjM4?x-oss-process=image/format,png)
 

接下来介绍第二种生成OffMeshLink的方法：

我们把L1——L5的物体选中，在Navigation面板的Object栏里面把OffMeshLink Generation选项打上勾

![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxNTA2NTk4?x-oss-process=image/format,png)
 

再次Bake，回到scene视窗：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxNTI3MjYx?x-oss-process=image/format,png)

这时候，场景里面会出现很多心的OffMeshLink，这是unity通过计算，把可以跳跃或者下落的地方自动生成了OffMeshLink了。

这时候，你应该已经可以通过移动目标点，让角色进行跳跃和下落了。 

 

进行到这里，我们的第二个目标也完成了。

 

不过有些朋友可能会提出疑问，在做的过程中，假如没有这个大兵的模型，而是用一个胶囊体来代替人物的话，它爬楼梯和跳跃的时候好像是在一瞬间完成的，没有大兵那个爬楼梯和跳跃动作的过程。如这样：

 ![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxNTQwODYw?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxNTUxMzQ4?x-oss-process=image/format,png)
的确是这样，因为默认的NaveMesh Agent组件上面是勾选了Auto Traverse Off Mesh Link（自动通过OffMeshLink）选项的。这样的意思是人物只要到了OffMeshLink的开始点，就会自动的移动到OffMeshLink的结束点。 

 ![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxNjA2MDcy?x-oss-process=image/format,png)

假如各位需要对越过OffMeshLink时候进行自己的控制，是需要另外写脚本的。我这里简单的介绍一下方法，有兴趣的朋友可以自己试试。

首先各位最好有用状态来控制角色的概念。比如人物可以分为站立、走路、跑步、上下楼梯、横向跳跃和往下掉落几种状态，针对NavMesh来说，人物简单的可以分为站立、正常的NavMesh寻路，和通过OffMeshLink移动几种状态。

先把 Auto Traverse Off Mesh Link选项取消。

然后，当人物在通过OffMeshLink移动的状态（可以用NavMeshAgent.isOnOffMeshLink来判断），获取到当前通过的OffMeshLink：

OffMeshLinkData  link = NavMeshAgent.currentOffMeshLinkData;

这样你就能获取到link的开始点和结束点的坐标（link.startPos和link.endPos），这时候你的人物就可以用最简单的Vector3.Lerp来进行移动，当人物的位移到达了结束点的坐标，人物的OffMeshLink移动状态就可以结束，又重新变回正常寻路或者站立的状态了。在这个Vector3.Lerp的过程中，你可以随意的控制人物的爬行或者跳跃的动作。

### 例子五：自动寻路Navmesh之跳跃,攀爬,斜坡
**步骤：**
1.在场景中摆放各种模型，包括地板，斜坡，山体，扶梯等
2.为所有的模型加上Navigation Static和OffMeshLink Generatic(这个根据需要，例如地板与斜坡相连，斜坡就不需要添加OffMeshLink)
3.特殊处理扶梯，需要手动添加Off Mesh Link，设置好开始点和结束点
4.保存场景，烘焙场景
5.添加角色模型，为其加Nav Mesh Agent组件
6.为角色添加一个新脚本，AgentLocomotion.cs,用来处理自动寻路，已经角色动画变换。代码比较长，大家可以结合注释来理解

**代码：**

```csharp
using UnityEngine;  
using System.Collections;  
  
public class AgentLocomotion : MonoBehaviour  
{  
    private Vector3 target;//目标位置  
    private NavMeshAgent agent;  
    private Animation anim;//动画  
    private string locoState = "Locomotion_Stand";  
    private Vector3 linkStart;//OffMeshLink的开始点  
    private Vector3 linkEnd;//OffMeshLink的结束点  
    private Quaternion linkRotate;//OffMeshLink的旋转  
    private bool begin;//是否开始寻路  
  
    // Use this for initialization  
    void Start()  
    {  
        agent = GetComponent<NavMeshAgent>();  
        //自动移动并关闭OffMeshLinks,即在两个隔离障碍物直接生成的OffMeshLink，agent不会自动越过  
        agent.autoTraverseOffMeshLink = false;  
        //创建动画  
        AnimationSetup();  
        //起一个协程，处理动画状态机  
        StartCoroutine(AnimationStateMachine());  
    }  
  
    void Update()  
    {  
        //鼠标左键点击  
        if (Input.GetMouseButtonDown(0))  
        {  
            //摄像机到点击位置的的射线  
            Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);  
            RaycastHit hit;  
            if (Physics.Raycast(ray, out hit))  
            {  
                //判断点击的是否地形  
                if (hit.collider.tag.Equals("Obstacle"))  
                {  
                    begin = true;  
                    //点击位置坐标  
                    target = hit.point;  
                }  
            }  
        }  
        //每一帧，设置目标点  
        if (begin)  
        {  
            agent.SetDestination(target);  
        }  
    }  
  
    IEnumerator AnimationStateMachine()  
    {  
        //根据locoState不同的状态来处理，调用相关的函数  
        while (Application.isPlaying)  
        {  
            yield return StartCoroutine(locoState);  
        }  
    }  
  
    //站立  
    IEnumerator Locomotion_Stand()  
    {  
        do  
        {  
            UpdateAnimationBlend();  
            yield return new WaitForSeconds(0);  
        } while (agent.remainingDistance == 0);  
        //未到达目标点，转到下一个状态Locomotion_Move  
        locoState = "Locomotion_Move";  
        yield return null;  
    }  
  
    IEnumerator Locomotion_Move()  
    {  
        do  
        {  
            UpdateAnimationBlend();  
            yield return new WaitForSeconds(0);  
            //角色处于OffMeshLink，根据不同的地点，选择不同动画  
            if (agent.isOnOffMeshLink)  
            {  
                locoState = SelectLinkAnimation();  
                return (true);  
            }  
        } while (agent.remainingDistance != 0);  
        //已经到达目标点，状态转为Stand  
        locoState = "Locomotion_Stand";  
        yield return null;  
    }  
  
    IEnumerator Locomotion_Jump()  
    {  
        //播放跳跃动画  
        string linkAnim = "RunJump";  
        Vector3 posStart = transform.position;  
  
        agent.Stop(true);  
        anim.CrossFade(linkAnim, 0.1f, PlayMode.StopAll);  
        transform.rotation = linkRotate;  
  
        do  
        {  
            //计算新的位置  
            float tlerp = anim[linkAnim].normalizedTime;  
            Vector3 newPos = Vector3.Lerp(posStart, linkEnd, tlerp);  
            newPos.y += 0.4f * Mathf.Sin(3.14159f * tlerp);  
            transform.position = newPos;  
  
            yield return new WaitForSeconds(0);  
        } while (anim[linkAnim].normalizedTime < 1);  
        //动画恢复到Idle  
        anim.Play("Idle");  
        agent.CompleteOffMeshLink();  
        agent.Resume();  
        //下一个状态为Stand  
        transform.position = linkEnd;  
        locoState = "Locomotion_Stand";  
        yield return null;  
    }  
    //梯子  
    IEnumerator Locomotion_Ladder()  
    {  
        //梯子的中心位置  
        Vector3 linkCenter = (linkStart + linkEnd) * 0.5f;  
        string linkAnim;  
        //判断是在梯子上还是梯子下  
        if (transform.position.y > linkCenter.y)  
            linkAnim = "Ladder Down";  
        else  
            linkAnim = "Ladder Up";  
  
        agent.Stop(true);  
  
        Quaternion startRot = transform.rotation;  
        Vector3 startPos = transform.position;  
        float blendTime = 0.2f;  
        float tblend = 0f;  
  
        //角色的位置插值变化（0.2内变化）  
        do  
        {  
            transform.position = Vector3.Lerp(startPos, linkStart, tblend / blendTime);  
            transform.rotation = Quaternion.Lerp(startRot, linkRotate, tblend / blendTime);  
  
            yield return new WaitForSeconds(0);  
            tblend += Time.deltaTime;  
        } while (tblend < blendTime);  
        //设置位置  
        transform.position = linkStart;  
        //播放动画  
        anim.CrossFade(linkAnim, 0.1f, PlayMode.StopAll);  
        agent.ActivateCurrentOffMeshLink(false);  
        //等待动画结束  
        do  
        {  
            yield return new WaitForSeconds(0);  
        } while (anim[linkAnim].normalizedTime < 1);  
        agent.ActivateCurrentOffMeshLink(true);  
        //恢复Idle状态  
        anim.Play("Idle");  
        transform.position = linkEnd;  
        agent.CompleteOffMeshLink();  
        agent.Resume();  
        //下一个状态Stand  
        locoState = "Locomotion_Stand";  
        yield return null;  
    }  
  
    private string SelectLinkAnimation()  
    {  
        //获得当前的OffMeshLink数据  
        OffMeshLinkData link = agent.currentOffMeshLinkData;  
        //计算角色当前是在link的开始点还是结束点（因为OffMeshLink是双向的）  
        float distS = (transform.position - link.startPos).magnitude;  
        float distE = (transform.position - link.endPos).magnitude;  
  
        if (distS < distE)  
        {  
            linkStart = link.startPos;  
            linkEnd = link.endPos;  
        }  
        else  
        {  
            linkStart = link.endPos;  
            linkEnd = link.startPos;  
        }  
        //OffMeshLink的方向  
        Vector3 alignDir = linkEnd - linkStart;  
        //忽略y轴  
        alignDir.y = 0;  
        //计算旋转角度  
        linkRotate = Quaternion.LookRotation(alignDir);  
  
        //判断OffMeshLink是手动的（楼梯）还是自动生成的（跳跃）  
        if (link.linkType == OffMeshLinkType.LinkTypeManual)  
        {  
            return ("Locomotion_Ladder");  
        }  
        else  
        {  
            return ("Locomotion_Jump");  
        }  
    }  
  
    private void AnimationSetup()  
    {  
        anim = GetComponent<Animation>();  
  
        // 把walk和run动画放到同一层，然后同步他们的速度。  
        anim["Walk"].layer = 1;  
        anim["Run"].layer = 1;  
        anim.SyncLayer(1);  
  
        //设置“跳跃”，“爬楼梯”，“下楼梯”的动画模式和速度  
        anim["RunJump"].wrapMode = WrapMode.ClampForever;  
        anim["RunJump"].speed = 2;  
        anim["Ladder Up"].wrapMode = WrapMode.ClampForever;  
        anim["Ladder Up"].speed = 2;  
        anim["Ladder Down"].wrapMode = WrapMode.ClampForever;  
        anim["Ladder Down"].speed = 2;  
  
        //初始化动画状态为Idle  
        anim.CrossFade("Idle", 0.1f, PlayMode.StopAll);  
    }  
    //更新动画融合  
    private void UpdateAnimationBlend()  
    {  
        //行走速度  
        float walkAnimationSpeed = 1.5f;  
        //奔跑速度  
        float runAnimationSpeed = 4.0f;  
        //速度阀值（idle和walk的临界点）  
        float speedThreshold = 0.1f;  
  
        //速度，只考虑x和z  
        Vector3 velocityXZ = new Vector3(agent.velocity.x, 0.0f, agent.velocity.z);  
        //速度值  
        float speed = velocityXZ.magnitude;  
        //设置Run动画的速度  
        anim["Run"].speed = speed / runAnimationSpeed;  
        //设置Walk动画的速度  
        anim["Walk"].speed = speed / walkAnimationSpeed;  
  
        //根据agent的速度大小，确定animation的播放状态  
        if (speed > (walkAnimationSpeed + runAnimationSpeed) / 2)  
        {  
            anim.CrossFade("Run");  
        }  
        else if (speed > speedThreshold)  
        {  
            anim.CrossFade("Walk");  
        }  
        else  
        {  
            anim.CrossFade("Idle", 0.1f, PlayMode.StopAll);  
        }  
    }  
}  
```

**效果图：**

点击任何一个地点，角色都可以自动寻路过去。中间可能经过不同的障碍物，我们可以看到角色如我们所预料的一样，可以跳跃下来，可以爬楼梯，最终到达目标点。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxNzE5MDgx?x-oss-process=image/format,png)
**源码：**
https://pan.baidu.com/s/1tSzaGhKPj8m0AtC8pkwbPQ

### 例子六：自动寻路Navmesh之高级主题
**隔离层自动生成寻路网格**
1.创建Plane实例P1,P2,两者之间出现一条鸿沟。直接控制角色位移是无法通过的。
2.打开Navigation窗口，分别选中P1，P2,分别设置Navigation Static 和OffMeshLink Generatic
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxODQ0ODIw?x-oss-process=image/format,png)
3.保存场景，点击场景烘焙按钮Bake。结束后我们可以看到P1，P2除了自身生产寻路网格外，它们直接还生成了连接纽带。
4.添加角色模型Solder，为其添加NavMeshAgent(Component->Navigation->NavMeshAgent)
5.给Solder添加PlayerController脚本

**代码：**

```csharp
using UnityEngine;  
using System.Collections;  
  
public class PlayerController : MonoBehaviour  
{  
    private NavMeshAgent agent;  
  
    public bool setAgentWalkMask;//是否需要动态修改寻路层，在scene4的实例中要用到  
  
    void Start()  
    {  
        //获取寻路组件  
        agent = GetComponent<NavMeshAgent>();  
    }  
  
    void Update()  
    {  
        //鼠标左键点击  
        if (Input.GetMouseButtonDown(0))  
        {  
            //摄像机到点击位置的的射线  
            Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);  
            RaycastHit hit;  
            if (Physics.Raycast(ray, out hit))  
            {  
                //判断点击的是否地形  
                if (!hit.collider.tag.Equals("Plane"))  
                {  
                    return;  
                }  
                //点击位置坐标  
                Vector3 point = hit.point;  
                //转向  
                transform.LookAt(new Vector3(point.x, transform.position.y, point.z));  
                //设置寻路的目标点  
                agent.SetDestination(point);  
            }  
        }  
  
        //播放动画  
        if (agent.remainingDistance == 0)  
        {  
            animation.Play("Idle");  
        }  
        else  
        {  
            animation.Play("Run");  
        }  
          
    }  
}  
```

5.点击任意的位置，可以看到角色都能自动寻路过去
效果图
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcxOTI0MTAx?x-oss-process=image/format,png)
**手动指定寻路网格方向**
1.将P1，P2的OffMeshLink Generatic去除
2.在P1上新建一个空的GameObject Start,P2上新建一个空的GameObject End
3.选中start，为它添加Off Mesh Link组件 Component->Navigation->OffMeshLink
4.设置Off Mesh Link组件的属性，Start Point 为 start，End Point为end
5.烘焙场景。我们可以看到有一条纽带从start指向end
点击地图，可以看到角色如果要跨越P1和P2,一定是沿着我们手动创建的路径
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcyMDA4NTk0?x-oss-process=image/format,png)
**导航网格障碍物 Navmesh Obstacle**
之前我们都是用固定的物体作为障碍物，然后烘焙场景。Unity还提供了动态的障碍物。任何一个GameObject都可以添加Navmesh Obstacle组件，变成一个障碍物。具体步骤是Component->Navigation->Navmesh Obstacle.它有两个属性：半径和高度，可以设置跟你的物品差不多的体积大小。

**寻路网格层的应用**
1.新建P1，P2，P3，P4等4个Plane，具体摆设形状见效果图
2.在Navigation窗口中，添加两个层Layers：Blue层和Red层
3.P1，P2的Navigation Layer设置为Default，P4的Navigation层设置为Red，P3设置为Blue
4.添加两个角色，设置他们的NavMeshAgent寻路层（NavMesh Walkable）。一个将Red层去掉，一个将Blue层去掉
5.点击P2的坐标，可以看到他们沿着不同的路径去目标点，一个走上层路线，一个走下层路线了。
效果图
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcyMDU3NjEy?x-oss-process=image/format,png)
**动态改变寻路网格层**
1.在scene3.unity基础上做一下修改。只保留一个角色
2.新增两个按钮，“走上层”和“走下层”，在游戏运行时，可以改变Agent的寻路层。

**代码：**
```csharp
//动态设置寻路路径层  
 void OnGUI()  
   {  
       if (!setAgentWalkMask)  
       {  
           return;  
       }  
       if (GUI.Button(new Rect(0, 0, 100, 50), "走下层"))  
       {  
           agent.walkableMask = 65;  
       }  
  
       if (GUI.Button(new Rect(0, 100, 100, 50), "走上层"))  
       {  
           agent.walkableMask = 129;  
       }  
   }  
```

   3.重新点击寻路，可以看到，选择不同的寻路层，角色的寻路路径也不同
   ![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcyMjA3OTE1?x-oss-process=image/format,png)
   看到代码中的agent.walkableMask = 65和129，大家会比较迷惑，其实寻路层每一层都是2的幂，见下图
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcyMjIwOTc0?x-oss-process=image/format,png)
所以上层的mask = Default(1)+Blue(128) = 129,下层的mak = Default(1)+Red(64) = 65

### 例子七：Navigation烘焙
**Building a NavMesh**
在Unity中，NavMesh  的生成操作需要Navigation窗口（在Window> Navigation）

在你的场景中构建NavMesh只需要4个步骤：

![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcyNTA2MDE0?x-oss-process=image/format,png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190929111018742.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**选择场景中需要生成寻路的几何体-可行走表面和障碍物。**
在NavMesh面板中选择需要烘焙寻路的物体，检测是否勾选Navigation Static.
根据你的agent大小来调整bake 面板的设置。
Agent Radius : agent可以距离墙体 ，窗户或边缘多近的距离。
Agent Height : agent可以通过的最低的空间高度。
Max Slope : agent可以直接行走上去的最小坡度。
Step Height:  agent可以踩上（走上）的障碍物最高高度。
点击bake按钮烘焙NavMesh。
当你的Navigation窗口打开并且可见时，烘焙的NavMesh结果在场景中会以蓝色的覆盖层在物体的几何体表面显示。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcyNTUzMDc3?x-oss-process=image/format,png)

烘焙完成后，您将在与NavMesh所属场景同名的文件夹中找到一个NavMesh资产文件。例如，如果在Assets文件夹中有一个名为First Level的场景，NavMesh将位于 Assets > First Level > NavMesh.asset.当烘焙结束后，你可以找到一个名字和你的场景名一样的文件夹，里面有一个NavMesh的资源文件，是属于这个场景的NavMesh。

让物体可烘焙的其他方法
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcyNjAxMDI3?x-oss-process=image/format,png)

直接选择物体在Inspector面板顶部的Static 菜单，你可以直接选择Navigation Static，而不用再打开Navigation 窗口。


**NavMesh烘焙的高级设置**
最小区域面积
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcyNjI0NzA3?x-oss-process=image/format,png)
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcyNjMzNDU3?x-oss-process=image/format,png)

**Min Region Area** 允许你剔除掉小的非连接NavMesh区域，当NavMesh区域小于指定值时将被剔除。

请注意：有些区域可能无法被移除，尽管有Min Region Area 的设置，原因是NavMesh的构建是一个个的网格并行构建。当一个区域横跨两个网格将不会被移除，因为区域修剪过程中无法获取到周围的网格。

**Voxel Size** 立体像素尺寸

**Manual voxel size** ：允许你改变烘焙操作过程中的精确性。

Navigation在构建寻路网格过程中，第一遍会把场景光栅化为体素，然后提取可行走区域，最后可行走区域会烘焙成网格。因此体素尺寸Voxel Size的决定了寻路网格烘焙的精准性。

提示：默认的体素设置最好的权衡了准确性和烘焙速度。在烘焙场景寻路的过程中，体素的增加会造成4x倍的内存消耗和4x倍的时间消耗。因此通常，你不需要自己去设置Voxel Size。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcyNzIxMjYy?x-oss-process=image/format,png)


**Smaller Agent Radius**

系统烘焙寻路是也会减小voxel size。如果你的agent尺寸保持不变，可能不需要增加NavMesh的构建分辨率。

更简单的方法如下所示：

设置你的半径为真实agent半径
打开Manual Voxel Size,这会保持当前的voxel的大小并且冻结它。
人为的将你的Agent Radius设置小，因为你已经勾选了Manual Voxel Size ,voxel size将不会改变。
Height mesh ： 允许你为角色提供更精准的位置。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTcxMDIxMTcyNzI5OTIx?x-oss-process=image/format,png)

导航时，NavMesh代理被约束在NavMesh的表面。由于NavMesh是可步行空间的近似，所以在构建NavMesh时，一些特性会被平均化。例如，楼梯可能在NavMesh中显示为一个斜坡。如果你的游戏需要准确的位置代理，你应该启用高度网格建设时，你烤NavMesh。该设置可以在导航窗口的高级设置中找到。注意，建筑高度网格将占用内存和处理在运行时，它将需要更长的时间来烘烤NavMesh。

尽管普通的NavMesh 已经可以很好的运行，但是由于NavMesh只是一个近似的可行走的空间，只保持了一些均衡的特性，如果你的游戏agent需要更精准的行走位置，你可以启用高度网格Height Mesh,

注意：高度网格Height Mesh将占用你“运行时”的内存和cpu，并且需要更多的烘焙时间。


**结束。。。**

这篇文章吸取了很多人的精华，有什么不对的地方希望都可以私信我。

参考文章：
Unity3D深入浅出 - 导航网格自动寻路(Navigation Mesh)
http://blog.csdn.net/yuxikuo_1/article/details/44974739
unity3D——自带寻路Navmesh入门教程（二）
http://www.cnblogs.com/wangweixznu/p/5443071.html
Unity3D自动寻路系统Navigation（三）之人物上下斜坡设置
http://blog.csdn.net/qq_27678295/article/details/53730252
Unity手游之路<八>自动寻路Navmesh之入门
http://blog.csdn.net/janeky/article/details/17457533
NavMesh Agent
https://docs.unity3d.com/Manual/class-NavMeshAgent.html
Unity 之Navigation烘焙
http://blog.csdn.net/yupu56/article/details/54932049


![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X2dpZi8xaFJlSGFxYWZhZXJPZDQ2M3BydGYxQWJibXFBMXhKSzUwUmN0MEZ4ZWFLSjJ6djBxc1lPUGF5RzZnMllLbnpMUTVnbUkxbnQ5UFVQaWI2cUtPQjVnOFEvNjQw?x-oss-process=image/format,png)
