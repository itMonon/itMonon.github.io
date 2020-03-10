---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《吃豆人》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
让我们在Unity里面制作一款吃豆人游戏，最初的吃豆人游戏于1980年10月发布，并很快成为有史以来最著名的街机游戏。这款游戏非常受欢迎，甚至连Unity公司也在他们的游戏引擎中加入了其中的一小部分：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019082315143920.png)
在本教程中，我们将使用Unity强大的2D功能，制作一个吃豆人游戏，我们会尽量让代码简单，像往常一样，尽可能的简单地解释，这样每个人都能理解它。


## 二、效果图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823151616643.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

## 三、教程
**注意：版本号，Unity5.0.0f4** 

**1.摄像机设置**

我们将选择主照相机在层次性然后将背景颜色设置为黑色。我们还将调整大小而位置如下图所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823151717524.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

**2.背景设置**
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9tYXplLnBuZw?x-oss-process=image/format,png)
保存下来，放到我们项目中Sprites文件夹中
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9tYXplX3Byb2plY3RhcmVhLnBuZw?x-oss-process=image/format,png)
然后修改它的属性。点击图片->Inspector面板
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9tYXplX2ltcG9ydHNldHRpbmdzLnBuZw?x-oss-process=image/format,png)
注：Pixels Per Unit单位像素值设置成8，这意味着8x8像素将适合在游戏世界的一个单位。我们将使用这个值作为我们所有的纹理。我们选择了8因为两个点之间的距离总是8。我们选择左下角为枢轴因为这样以后对齐就更容易了。

然后我们将图片拖到到场景中，并且归零：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823153804169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823153809797.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
添加物理效果：
将每一面墙都添加上BoxCollider，需要注意的是每一道墙的BoxCollider应该都是非常精确的，不然可能游戏开始的时候你可能会困到迷宫里面了。


**3.吃豆人设置**
吃豆人的运动方向不同，状态也不同，让我们在一个图像中绘制所有动画，其中每行有一个动画：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9wYWNtYW4ucG5n?x-oss-process=image/format,png)
将图片保存到Sprite文件夹中。

我们需要将图片处理一下，将这样图片做一下切片：
点击图片
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823154458493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
Texture Type设置成Sprite
Sprite Mode设置成Multiple
点击Apply

然后点击Inspector面板上的Sprite Editor进行切片：
把它切成16X16的网格，点击Apply
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9wYWNtYW5fc3ByaXRlZWRpdG9yX3NldHRpbmdzLnBuZw?x-oss-process=image/format,png)

制作动画

现在我们有了动画切片，就可以用它创建4个动画，分别是：
- 右（边），正确的(切片0、1和2)
- 左边(第3、4及5段)
- 向上(第6、7及8段)
- 降下来(第9、10及11片)

选择前三个切片，拖入到场景中：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9wYWNtYW5fYW5pbWF0aW9uX3JpZ2h0X3NsaWNlcy5wbmc?x-oss-process=image/format,png)
Unity就会提示我们在哪里保存动画
![\(https://noobtuts.com/content/unity/2d-pacman-game/pacman_animation_right_slices_toscene.png)\]](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9wYWNtYW5fYW5pbWF0aW9uX3JpZ2h0X3R3b2ZpbGVzLnBuZw?x-oss-process=image/format,png)
我们重复这个动画，将动画制作完毕。


**动画状态机**

我们有4个动画文件，但是还要知道什么时候播放动画，所以我们需要一个动画状态机，它有4个状态：
- 右（边），正确的
- 左边
- 向上
- 降下来

我们在状态机里面创建一个Float类型的参数，用来控制状态：

如果DirY>0然后去向上 (如DirY=1，DirY=2，DirY=3等等)
如果DirY<0然后去降下来 (如DirY=-1，DirY=-2，DirY=-3等)
如果DirX>0然后去右（边），正确的 (如DirX=1，DirX=2，DirX=3等等)
如果DirX<0然后去左边 (如DirX=-1，DirX=-2，DirX=-3等等)

稍后我们在代码中会用到他们：

```csharp
GetComponent<Animator>().SetFloat("DirX", 0);
GetComponent<Animator>().SetFloat("DirY", 0);
```
**过渡效果**
我们希望Unity能够根据这些参数自动切换到不同的动画状态。过渡就是用来实现这一点的。例如，我们可以从左边到右（边），正确的条件是DirX>0..然而，它被认为是最好的做法，有一个小的错误容差，因为浮点比较并不总是完美的，因此我们将使用DirX>0.1相反。

现在我们还必须使用DirX>0.1从其他状态过渡到右（边），正确的..为了避免我们做这些工作，我们可以使用Any State

这个Any State代表任何一个状态。所以如果我们从任何装啊提到右（边），正确的然后，这与创建一个从左边, 向上和降下来到右（边），正确的.

让我们右键单击任何状态并选择过渡..之后，我们可以将白色箭头拖到右（边），正确的状态：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823160506706.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
在Inspector面板中设置动画的参数：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190823161106210.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注意：这避免了在按住移动键的同时，动画总是被重新启动的奇怪情况。

因此，每当吃豆人走到右边(DirX>0.1)，动画师将切换到右（边），正确的状态并播放动画。

我们将增加3个过渡：
- 任何状态到左边有条件DirX<-0.1
- 任何状态到向上有条件DirY>0.1
- 任何状态到降下来有条件DirY<-0.1


我们几乎完成了我们的动画状态机。这里还有最后一个调整，所以让我们选择所有的状态，然后修改它们的速度在Inspector这样动画看起来就不会太快了：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9wYWNtYW5fYW5pbWF0b3Jfc3BlZWRzLnBuZw?x-oss-process=image/format,png)
**添加吃豆人**

添加物体状态
BoxCollider2D
添加刚体
Rigidbody2D 

移动脚本
PacmanMove.cs

```csharp
using UnityEngine;
using System.Collections;

public class PacmanMove : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
这个启动函数在开始游戏时由统一自动调用。这个更新函数被一次又一次地自动调用，大约每秒60次。(这取决于当前的帧速率，如果屏幕上有很多东西，它可能会更低).
还有另一种类型的更新函数，即修正更新..它也被一次又一次的调用，但是在一个固定的时间间隔内。单位物理是在完全相同的时间间隔内计算的，所以使用它总是一个好主意修正更新在做物理工作时：

```csharp
using UnityEngine;
using System.Collections;

public class PacmanMove : MonoBehaviour {

    void Start() {

    }

    void FixedUpdate() {

    }
}
```

我们需要一个公共变量，以便稍后可以修改移动速度：

```csharp
using UnityEngine;
using System.Collections;

public class PacmanMove : MonoBehaviour {
    public float speed = 0.4f;

    void Start() {

    }

    void FixedUpdate() {

    }
}
```

我们还需要一种方法来找出行动人员是否可以移动到某个方向，或者是否有一堵墙。所以举个例子，如果我们想知道帕克曼的顶端是否有一堵墙，我们可以简单地投线从一个单位以上的帕克曼到帕克曼，看看它是否击中任何东西。如果它击中了帕克曼本人，那就没有中间的任何东西，否则肯定有一堵墙。

以下是功能：

```csharp
bool valid(Vector2 dir) {
    // Cast Line from 'next to Pac-Man' to 'Pac-Man'
    Vector2 pos = transform.position;
    RaycastHit2D hit = Physics2D.Linecast(pos + dir, pos);
    return (hit.collider == GetComponent<Collider2D>());
}
```

注：我们只是从Pac-Man旁边的点抛出这条线(pos+dir)给帕克曼本人(pos).
我们还需要一个功能，使Pac-Man移动1单位到所需的方向.当然，我们可以这样做：

```c
transform.position += dir;
```
但这看起来太突然了，因为这是一个巨大的进步。相反，我们希望他能一帆风顺地去那里。让我们将移动目的地存储在变量中，然后使用修正更新一步地走向目的地：

```csharp
using UnityEngine;
using System.Collections;

public class PacmanMove : MonoBehaviour {
    public float speed = 0.4f;
    Vector2 dest = Vector2.zero;

    void Start() {
        dest = transform.position;
    }

    void FixedUpdate() {
        // Move closer to Destination
        Vector2 p = Vector2.MoveTowards(transform.position, dest, speed);
        GetComponent<Rigidbody2D>().MovePosition(p);
    }

    bool valid(Vector2 dir) {
        // Cast Line from 'next to Pac-Man' to 'Pac-Man'
        Vector2 pos = transform.position;
        RaycastHit2D hit = Physics2D.Linecast(pos + dir, pos);
        return (hit.collider == GetComponent<Collider2D>());
    }
}
```
注：我们使用GetComponent来访问Pac-Man的刚体组件。然后，我们使用它来进行移动(我们不应该使用Transform.Place来移动具有刚体的GameObjects)。
当我们不移动时，我们也要注意箭头键的按压。
注意：如果当前位置等于目的地，我们就不会移动。

下面是具有输入检查的FixedUpdate函数：

```csharp
void FixedUpdate() {
    // Move closer to Destination
    Vector2 p = Vector2.MoveTowards(transform.position, dest, speed);
    GetComponent<Rigidbody2D>().MovePosition(p);

    // Check for Input if not moving
    if ((Vector2)transform.position == dest) {
        if (Input.GetKey(KeyCode.UpArrow) && valid(Vector2.up))
            dest = (Vector2)transform.position + Vector2.up;
        if (Input.GetKey(KeyCode.RightArrow) && valid(Vector2.right))
            dest = (Vector2)transform.position + Vector2.right;
        if (Input.GetKey(KeyCode.DownArrow) && valid(-Vector2.up))
            dest = (Vector2)transform.position - Vector2.up;
        if (Input.GetKey(KeyCode.LeftArrow) && valid(-Vector2.right))
            dest = (Vector2)transform.position - Vector2.right;
    }
}
```
注：变换位置铸成矢量2因为这是比较或添加另一个Vector 2的唯一方法。也-向量2.右手段左边和-向量2.向上手段降下来.



设置动画参数
现在，我们可以很好地将Pac-Man移动到迷宫中，但是动画师还没有播放所有的动画。没问题，让我们修改代码来计算当前的移动方向，然后设置动画参数：

```csharp
void FixedUpdate() {
    // Move closer to Destination
    Vector2 p = Vector2.MoveTowards(transform.position, dest, speed);
    GetComponent<Rigidbody2D>().MovePosition(p);

    // Check for Input if not moving
    if ((Vector2)transform.position == dest) {
        if (Input.GetKey(KeyCode.UpArrow) && valid(Vector2.up))
            dest = (Vector2)transform.position + Vector2.up;
        if (Input.GetKey(KeyCode.RightArrow) && valid(Vector2.right))
            dest = (Vector2)transform.position + Vector2.right;
        if (Input.GetKey(KeyCode.DownArrow) && valid(-Vector2.up))
            dest = (Vector2)transform.position - Vector2.up;
        if (Input.GetKey(KeyCode.LeftArrow) && valid(-Vector2.right))
            dest = (Vector2)transform.position - Vector2.right;
    }

    // Animation Parameters
    Vector2 dir = dest - (Vector2)transform.position;
    GetComponent<Animator>().SetFloat("DirX", dir.x);
    GetComponent<Animator>().SetFloat("DirY", dir.y);
}
```

注：我们计算了当前的移动。迪尔基本向量数学。我们所要做的就是减去电流位置从长子理性。

**将吃豆人放在最前面**

在我们开始研究Pac-Dot之前，我们应该确保Pac-Man总是被吸引到他们的前面。我们正在做一个2D游戏，所以没有真正的Z就像3D游戏一样。这意味着，统一只会随意绘制对象。让我们确保团结总是在其他一切面前吸引帕克曼。

有两种方法可以做到这一点。我们可以要么更改雪碧渲染器氏分选层属性，或者我们可以更改层序财产。这个分选层对于拥有更多物品的大型游戏来说是很重要的。对我们来说，只要简单地更改层序到1:
Pac-Man SpriteRenderer Order in Layer
注意：统一绘制对象按其顺序排序。它以最低的顺序开始，以更高的顺序继续。所以，如果帕克-曼有第一项命令，那么他总是在迷宫、食物和其他任何有0顺序的东西之后抽签。因为他喜欢其他的东西，所以他会自动地排在其他的前面

**4.小圆点设置**
可以吃的小圆点叫做“点”(pac-dots)。有些人现在可能是“水果”或者只是“食物”。我们先画一张小画2x2PX图像的Pac-点：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9wYWNkb3QucG5n?x-oss-process=image/format,png)
将这个图片保存下来，拖到项目的Sprite文件夹中

然后拖到场景中，添加BoxCollider组件

让我们为这个小圆点添加一个脚本：
Pacdot.cs

```csharp
using UnityEngine;
using System.Collections;

public class Pacdot : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
我们不需要启动或者更新函数，所以让我们移除这两个函数。相反，我们将使用前面提到的OnTriggerEnter2D职能：

```csharp
using UnityEngine;
using System.Collections;

public class Pacdot : MonoBehaviour {

    void OnTriggerEnter2D(Collider2D co) {
        // Do Stuff...
    }
}
```
判断一下，穿过小圆点的是否是吃豆人，是的话就销毁自身：

```csharp
using UnityEngine;
using System.Collections;

public class Pacdot : MonoBehaviour {

    void OnTriggerEnter2D(Collider2D co) {
        if (co.name == "pacman")
            Destroy(gameObject);
    }
}
```
然后我们复制豆豆，将它像场景中摆放的一样，摆放一下：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9wYWNkb3RzX2FsbC5wbmc?x-oss-process=image/format,png)
5.敌人设置

红鬼形象：
和往常一样，我们将从画一个幽灵Sprite开始，里面有所有的动画。每一行将包含一个动画：
- 右（边），正确的
- 左边
- 向上
- 降下来

第一个将是红色的鬼魂，也被称为布林基：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9ibGlua3kucG5n?x-oss-process=image/format,png)
像上面一样进行以下处理：
切片成16X16网格
创建动画
创建动画状态机
设置状态参数


然后将它拖到场景中，添加物理BoxCollider2D

敌人的移动脚本：GhostMove.cs

```csharp
using UnityEngine;
using System.Collections;

public class GhostMove : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
我们不需要启动或者更新函数，相反，我们将使用修正更新功能(因为它是为了运动之类的物理材料而设计的):

```csharp
using UnityEngine;
using System.Collections;

public class GhostMove : MonoBehaviour {

    void FixedUpdate () {

    }
}
```
让我们增加一个公众变换数组，以便稍后可以在检查器中设置路径点：

```csharp
using UnityEngine;
using System.Collections;

public class GhostMove : MonoBehaviour {
    public Transform[] waypoints;

    void FixedUpdate () {

    }
}
```
注意：数组意味着它不仅仅是一个转换。
我们还需要某种索引变量来跟踪敌人当前走向的路径点：

```csharp
using UnityEngine;
using System.Collections;

public class GhostMove : MonoBehaviour {
    public Transform[] waypoints;
    int cur = 0;

    void FixedUpdate () {

    }
}
```

注意：当前的路径点总是可以用路标[路].
当然还有一个运动速度变量：

```csharp
using UnityEngine;
using System.Collections;

public class GhostMove : MonoBehaviour {
    public Transform[] waypoints;
    int cur = 0;

    public float speed = 0.3f;

    void FixedUpdate () {

    }
}
```
现在我们可以使用修正更新函数使其更接近当前路径点，或在到达时立即选择下一个路径点：

```csharp
void FixedUpdate () {
    // Waypoint not reached yet? then move closer
    if (transform.position != waypoints[cur].position) {
        Vector2 p = Vector2.MoveTowards(transform.position,
                                        waypoints[cur].position,
                                        speed);
        GetComponent<Rigidbody2D>().MovePosition(p);
    }
    // Waypoint reached, select next one
    else cur = (cur + 1) % waypoints.Length;
}
```

注意：我们使用了矢量2.运动塔函数来计算一个点，该点离路径点有点近。之后我们把鬼魂的位置刚体2D.移动定位..如果到达路径点，则增加库尔一分为二。我们还想重新设置库尔到0如果它超过列表长度。我们可以用这样的方法如果(cur=waypoints.length)cur=0，但是使用模块(%)操作符使这个看起来更优雅一些。
我们也不要忘记设置动画参数：

```csharp
void FixedUpdate () {
    // Waypoint not reached yet? then move closer
    if (transform.position != waypoints[cur].position) {
        Vector2 p = Vector2.MoveTowards(transform.position,
                                        waypoints[cur].position,
                                        speed);
        GetComponent<Rigidbody2D>().MovePosition(p);
    }
    // Waypoint reached, select next one
    else cur = (cur + 1) % waypoints.Length;

    // Animation
    Vector2 dir = waypoints[cur].position - transform.position;
    GetComponent<Animator>().SetFloat("DirX", dir.x);
    GetComponent<Animator>().SetFloat("DirY", dir.y);
}
```
太好了，还有最后一件事要添加到我们的脚本中。鬼魂和帕克曼相撞时应该把他杀死：

```csharp
void OnTriggerEnter2D(Collider2D co) {
    if (co.name == "pacman")
        Destroy(co.gameObject);
}
```
注意：请随意减少病人的生命或显示游戏结束屏幕就在这一点上。


6.路径点设置
好的，让我们为敌人添加一些路径点。我们将从选择游戏对象->创建空从上面的菜单。我们会把它重命名为Blinky_Waypoint 0然后分配一个这样我们才能更容易地在现场看到：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9ibGlua3lfd2F5cG9pbnQwX2luc3BlY3Rvci5wbmc?x-oss-process=image/format,png)
注意：Gizmo只是一个视觉助手，我们在玩游戏时看不到它。

我们也把它放在(15, 20)..现在情况如下：![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9ibGlua3lfd2F5cG9pbnQwX3NjZW5lLnBuZw?x-oss-process=image/format,png)
现在我们可以复制Waypoint，将其重命名为Blinky Waypoint 1把它放在(10, 20):![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9ibGlua3lfd2F5cG9pbnQxX3NjZW5lLnBuZw?x-oss-process=image/format,png)
然后Blinky Waypoint 2在…(10, 14):![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wYWNtYW4tZ2FtZS9ibGlua3lfd2F5cG9pbnQ0X3NjZW5lLnBuZw?x-oss-process=image/format,png)
由于我们的移动脚本将自动继续走到第一个路径点后，最后一个到达，我们将有一个完美的循环。

让我们选择敌人在层次性然后将一个又一个的路径点拖到路点我们的插槽GhostMove脚本中：

**7.开始游戏**

我们点击Play，然后我们就可以开始游戏了

**8.内容扩展**

现在该由读者来让游戏更有趣了。可以添加大量的功能：
- 音乐
- 分数
- 高级AI
- 开始界面
- 升级
- 生命值
- 道具奖励
- 更多动画
