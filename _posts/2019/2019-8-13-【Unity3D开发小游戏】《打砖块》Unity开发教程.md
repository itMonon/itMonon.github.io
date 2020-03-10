---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《打砖块》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
最初的《打砖块》是在1986年发行的，虽然已经这么长时间了，但是仍然非常容易上瘾，由于它的街机风格的性质，它很容易在Unity中开发。

像往常一样，每件事件都会尽可能简单地解释，以便每个人都能理解。

下面是游戏的预览:
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2Fya2Fub2lkX3ByZXZpZXcucG5n)

## 二、版本
项目的版本为：Unity 2018.3.14f1
更新的版本也可以很好的使用了，如果使用旧版本的话可能出现一些API无法使用的错误，为了获得更好的结果，请也使用Unity 2018.3.14f1版本

## 三、教程

### 1、相机设置
首先我们找到场景中的主摄像机 Main Camera，然后将背景颜色调整为深色，并修改大小如下午所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLWNhbWVyYXNldHVwLnBuZw)

### 2、背景设置
注意：右击图像，选择另存为，找到项目的资产文件夹并将其保存到新的Sprite文件夹。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2hleGFnb25fcGF0dGVybi5wbmc?x-oss-process=image/format,png)

让我们在项目区：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLWltcG9ydGVkYmdzcHJpdGUucG5n)
将这个图片铺满整个屏幕

### 3、设置分层

我们正在制作一个2D游戏，在一些情况下，有几个图像是画在对方之上，例如，当球和背景的图案都是画出来的。

让我们告诉联合，我们的背景图案应该在后台，以确保球总是画在上面(因此可见).

我们可以用Sorting Layer，更改背景图片的排序层

让我们选择添加排序层.。从Sorting Layer列表，然后添加Background层，并将其移动到第一个位置，如下所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLWNyZWF0ZXNvcnRpbmdsYXllci5wbmc)

让我们找到背景图片的Sprite Renderer组件,，并分配我们的新Background排序层：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLWJnaW5zcGVjdG9yb25iZ2xheWVyLnBuZw)

### 4、添加边界

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2JvcmRlcl9sZWZ0LnBuZw?x-oss-process=image/format,png)![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2JvcmRlcl90b3AucG5n?x-oss-process=image/format,png)![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2JvcmRlcl9yaWdodC5wbmc?x-oss-process=image/format,png)
选择图片另存为，把资源导入到项目中 Sprite文件夹中

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLXdhbGxpbXBvcnRpbmcucG5n)
然后把这些边界摆放到场景中：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2JvcmRlcnNfcG9zaXRpb25lZC5wbmc)
添加边界的碰撞：
选中边界：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLXdhbGxpbmhlaXJhY2h5LnBuZw)
添加碰撞器:
Add Component -> Physics 2D -> Box Collider 2D
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLXdhbGx3aXRoY29sbGlkZXJzLnBuZw)

### 5、添加球拍

球拍图像
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3JhY2tldC5wbmc?x-oss-process=image/format,png)
注意：选择图片，另存图片，选择另存为，将其保存到项目中的Sprite文件夹中

将球拍图片放置到场景中，调整位置，X0，Y-95，Z0.
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3JhY2tldF9pbmdhbWUucG5n)
球拍的碰撞器：
选中球拍，添加碰撞器
Add Component -> Physics 2D -> Box Collider 2D
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLXJhY2tldGJveGNvbGxpZGVyLnBuZw)
选中球拍，添加刚体
Add Component -> Physics 2D -> Rigidbody 2D
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLXJhY2tldHJpZ2lkYm9keTJkLnBuZw)
Gravity Scale：0（防止球拍出界）
Collision Detection:Contonuous 连续（防止因物理二维引擎bug而使球穿过球拍的小故障）
在Z轴上冻结旋转：这使球拍冻结了角度。

### 6、球拍移动
我们添加一个Racket.cs脚本：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLWFkZHJhY2tldHNjcmlwdC5wbmc)
双击脚本来打开它：

```csharp
using UnityEngine;
using System.Collections;

public class Racket : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
我们的Start函数在开始游戏时由统一自动调用。我们的Update函数一次又一次地自动调用，大致与默认设置为60的游戏帧速率相关联。

还有另一种类型的Update函数，它被称为FixedUpdate，它也被一次又一次的调用，但是在一个固定的时间间隔内。单位物理是在完全相同的时间间隔内计算的，所以使用它总是一个好主意FixedUpdate在做物理相关的计算时，因为我们的球拍是一个物理物体(因为它有一个刚体)。

我们可以移除Start和Update函数并创建一个FixedUpdate函数：

```csharp
using UnityEngine;
using System.Collections;

public class Racket : MonoBehaviour {

    void FixedUpdate () {

    }
}
```
我们可以使用刚体组件Rigidbody的velocity属性控制球拍移动
移动的原理：Speed * 运动方向 
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3ZlY3RvcjJfZGlyZWN0aW9ucy5wbmc)
让我们先添加一个速度变量到我们的脚本中：

```csharp
using UnityEngine;
using System.Collections;

public class Racket : MonoBehaviour {
    // Movement Speed
    public float speed = 150;

    void FixedUpdate () {

    }
}
```
玩家可以使用键盘上的A/D控制左右移动，检查玩家输入的函数可以使用Unity自带的GetAxisRaw函数来获取水平输入，它会返回来一个-1 到1之间的值，-1为左边，1为右边

```csharp
void FixedUpdate () {
    // Get Horizontal Input
    float h = Input.GetAxisRaw("Horizontal");
}
```

现在我们可以设置运动方向乘以速度：

```csharp
void FixedUpdate () {
    // Get Horizontal Input
    float h = Input.GetAxisRaw("Horizontal");

    // Set Velocity (movement direction * speed)
    GetComponent<Rigidbody2D>().velocity = Vector2.right * h * speed;
}
```
将Racket脚本，附加到球拍物体上面。
接下来我们按下Play，我们现在可以移动球拍了：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3JhY2tldF9tb3ZlbWVudC5naWY)
### 7、这个球
球的图片
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2JhbGwucG5n?x-oss-process=image/format,png)
注意：选择图片，另存为，加入到项目中Sprite文件中
摆放到场景中：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2JhbGxfaW5fc2NlbmUucG5n)

添加碰撞器：Add Component -> Physics 2D -> Box Collider 2D
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLWJhbGxib3hjb2xsaWRlci5wbmc)
球也应该从墙壁上弹起。如果它直接飞向墙壁，那么它就会反弹到相反的方向。如果它撞到墙上45°角度，然后它应该在一个-45°夹角(等等)..这背后的数学非常简单，可以用脚本来完成，但是我们将通过分配一个物理材料球对撞机。物理材料包含有关对撞机物理方面的信息，如摩擦和弹性。

让我们右键单击项目区，选择Create -> Physics Material 2D给它起个名字球料:![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLWJhbGxtYXRlcmlhbHByb2plY3R2aWV3LnBuZw)
之后我们可以看看Inspector并调整材料性能以使其反弹：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLWJhbGxtYXRlcmlhbGluc3BlY3Rvci5wbmc)
现在我们只需要把材料从项目区进入材料球的对撞机插槽：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLWJhbGxjb2xsaWRlcndpdGhtYXRlcmlhbC5wbmc)

添加刚体：Add Component -> Physics 2D -> Rigidbody 2D
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3VuaXR5MjAxOC4zLWJhbGxyaWdpZGJvZHkucG5n)
Mass：0.0001(这将有助于防止球推开球拍)
Linear Drag：0(这将防止球使用重力)
Interpolate：Interpolate插值(使物理尽可能精确)
Collision Detection：Continuous连续(有助于防止因统一物理2D引擎bug而产生的碰撞错误)

### 8、添加球的脚本
Add Component -> New Script->Ball.cs

接着我们双击打开它：

```csharp
using UnityEngine;
using System.Collections;

public class Ball : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
我们不需要更新函数，所以让我们删除它：
```csharp
using UnityEngine;
using System.Collections;

public class Ball : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }
}
```
让我们用刚体的速度属性使其移动。向上由某人速度:

```csharp
using UnityEngine;
using System.Collections;

public class Ball : MonoBehaviour {
    // Movement Speed
    public float speed = 100.0f;

    // Use this for initialization
    void Start () {
        GetComponent<Rigidbody2D>().velocity = Vector2.up * speed;
    }
}
```
将Ball.cs赋给 球物体
我们可以按下播放键，看球从边框弹出：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2JhbGxfYm91bmNpbmcuZ2lm)
### 9、球拍碰撞角

当球击中球拍时，我们希望球员能够对球的外角有一定的控制：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL3JhY2tldF9ib3VuY2VfYW5nbGVzLnBuZw)

让我们打开球脚本，这样我们就可以实现出角功能了。当球与其他东西碰撞时，我们会用OnCollisionEnter2D函数，Unity自动调用的函数：

```csharp
using UnityEngine;
using System.Collections;

public class Ball : MonoBehaviour {
    // Movement Speed
    public float speed = 100.0f;

    // Use this for initialization
    void Start () {
        GetComponent<Rigidbody2D>().velocity = Vector2.up * speed;
    }

    void OnCollisionEnter2D(Collision2D col) {
        // This function is called whenever the ball
        // collides with something
    }
}
```

球的速度取决于它击中球拍的位置。

y值永远是1，因为我们希望它飞向顶部，-1是飞向底部。x值是它击中球拍的位置，也就是它的速度。

```c
1  -0.5  0  0.5   1  <- x值取决于它被击中的位置
===================  <- 这是那个球拍
```

我们真正需要做的就是找出球在哪里，相对于球拍。我们可以通过简单的分割球来做到这一点x球拍坐标宽度..这是我们的功能：

```csharp
float hitFactor(Vector2 ballPos, Vector2 racketPos,
                float racketWidth) {
    // ascii art:
    //
    // 1  -0.5  0  0.5   1  <- x value
    // ===================  <- racket
    //
    return (ballPos.x - racketPos.x) / racketWidth;
}
```
注：我们减去球拍PO.x值的球波塞值，以便得到相对位置。
这是我们的OnCollisionEnter2D函数：

```csharp
void OnCollisionEnter2D(Collision2D col) {
    // Hit the Racket?
    if (col.gameObject.name == "racket") {
        // Calculate hit Factor
        float x=hitFactor(transform.position,
                          col.transform.position,
                          col.collider.bounds.size.x);

        // Calculate direction, set length to 1
        Vector2 dir = new Vector2(x, 1).normalized;

        // Set Velocity with dir * speed
        GetComponent<Rigidbody2D>().velocity = dir * speed;
    }
}
```

如果我们按下播放，那么我们现在可以影响球的弹跳方向取决于它击中球拍的位置。


### 10、添加块
我们将使用以下图像作为我们的块：

蓝色：![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2Jsb2NrX2JsdWUucG5n)绿色：![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2Jsb2NrX2dyZWVuLnBuZw)粉红：![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2Jsb2NrX3BpbmsucG5n)红色：![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2Jsb2NrX3JlZC5wbmc)黄色：![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2Jsb2NrX3llbGxvdy5wbmc)

把这些资源导入到我们项目中去

添加碰撞器：Add Component -> Physics 2D -> Box Collider 2D

添加新脚本 Block.cs

然后我们打开它：

```csharp
using UnityEngine;
using System.Collections;

public class Block : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
我们可以移除启动而更新功能，因为我们不需要它们。我们已经熟悉了OnCollisionEnter2D函数，所以让我们再次使用它：

```csharp
using UnityEngine;
using System.Collections;

public class Block : MonoBehaviour {

    void OnCollisionEnter2D(Collision2D collisionInfo) {
        // Destroy the whole Block
        Destroy(gameObject);
    }
}
```

注：销毁(这个)只会破坏块脚本组件。如果我们想摧毁整个街区，那么我们就必须使用摧毁(游戏对象)，这会破坏块的游戏对象本身。


然后我们把这个块铺到场景中去：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hcmthbm9pZC1nYW1lL2Jsb2NrX2FsbC5wbmc)

然后按下播放键！

恭喜你，你已经完成了自己的2D打砖块游戏。
