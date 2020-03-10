---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《Flappy Bird》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
在本教程，我们将带领大家开发一个Flappy Bird的游戏。Flappy Bird Game于2013年发布，并于2014年1月成为当月下载最多的IOS应用程序。

游戏设计非常简单：一只鸟在障碍物之间水平飞行，玩家可以按下一个按钮让鸟飞起来，之后鸟由于重力原因往下坠落，就这样躲避障碍物，获得更高分数。

## 二、版本
==Unity5.0.0f4==

## 三、正文
### 1.调整摄像机
我们在Hierarchy面板中找到主摄像机Main Camera,然后我们可以设置背景色变成浅蓝色(r=198，G=208，B=230)对于天空的颜色，并调整大小如下图所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091815354151.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 2.背景设置
我们将首先在我们选择的绘图工具中绘制一个非常简单的天空背景：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1mbGFwcHktYmlyZC1nYW1lL2JhY2tncm91bmQucG5n?x-oss-process=image/format,png)
*注意：右击图像，选择另存为，导入到项目中，将图片保存到Sprites文件夹中。

保存后，我们可以在项目区：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918153903726.png)
然后在Inspector面板修改导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918153926319.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注：Pixels Per Unit值设成16这意味着16x16像素将适合在游戏世界的一个单位。我们将使用这个值的所有纹理，因为鸟将是16x16像素，这应该是一个单位在游戏世界。


好的，让我们将鸟的图片从项目区拖入到场景中，并作为Main Camera的一个子节点：
*注：因为我们主摄像机要一直移动中，我们想让背景也跟着主摄像机移动
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918154025756.png)：
这样，背景就成了主摄像机的子节点：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091815405050.png)

现在背景background是相机的一个孩子，它将永远走到相机的地方。换句话说，玩家总是会看到背景background。
*注意：我们也可以把几个背景放在一起，这样当相机水平移动时，仍然有一个背景，但是让它成为相机的一个子背景要容易得多。

我们将background的位置设置成0,-1,10，这样它就在其他游戏对象的最下面了：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918154357182.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
如果我们按下Play然后我们就可以看到背景天空：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918154420211.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

这里还有一项调整要做。我们很快就会增加鸟类和一些障碍，所以让我们也确保背景是真正的在所有的对象后面。Unity使用SpriteRenderer的分选层和层序属性来决定游戏的哪些部分应该在其他部分的前面。

我们将简单地设置层序到-1这样其他的一切都会被画在它面前：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918154504568.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注意：Order In Layer数值越高，就越靠前。Order In Layer数值越低，它在背景中的位置就越往后。

### 3.地面设置
我们来画一些地面的地形。我们会把它弄得很宽，以便以后有足够的空间来设置障碍:
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1mbGFwcHktYmlyZC1nYW1lL2dyb3VuZC5wbmc?x-oss-process=image/format,png)
*注意：右击图像，选择另存为，导入到项目中，将图片保存到Sprites文件夹中。

让我们在项目区找到ground图片，然后修改导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091815472775.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
然后，将它拖入到场景中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918154745658.png)
*注意：这一次我们不会让它成为相机的孩子。

让我们把地面定位在X=16 Y=-6，使其位于背景的上面，并使大部分区域位于屏幕下侧：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918154920272.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
这一次我们将选择一个Order In Layer值为1，这样它就永远会在鸟儿面前和后面的障碍面前：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918154945986.png)
**地面物理**
地面应该是物理世界的一部分。现在，它真的只是游戏世界中的一幅图像，只是一种视觉效果，仅此而已。我们希望地面像一堵墙，鸟儿可以与之相撞，所以让我们在Inspector面板选择**Add Component->Physics 2D->Box Collider 2D**:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918155047798.png)
正常情况下，我们现在就做完了，但是这里还有一个调整要做。
稍后，我们将为我们的游戏增加障碍。(就像原来的Flappy游戏中的绿色管道)这些障碍将向上和向下移动到地面。障碍和地面都是物理世界的一部分，根据物理定律，在同一个地方永远不可能有两个物体。(或在我们的例子中，两个碰撞器).

有几种方法可以解决这个问题。和往常一样，我们将通过创建一个新的物理方法来选择最简单的方法。层我们将用它作为地面和障碍。之后，我们将告诉Unity，简单地忽略该层之间的碰撞。

我们可以通过在Inspector选择加层:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918155257158.png)
之后，我们将添加一个自定义层，让我们起名叫做WeirdPhysics：
![Weird Physics Layer](https://img-blog.csdnimg.cn/20190918155325898.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在我们可以在Hierarchy然后给地面再分配WeirdPhysics层：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918155415688.png)
之后我们选择**Edit->Project Settings->Physics 2D**从顶部菜单中禁用WeirdPhysics 碰撞层碰撞矩阵:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918155529527.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注意：这么做在Unity中是非常少见的，但是我们的Flappy Bird游戏是一个例外。

现在地面将永远不会与任何障碍相撞。

如果我们按下Play然后我们就可以看到天空和地面了：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918155659615.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 4.鸟设置
**鸟的图片**
好吧，让我们谈谈我们游戏中最重要的部分：鸟。首先，我们将画一个4帧的鸟类飞行动画：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1mbGFwcHktYmlyZC1nYW1lL2JpcmQucG5n?x-oss-process=image/format,png)
*注意：右击图像，选择另存为，导入到项目中，将图片保存到Sprites文件夹中。

导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918155820769.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们的鸟类图片包含了几个较小的图像，因此我们选择SpriteMode：Multiple，这样的话就可以将图片切开来，然后我们单击Sprite Editor:
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091815591371.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
在Sprite Editor我们将图片切成4块，Pixel Size设成16*16网格：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918160016846.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
在按下Slice按钮，我们可以关闭Sprite Editor.。UNity会问我们是否想应用未导入设置，所以让我们选择应用.

现在，我们可以在项目区中看到我们鸟的4个切片图片：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918160131723.png)
**鸟类动画**
让我们选择所有的鸟的切片图片，拖入到场景中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918160200832.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
Unity知道我们想要从这些切片创建一个动画，这就是为什么它问我们在哪里保存动画文件。我们将创造一个新的BirdAnimation文件夹，然后将动画保存为fly.anim.

之后，我们可以在我们的BirdAnimation文件夹：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918160238370.png)
这个bird_0文件是处理动画状态和速度的状态机。第二个文件是fly动画本身。让我们双击bird_0文件，这样我们就可以看到动画状态机：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918160344685.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注意：我们不必担心动画状态，因为我们只有一个动画。

我们将单击fly状态，然后在Inspector减少速度到0.5:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918160442728.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
因为我们只有一个动画，我们已经在这里完成了。如果我们按下Play我们甚至可以看到它的作用：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1mbGFwcHktYmlyZC1nYW1lL2JpcmRfYW5pbWF0ZWQuZ2lm)
**鸟类物理学**
我们的鸟应该是物理世界的一部分。让我们先给它一个碰撞器，就像我们在地面上做的一样。我们会在Inspector选择
添加组件->物理二维->Circle Collider 2D:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918160559783.png)
在物理世界里，所有应该移动的东西也需要一个刚体..刚体负责像重力、速度和运动力这样的事情。
我们可以通过在Inspector
选择**添加组件->物理二维->Rigidbody 2D**..
我们还将启用Fixed Angle属性，以便该鸟不会突然开始旋转：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918160659892.png)
如果我们按下Play然后我们就可以看到刚体的重力特性：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1mbGFwcHktYmlyZC1nYW1lL2JpcmRfZ3Jhdml0eS5naWY)
**鸟的运动脚本Bird.cs**
我们的鸟看起来已经相当不错，但它也应该在任何时候向右飞，如果用户按下按钮，它应该拍打翅膀向上飞。

这种行为可以用脚本实现。让我们在**Inspector**选择**Add Component->New Script**，将名字命令为Brid，并保存到我们的Scripts文件夹中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918160944535.png)
我们可以双击脚本以便打开它：

```csharp
using UnityEngine;
using System.Collections;

public class Bird : MonoBehaviour {

    // Use this for initialization
    void Start () {
    
    }
    
    // Update is called once per frame
    void Update () {
    
    }
}
```
我们可以让这只鸟在任何时候都向右飞，首先增加一个新的速度变量，然后使用刚体的velocity属性：

```csharp
using UnityEngine;
using System.Collections;

public class Bird : MonoBehaviour {
    // Movement speed
    public float speed = 2;

    // Use this for initialization
    void Start () {    
        // Fly towards the right
        GetComponent<Rigidbody2D>().velocity = Vector2.right * speed;
    }
    
    // Update is called once per frame
    void Update () {
    
    }
}
```

*注：速度正好是运动方向乘以运动速度。
如果我们保存脚本并按下Play然后我们就可以看到这只鸟是如何向屏幕右边飞去的。

现在我们创造了一个新的force变量，然后使用更新函数来检查按键。如果用户按下Space然后我们就能让小鸟飞起来force:

```csharp
using UnityEngine;
using System.Collections;

public class Bird : MonoBehaviour {
    // Movement speed
    public float speed = 2;
    
    // Flap force
    public float force = 300;

    // Use this for initialization
    void Start () {    
        // Fly towards the right
        GetComponent<Rigidbody2D>().velocity = Vector2.right * speed;
    }
    
    // Update is called once per frame
    void Update () {
        // Flap
        if (Input.GetKeyDown(KeyCode.Space))
            GetComponent<Rigidbody2D>().AddForce(Vector2.up * force);
    }
}
```
如果我们按下Play然后我们可以让这只鸟向上飞：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1mbGFwcHktYmlyZC1nYW1lL2JpcmRfZmxhcHBpbmcuZ2lm)
这里还有最后一件事要做。当鸟与地面或障碍物相撞时，我们想重新开始游戏。我们的鸟已经有了碰撞器和一个刚体，这意味着Unity会自动调用OnCollisionEnter2D功能。我们所要做的就是将它实际添加到我们的脚本中：

```csharp
void OnCollisionEnter2D(Collision2D coll) {
    // Restart
    Application.LoadLevel(Application.loadedLevel);
}
```
*注：Application.LoadLevel可以用来加载场景。Application.loadedLevel是当前加载的场景。换句话说，我们只需重新启动当前的场景。

### 5.摄像机跟随
现在摄像机从来不动。因为这只鸟总是飞向屏幕的右边，我们很长一段时间都不能看到它。我们将通过创建一个新脚本来解决这个问题，这个脚本可以让相机始终跟踪鸟。

让我们添加新的脚本CaeraFollow.cs:

```csharp
using UnityEngine;
using System.Collections;

public class CameraFollow : MonoBehaviour {

    // Use this for initialization
    void Start () {
    
    }
    
    // Update is called once per frame
    void Update () {
    
    }
}
```
我们不需要启动函数，所以让我们删除它：

```csharp
using UnityEngine;
using System.Collections;

public class CameraFollow : MonoBehaviour {
    
    // Update is called once per frame
    void Update () {
    
    }
}
```

让我们增加一个public Transform 变量指定要跟随的目标：

```csharp
using UnityEngine;
using System.Collections;

public class CameraFollow : MonoBehaviour {
    // The Target
    public Transform target;
    
    // Update is called once per frame
    void Update () {
    
    }
}
```
*注意：这样我们就可以在后面拖入跟随目标

之后，我们就在Update里面，使当前对象始终跟随目标点的x的值：
```csharp
// Update is called once per frame
void Update () {
    transform.position = new Vector3(target.position.x,
                                     transform.position.y,
                                     transform.position.z);
}
```
*注：之所以只改变x的值，因为改变x的值就是水平移动了。

现在我们的脚本完成了。然而，通常认为最好的做法是在场景中的其他内容更新之后进行摄像机运动。我们只需要把它改到我们的LateUpdate函数里面，只是为了让它完全平滑：

```csharp
void LateUpdate () {
    transform.position = new Vector3(target.position.x,
                                     transform.position.y,
                                     transform.position.z);
}
```
*注意：LateUpdate 函数是在其他内容都更新之后才更新的函数，通常是用来移动相机，移动对象使用，我们将移动摄像机的脚本改到LateUpdate 函数里面，就可以把Update里面的脚本删除了，这样会更加平滑


然后让我们保存脚本，将我们的bird_0对象拖入到CameraFollow脚本的Target插槽中：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1mbGFwcHktYmlyZC1nYW1lL2NhbWVyYV9mb2xsb3dpbmdfYmlyZC5naWY)
### 6.障碍物设置
**障碍图像**
现在我们的游戏不是很难，我们可以通过增加一些障碍来改变这种状况，让我们画一个：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1mbGFwcHktYmlyZC1nYW1lL29ic3RhY2xlLnBuZw?x-oss-process=image/format,png)
*注意：右击图像，选择另存为，导入到项目中，将图片保存到Sprites文件夹中。

我们修改导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918162520334.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

我们将障碍物拖入到场景中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918162540681.png)
我们会把它定位在X=3, Y=-5:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918162551327.png)

下面是场景中的场景：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918162602915.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**障碍物理**
这个障碍应该再次成为物理世界的一部分。这只鸟应该能和它相撞，所以让我们选择在Inspector添加组件->物理二维->Box Collider 2D:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918162642862.png)
我们讨论了障碍是如何在地下形成的，我们不想让两者相互碰撞，所以让我们把它作为我们的一部分WeirdPhysics图层：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918162703395.png)
*注：由于我们禁止了在奇怪的物理层之间的碰撞，地面永远不会与障碍物相撞。鸟仍然可以与地面和障碍物碰撞，因为它有一个不同的层(默认层)。

好吧，所以有些障碍也应该是上下移动的。物理世界中所有应该移动的东西都需要一个刚体，所以让我们在Inspector选择添加组件->物理二维->刚体2D。我们不希望它受到重力的影响，所以我们设置Gravity Scale到0..我们也不希望它旋转，所以我们也要再次启用Fixed Angle：
![在这里插入图片描述](https://img-blog.csdnimg.cn/201909181628107.png)
如果我们按下Play让鸟飞向障碍物，然后我们就可以看到水平是如何重新启动的：

**障碍物脚本**
好的，所以有些障碍应该是上下移动的。这种行为可以再次用脚本实现。让我们添加新脚本Obstacle.cs:

```csharp
using UnityEngine;
using System.Collections;

public class Obstacle : MonoBehaviour {

    // Use this for initialization
    void Start () {
    
    }
    
    // Update is called once per frame
    void Update () {
    
    }
}
```
有许多不同的方法可以使障碍一直上下移动。和往常一样，我们将使用最简单的方法。

我们将首先添加一个速度变量，然后设置刚体的velocity使障碍物向上移动速度:

```csharp
using UnityEngine;
using System.Collections;

public class Obstacle : MonoBehaviour {
    // Movement Speed (0 means don't move)
    public float speed = 0;

    // Use this for initialization
    void Start () {
        // Initial Movement Direction
        GetComponent<Rigidbody2D>().velocity = Vector2.up * speed;    
    }
    
    // Update is called once per frame
    void Update () {
    
    }
}
```
现在的诀窍是使用UNity的InvokeRepeting函数每隔几秒钟反转一次该速度：

```csharp
using UnityEngine;
using System.Collections;

public class Obstacle : MonoBehaviour {
    // Movement Speed (0 means don't move)
    public float speed = 0;
    
    // Switch Movement Direction every x seconds
    public float switchTime = 2;

    void Start() {
        // Initial Movement Direction
        GetComponent<Rigidbody2D>().velocity = Vector2.up * speed;
        
        // Switch every few seconds
        InvokeRepeating("Switch", 0, switchTime);
    }
    
    void Switch() {
        GetComponent<Rigidbody2D>().velocity *= -1;
    }
}
```
*注意：Switch函数只是逆转刚体的速度。然后我们用InvokeRepeting每隔几秒钟就调用一次这个函数。我们还添加了一个切换时间变量指定的时间Switch Time。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091816311310.png)

让我们保存脚本并设置障碍速度到1:

*注意：如果我们不想有障碍移动，那么我们可以设置速度到0或者禁用脚本。

如果我们按下Play然后我们可以看到我们的障碍向上和向下移动：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1mbGFwcHktYmlyZC1nYW1lL29ic3RhY2xlX21vdmluZy5naWY)
**增加更多的障碍**
让我们在场景中，选中障碍物，然后选择复制，将新物体往右移动一点：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918163233895.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

我们还将复制一个，然后设置Scale.Y=-1:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918163258319.png)
这样，当将它倒置时，它看起来是正确的：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918163310399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们可以用同样多的不同来增加同样多的障碍速度和切换时间我们想要的属性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190918163324610.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 6.总结
我们刚刚了解到在Unity中制作一款Flappy Bird游戏是多么的容易。我们使用了一些Unity中非常强大的特性，比如物理碰撞，Mecanim（动画）、图层、Sprite Editor和脚本。和往常一样，现在该由大家来让游戏变得更加有趣和困难了。
