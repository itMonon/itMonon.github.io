---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《乒乓球游戏》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
本篇文章教大家如何用unity制作一个2D游戏，乒乓球游戏，主要用到的Unity知识包括碰撞，Transfrom等知识，希望大家可以在这个小教程中学习到东西

## 二、效果图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191224182820413.gif)

## 三、正文
本教程将展示如何在统一游戏引擎中使用38行代码制作2D乒乓游戏。每件事都会一步步地解释，这样每个人都能理解它。

### 前言
这个二维乒乓球游戏受到1972年乒乓球游戏的启发。

首先让我们考虑一下游戏规则和看一下Unity引擎

请注意，本教程适用于Windows系统，但是在MacOS和Linux版本的Unity也是通用的，不过要注意不同的操作系统，需要注意项目的位置。


### 游戏规则
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvYmFzaWNfcnVsZXMucG5n?x-oss-process=image/format,png)
游戏规则应该非常类似于原来的乒乓球。如果球击中左边的墙，右边的球员得分一分。如果球击中右面墙，左边的球员就得分。如果它撞到了顶部或底部的墙壁，那么它就会反弹。每个球员将有一个球拍，可以上下移动，以击退球。

我们的球拍应该对球的外角产生影响：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvcmFja2V0X2JvdW5jZV9hbmdsZXMucG5n?x-oss-process=image/format,png)
- 如果球拍击中球顶拐角处，它应该会向我们的最高边界反弹。
- 如果球拍击中球中心，然后它应该反弹到右边，而不是向上或向下。
- 如果球拍击中球底部拐角处，它应该会反弹到我们的底部边界。

## 造墙
**墙结构**
让我们为游戏添加四面墙，我们需要建造一堵用Sprite建造的墙，或者可以说是：纹理

我们将使用一个水平的Sprite作为顶部和底部墙壁，一个垂直的Sprit用于左墙和右墙：
[WallHorizontal.png](https://noobtuts.com/content/unity/2d-pong-game/WallHorizontal.png)
[WallVertical.png](https://noobtuts.com/content/unity/2d-pong-game/WallVertical.png)
*注意：右击每个图像，选择另存为，把她们保存到项目中资产文件夹。*

好了，现在我们可以在Unity上看到他们了项目区：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvcHJvamVjdGFyZWFfd2FsbHRleHR1cmVzLnBuZw?x-oss-process=image/format,png)

### 墙导入设置

让我们选择两个墙壁图像，然后查看Inspector们将应用以下内容导入设置：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLWltcG9ydC13YWxscy5wbmc?x-oss-process=image/format,png)
注意：Filter Mode和Format可以用来决定质量和性能。一个单位像素价值1意味着1x1像素将适合在游戏世界的一个单位。我们将在所有纹理中使用这个值，因为球将是1x1像素，这个在游戏坐标中是一个单位。

修改导入设置，如果你是Unity的新手，那似乎是件很奇怪的事。事实上，我们的游戏可以很好的工作，根本不碰导入设置。然而，在2D游戏中，修改这些设置通常是个好主意，这样坐标大小才是合理的。(我们不想要一个100米大的球拍，这可能会使物理有点棘手).

### 为游戏世界添墙
因此，为了将墙壁添加到我们的游戏中，我们所要做的就是将它们从项目区进入场景：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvZHJhZ193YWxsX2ludG9fc2NlbmUucG5n?x-oss-process=image/format,png)
我们将把每一个纹理拖到场景中两次，因为我们有四堵墙：
[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-hlRQwDnH-1577180126955)(https://noobtuts.com/content/unity/2d-pong-game/walls_before.png)]
然后，我们将墙壁定位，使它们看起来像一个长方形，中间有摄像头：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvd2FsbHNfYWZ0ZXIucG5n?x-oss-process=image/format,png)
注意：我们可以通过拖动墙壁或选择它们，然后改变它们的位置来定位墙壁。位置在Inspector。

### 重命名墙壁
我们也会把墙壁重命名为WallLeft, WallRight, WallTop和WallBottom这样我们以后就不会失去概述了。重命名非常容易，我们所要做的就是右键单击层次性并选择重命名:
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLXJlbmFtZS5wbmc?x-oss-process=image/format,png)
这是我们的层次性看上去之后：
[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-DJyLCLln-1577180126956)(https://noobtuts.com/content/unity/2d-pong-game/unity-editor-hierarchyafterrename.png)]
### 墙壁的物理效果
现在我们可以看到游戏中的墙，但它们还不是真正的墙。它们只是游戏世界中的图像，纯粹的视觉效果。

我们希望墙壁是真实的墙壁，这样球拍和球拍就会与它们相撞，而不是直接穿过它们。

Unity伴随着一个非常强大的物理引擎，我们所要做的就是告诉Unity，我们的墙壁应该是Colliders，我们在Hierarchy选择它们：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLXNlbGVjdGVkd2FsbHMucG5n?x-oss-process=image/format,png)
之后，我们单击Add Component按钮中的Inspector然后选择Physics 2D -> Box Collider 2D，或者直接搜索Box，如下图：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLXdhbGxzYm94Y29sbGlkZXIucG5n?x-oss-process=image/format,png)
注意：无论我们在Inspector中做什么，都将对Hierarchy中选择的所有对象执行。因为我们选择了所有的四面墙，他们现在都有了Collider 。

现在我们可以在Inspector面板看到我们所有的墙壁都有一个BoxCollider 2D的组件：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLXNlbGVjdGVkd2FsbHN3aXRoYm94Y29sbDJkLnBuZw?x-oss-process=image/format,png)
注：--值是所选游戏对象之间不同的值。

如果我们看看场景然后我们还可以看到，每一堵墙现在都被一个绿色的矩形包围着：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvd2FsbF9jb2xsaWRlcl9pbnNjZW5lLnBuZw?x-oss-process=image/format,png)
注：绿色矩形是Collider。他们只在Scene 显示，而不是在最后的游戏。

我们还可以只选择一个墙来正确地查看所有的值：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLXNob3dpbmdzaW5nbGV3YWxsLnBuZw?x-oss-process=image/format,png)
### 添加虚线

好吧，如果你走到这一步，那就干得好！让我们在中间加上虚线。我们将使用以下纹理：
[DottedLine.png](https://noobtuts.com/content/unity/2d-pong-game/DottedLine.png)
注意：右击图像，选择另存为.。并将其保存在项目的资产文件夹。

我们将在Project Area然后应用我们以前用过的相同的导入设置：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLWRvdHRlZGxpbmVpbXBvcnQucG5n?x-oss-process=image/format,png)
之后，我们可以从Project Area进入场景..我们将把它放在游戏的中间位置：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvZG90dGVkbGluZV9pbnNjZW5lLnBuZw?x-oss-process=image/format,png)
注：虚线是理解Unity物理工作原理的一个很好的例子。现在，虚线只是一种纹理。只是我们能做的看见..球将不会与虚线碰撞，除非我们添加一个Collider对它(我们不会，因为球不应该与它相撞)。
### 创造球拍
**球拍纹理**
我们将用另一种白色纹理来制作球拍：

[Racket.png](https://noobtuts.com/content/unity/2d-pong-game/Racket.png)
注意：右击图像，选择但作为.。并将其保存在项目的资产文件夹。
我们将使用以下方法导入设置为此：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLWltcG9ydHJhY2tldC5wbmc?x-oss-process=image/format,png)
我们的比赛将有两个球员，一个在左边，一个在右边。所以让我们把球拍拖到游戏里两次，然后把它放在左边和右边：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvcmFja2V0c19pbl9zY2VuZS5wbmc?x-oss-process=image/format,png)


### 重命名球拍
为了让我们以后更好的控制这两个球拍，我们在Hierarchy面板中将这两个球拍重命名为RacketLeft和RacketRight：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLXJhY2tldHN3YWxsaGllcmFyY2h5LnBuZw?x-oss-process=image/format,png)

### 球拍的Physics
好的，我们的球拍应该利用统一的物理引擎。首先，它们应该能够与墙壁碰撞，因此，为什么我们再次添加对撞机，在层次结构中选择两个球拍，然后选择AddComponent->Physics2D->BoxCollider2D如下图：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLXJhY2tldGNvbGxpZGVycy5wbmc?x-oss-process=image/format,png)
球员也应该能够将球拍向上和向下移动。但是当球拍与墙相撞时，应该停止向上移动(或向下)。

听起来像一些复杂的数学在Unity中是非常容易的，因为刚体就是这样。它总是调整物体的位置，使其在物理上是正确的。例如，它可以自动对物体施加重力，也可以确保我们的球拍永远不会穿过墙壁。
注意：作为经验法则，任何在游戏世界中移动的物理事物都需要一个刚体.

为了给球拍添加刚体，我们只需要在Hierarchy 面板中中再次选择它们，然后查看Inspector面板 点击 AddComponent -> Physics2D -> BoxCollider2D
然后修改BoxCollider2D 禁用重力 (因为乒乓球没有重力) 通过设置重力标度到0，启用冻结旋转Z (球拍不应旋转)
然后我们设置Interpolate为Interpolate，这确保了物理学尽可能准确：
[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-vkugfFzp-1577180126961)(https://noobtuts.com/content/unity/2d-pong-game/unity-editor-racketrb2d.png)]


### 球拍运动
让我们确保球员能够移动他们的球拍。这种自定义行为通常需要脚本编写
在两个球拍仍被选中的情况下，我们将点击AddComponent ->New Script 然后输入名字MoveRacket
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLWNyZWF0ZW1vdmVyYWNrZXRzY3JpcHQucG5n?x-oss-process=image/format,png)
之后，我们可以双击打开它：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLW9wZW5tb3ZlcmFja2V0c2NyaXB0LnBuZw?x-oss-process=image/format,png)
下面是我们的脚本当前的样子：

```csharp
using System.Collections;
using UnityEngine;

public class MoveRacket : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}
```
这个Start函数在开始游戏时由Unity自动调用。这个Update函数被一次又一次地自动调用，大约每秒60次。

但还有另一个Update函数，它被称为FixedUpdate..这个也是一次又一次的调用，但是在一个固定的时间间隔内。这个函数也会被反复调用，但调用的时间间隔是固定的。Unity的物理计算是在完全相同的时间间隔内进行的，所以在物理运动的时候使用FixedUpdate通常是一个好主意。(我们想移动球拍，有BoxCollider和FixedUpdate，因此需要物理运动).

好的，让我们移除Start和Update函数并创建一个FixedUpdate功能代替：

```csharp
using System.Collections;
using UnityEngine;

public class MoveRacket : MonoBehaviour
{    
    void FixedUpdate()
    {
        
    }
}
```
注意：重要的是我们要准确地命名它FixedUpdate因为这是Unity所内设的名字。我们也可以使用不同的名称来创建函数，但是Unity不会自动调用它们。

球拍BoxCollider组件，我们将使用刚体的速度用于移动的速度。速度总是运动方向乘以速度.

方向是Vector2带着x(水平方向)和一个y(垂直方向)..下面的图像显示了一些Vector2示例：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdmVjdG9yMl9kaXJlY3Rpb25zLnBuZw?x-oss-process=image/format,png)
球拍只能上下移动，这意味着x组件将永远是0而y组件1为向上, -1为向下或0为不动.

这个y值取决于用户输入。我们可以检查各种按键(WSAD，箭头键，游戏手柄等)，或者我们可以使用Unity的GetAxisRaw函数：

```csharp
using System.Collections;
using UnityEngine;

public class MoveRacket : MonoBehaviour
{    
    void FixedUpdate()
    {
        float v = Input.GetAxisRaw("Vertical");
    }
}
```
注意：我们使用GetAxisRaw检查垂直输入轴。
当按下W键或者上箭头，或当指向游戏手柄的棍子向上，值会变成1。
当按下S键或者下箭头，或当指向游戏手柄的棍子向下。值会变成-1
当这些键都没有被按下的时候,值会变成0
换句话说，这正是我们所需要的。
现在我们可以用GetComponent进入球拍刚体组件，然后将其设置为速度

```csharp
using System.Collections;
using UnityEngine;

public class MoveRacket : MonoBehaviour
{    
    void FixedUpdate()
    {
        float v = Input.GetAxisRaw("Vertical");
        GetComponent<Rigidbody2D>().velocity = new Vector2(0, v);
    }
}
```
我们还将添加一个速度变量到脚本中，以便我们可以控制球拍的移动速度：

```csharp
using System.Collections;
using UnityEngine;

public class MoveRacket : MonoBehaviour
{   
    public float speed = 30;
        
    void FixedUpdate()
    {
        float v = Input.GetAxisRaw("Vertical");
        GetComponent<Rigidbody2D>().velocity = new Vector2(0, v);
    }
}
```
我们将Speed变量变成Public，这样我们就可以在Inspector面板中修改，而不需要更改脚本：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvbW92ZXJhY2tldF9zcGVlZC5wbmc?x-oss-process=image/format,png)
现在，我们可以修改脚本以使用速度变量：

```csharp
using System.Collections;
using UnityEngine;

public class MoveRacket : MonoBehaviour
{   
    public float speed = 30;
    void FixedUpdate()
    {
        float v = Input.GetAxisRaw("Vertical");
        GetComponent<Rigidbody2D>().velocity = new Vector2(0, v) * speed;
    }
}
```

注意：我们设置了速度，是方向乘以速度，这正是速度的定义。

如果我们保存脚本并按下Play现在我们可以移动球拍了：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvcmFja2V0c19tb3ZpbmcuZ2lm)
只有一个问题，我们还不能分开移动球拍。

### 增加运动轴
现在，我们的两个脚本都检查“垂直”运动计算的输入轴。让我们创建一个新的轴心变量，以便可以更改Inspector中的输入轴：

```csharp
using System.Collections;
using UnityEngine;

public class MoveRacket : MonoBehaviour
{   
    public float speed = 30;
    public string axis = "Vertical";
        
    void FixedUpdate()
    {
        float v = Input.GetAxisRaw(axis);
        GetComponent<Rigidbody2D>().velocity = new Vector2(0, v) * speed;
    }
}
```
让我们选择Edit -> Project Settings..这将弹出一个弹出式窗口，您可以调整各种设置，如下所示。从左侧菜单中选择输入..这将显示如下所示：
[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-9Ra570pd-1577180126963)(https://noobtuts.com/content/unity/2d-pong-game/unity-editor-inputmanager.png)]
在这里，我们可以修改当前垂直轴，以便它只使用W和S键。我们也将使它只使用Joystick 1.单击第一个垂直条目(通常位于水平下方)，使其看起来如下所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLW1vZGlmeXZlcnRpY2FsYXhpczEucG5n?x-oss-process=image/format,png)
现在我们将增加Size为了添加一个新轴：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLWluY3JlYXNlYXhpc3NpemUucG5n?x-oss-process=image/format,png)
我们给它起个名字Vertical 2并作相应修改：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLW1vZGlmeXZlcnRpY2FsYXhpczIucG5n?x-oss-process=image/format,png)
然后我们将选择RacketRight GameObject并将MoveRacket脚本的属性更改为Vertical2：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvbW92ZXJhY2tldF9heGlzX3ZlcnRpY2FsMi5wbmc?x-oss-process=image/format,png)
如果我们按下Play然后我们可以分开移动球拍：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvcmFja2V0c19tb3Zpbmdfc2VwZXJhdGVseS5naWY)
注意：我们也可以创建一个向上和向下脚本中的键变量，然后将其设置为w/s左边的球拍上下，上键/下键设置为右边的球拍上下。然而，通过使用轴，我们最终得到了更少的代码和完善的游戏平台/操纵杆/键盘支持。

### 创造球
**球织构**
快到了！创造球将再次容易。首先，我们将以下纹理保存到项目的“资产”文件夹中：

[Ball.png](https://noobtuts.com/content/unity/2d-pong-game/Ball.png)

注意：右击图像，选择但作为.。并将其保存在项目的资产文件夹。

我们将使用以下方法导入设置为此：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLWltcG9ydGJhbGxzZXR0aW5ncy5wbmc?x-oss-process=image/format,png)
现在我们可以从Project Area进入Scenes场景中：
[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-ik9JBapn-1577180126965)(https://noobtuts.com/content/unity/2d-pong-game/ball_in_scene.png)]
### 球的Collider
我们的球应该再次利用Unity的物理，所以如果球对象还没有被选中，让我们选择它，然后选择添加组件 -> Physics -> BoxCollider2D添加BoxCollider：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLWJhbGxib3hjb2xsaWRlcjJkc2V0dGluZ3MucG5n?x-oss-process=image/format,png)
我们的球应该从墙上弹出来。例如，当它直接飞向墙壁时，它应该在完全相反的方向反弹。在飞向墙的45°角时，应以-45°角等方式弹跳。

这听起来像是一些复杂的数学，可以用脚本来完成。但是，由于我们很懒，所以我们将通过分配一个Physics Material，让它一直与物体反弹。

首先，我们在我们的项目区中右击并选择Create -> Physics 2D Material我们会把它命名为BallMaterial:
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLWJhbGxtYXRlcmlhbC1wcm9qZWN0YXJlYS5wbmc?x-oss-process=image/format,png)
现在，我们可以在Inspector中修改它，使其弹出：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLWJhbGxtYXRlcmlhbGluc3BlY3Rvci5wbmc?x-oss-process=image/format,png)
然后我们将BallMaterial放入BoxCollider的Materrial插槽：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLWJhbGxjb2xsaWRlcndpdGhtYXRlcmlhbC5wbmc?x-oss-process=image/format,png)
仅此而已。现在球会反弹，以防它与比赛中的东西相撞。

### 球刚体
为了让我们的球在游戏世界中移动，我们将添加一个BoxCollider2D通过选择添加组件 -> Physics2D -> BoxCollider2D.
注意：记住，每个物理的东西，应该在游戏世界中移动将需要一个刚体。

我们将以几种方式修改Rigidbody组件：
- 我们不想让它利用地心引力
- 我们希望它有一个很小的质量，这样它就不会在碰撞时推开球拍
- 我们不想让它旋转
- 我们将插值和连续碰撞检测用于精确物理。

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdW5pdHktZWRpdG9yLWJhbGxyaWdpZGJvZHkucG5n?x-oss-process=image/format,png)
注意：这些修改对初学者来说并不是很明显。通常的工作流程是添加一个刚体，测试游戏，然后修改刚体，以防效果太大。

好的，在我们看到一些酷球运动之前还有一件事要做。我们会选择添加组件 -> 新脚本给它起个名字Ball.之后，我们双击打开它：

```csharp
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class Ball : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
让我们移除Update因为我们不需要它。相反，我们将使用Start函数给出球的初始速度。再一次，我们将使用方向乘以速度:

```csharp
using UnityEngine;
using System.Collections;

public class Ball : MonoBehaviour {
    public float speed = 30;

    void Start() {
        // Initial Velocity
        GetComponent<Rigidbody2D>().velocity = Vector2.right * speed;
    }
}
```
现在，如果我们按下Play键，我们就可以看到球从墙壁和球拍上弹起：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvYmFsbF9ib3VuY2luZy5naWY)
再说一遍，我们不必担心任何复杂的数学。Unity为我们提供了强大的物理引擎。

球拍碰撞角
我们的游戏看起来已经很像乒乓了，但是还有一个更重要的修改要做。我们在一开始就解释说，球的出角应该取决于它击中球拍的位置：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvcmFja2V0X2JvdW5jZV9hbmdsZXMucG5n?x-oss-process=image/format,png)
这样玩家就可以把球射向他们喜欢的方向，这给比赛增加了巨大的战术成分。

让我们修改Ball脚本以使用OnCollisionEnter2D在与其他东西碰撞时由Unity自动调用的函数：

```csharp
using UnityEngine;
using System.Collections;

public class Ball : MonoBehaviour {
    public float speed = 30;

    void Start() {
        // Initial Velocity
        GetComponent<Rigidbody2D>().velocity = Vector2.right * speed;
    }

    void OnCollisionEnter2D(Collision2D col) {
        // Note: 'col' holds the collision information. If the
        // Ball collided with a racket, then:
        //   col.gameObject is the racket
        //   col.transform.position is the racket's position
        //   col.collider is the racket's collider
    }
}
```
所以现在我们需要一个函数来计算球的速度取决于它击中球拍的位置。下面的图像再次显示了几个运动方向的Vector 2：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wb25nLWdhbWUvdmVjdG9yMl9kaXJlY3Rpb25zLnBuZw?x-oss-process=image/format,png)
x值得主要作用是防止它从球拍上跳下来，-1是防止它从右边的球拍上跳下来，1是防止它从左边的球拍上跳下来。
我们需要考虑的是y值，它将介于-1和1..我们真正需要计算的是：

>||  1 <- 在球的顶端
||
||  0 <- 在球的中间
||
|| -1 <- 在球的下面

换句话说，我们只需要找出球与球拍的关系。或者换句话说：我们只需要把球分成两半y球拍坐标高度..我们的函数如下：

```csharp
float hitFactor(Vector2 ballPos, Vector2 racketPos,
                float racketHeight) {
    // ascii art:
    // ||  1 <- 在球的顶端
    // ||
    // ||  0 <- 在球的中间
    // ||
    // || -1 <- 在球的下面
    return (ballPos.y - racketPos.y) / racketHeight;
}
```
注：我们从球拍中减去球拍，得到一个相对位置。

看一下我们最后OnCollisionEnter2D函数最后的样子：

```csharp
void OnCollisionEnter2D(Collision2D col) {
    // Note: 'col' holds the collision information. If the
    // Ball collided with a racket, then:
    //   col.gameObject is the racket
    //   col.transform.position is the racket's position
    //   col.collider is the racket's collider

    // Hit the left Racket?
    if (col.gameObject.name == "RacketLeft") {
        // Calculate hit Factor
        float y = hitFactor(transform.position,
                            col.transform.position,
                            col.collider.bounds.size.y);

        // Calculate direction, make length=1 via .normalized
        Vector2 dir = new Vector2(1, y).normalized;

        // Set Velocity with dir * speed
        GetComponent<Rigidbody2D>().velocity = dir * speed;
    }

    // Hit the right Racket?
    if (col.gameObject.name == "RacketRight") {
        // Calculate hit Factor
        float y = hitFactor(transform.position,
                            col.transform.position,
                            col.collider.bounds.size.y);

        // Calculate direction, make length=1 via .normalized
        Vector2 dir = new Vector2(-1, y).normalized;

        // Set Velocity with dir * speed
        GetComponent<Rigidbody2D>().velocity = dir * speed;
    }
}
```
如果我们按下Play，我们现在可以影响球的弹跳方向，这取决于我们用球拍击中它的位置。

### 摘要
恭喜你走了这么远！

在本教程中，我们学习了如何创建一个只有一些纹理的基本场景，使用Unity的2D物理引擎和创建脚本添加自定义游戏机制。

当对游戏做进一步的改进时，一定要记住：Unity很简单，只需点击几下鼠标或几行C#代码就可以完成几乎所有的事情。有大量的功能可以增加，使游戏尽可能有趣：
- 添加一个像顶部显示的拖动效果
- 加上我们都喜欢的古老的乒乓声音
- 分数功能
- 随着时间的推移，提高球的速度
- 增加AI敌人
- 添加菜单和学分屏幕
