---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《贪吃蛇》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
贪吃蛇游戏是一款经典的益智游戏，有PC和手机等多平台版本。既简单又耐玩。该游戏通过控制蛇头方向吃蛋，从而使得蛇变得越来越长。
那么如何用unity做一个贪吃蛇游戏呢，就跟随作者一起实现以下吧。

## 二、效果图
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEhRSzhaSVd2WmljVFhURWNtaFdoOVlhc0o1aktkMnFVMmljbEdKa1VtWmFCeVlkcUFwS0FJd1daV3pqTjYzU1pVdmVUQVZhbXcyMVFCQS8w?x-oss-process=image/format,png)

GitHub地址：https://github.com/764424567/Game_Snake
**注意：可以直接在GitHub仓库克隆或者下载源代码*

## 三、正文
**3.1 介绍**

这篇文章将讲解怎么使用Unity制作简单的贪吃蛇游戏。贪吃蛇是一种街机游戏，最早的原型诞生于1976年。正如大多数街机游戏一样，它开发简单，且娱乐性强（至少克森的童年时玩它玩过来的）。

**3.2 知识**

我们的文章不需要什么特别的技巧，只需知道Unity的基本操作，例如：GameObject和Transform的概念等等。即使你不了解也没关系，只要你跟着我的思路来即可。

**3.3 Unity版本**

在本章教程中，我们将使用Unity5.0.04版本来制作。对于旧的版本也可以正常运行，不过建议大家还是使用Unity5.0以上的版本。

项目设置
让我们开始吧。首先先创建一个项目：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVwMWlhV1B6UEs3TE1rUVNGaWNtWFFzOUY4bGlhd3ptUFhhTDI1eDZ4Sm4ycmNvQUE0ODBWaWN3NXNRLzA?x-oss-process=image/format,png)
将该工程命名为“snake”，路径由你们来设置，这里我设置的是C盘根目录下，选择2D开发，然后点击创建项目按钮：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnUyeUZKY2thTUFtelFLYjNFRXZ3amJ0N1JZWGhOZFBzMnpZUTdSQzRTNm1TNXVtT3BIUkdXUFEvMA?x-oss-process=image/format,png)
我选择场景中的Main Camera（主相机），然后再Inspector面板中修改相机的Background为黑色背景，最后调整Size和Position，如下图所示（注意参数要一样，方便后续跟进）：


![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnUxNHh3enZnWDFYanJpY2M1bkdHQTFjd2FQSXZSRjJiUDNLUjRFQXhiNENhanZIYUdsVjVIQnZnLzA?x-oss-process=image/format,png)

提示：Size是相机缩放调节的参数



**3.4 添加边界**

我们将使用下面两张图片来制作我们的边框：
- [border_horizontal.png](https://noobtuts.com/content/unity/2d-snake-game/border_horizontal.png)

- [border_vertical.png](https://noobtuts.com/content/unity/2d-snake-game/border_vertical.png)



提示：你可以右击将图片保存到你项目的Assets文件夹下。

我们再一次在Assets下选择这两张图片，如下所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVpYlFuQURRbjU2S1k1aWJERVhSVXgzTEg2QVRNVWlhZkZGZWQzUk5nczM4SWliUlpYaWFTRWFTQzF5US8w?x-oss-process=image/format,png)
之后，我们可以在Inspector面板中改变他们的导入设置，改变参数如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVoVlRIMUJNZ2JMTTU5dDh3UzUyc0hZWGZmQWVrWVpCaEdpYkRpYnQwaWNGeGFxcTA1VzFqb3BwQ3cvMA?x-oss-process=image/format,png)
提示：Pixels Per Unit 是在图片中的一个像素与世界坐标中的一个单位之间的比例尺。贪吃蛇每移动一步将对应游戏世界坐标上的一个单位。这就是为什么我们要把Pixels Per Unit设置为1的原因。

现在，我们可以制作我们的边框了。首先把Assets下的两张图片拖拽到Hierarchy面板下，拖拽两次（你也可以通过复制的方式实现），如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVLdEVsWk5IT0trMWcwUHpySDUzM0J1cU9JVVVTRjJLczRmQjFaVDdGOTRLS2FqWXZ6a0Jua3cvMA?x-oss-process=image/format,png)
提示：使用border_horizontal来制作顶部和底部的边框，使用border_vertical来制作左边和右边的边框。

让我们为它们重命名一下，方便查找。如下图所示（Top是顶部，Bottom是底部，Left是左边，right是右边）：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnU1WjdndXlMVUZQSEJvMHA2aG5tZUgxQnNvMjFTbjFtRE5lMnNjOXF3emliaGliR09QS1NSd01VQS8w?x-oss-process=image/format,png)
现在，它们在游戏中只是一张一张的图片，毫无卵用，现在就让我们为这些图片添加Colliders（碰撞器）组件，让这些图片变为一堵堵墙吧。

首先先在Hierarchy面板中选择那四张图片，如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVYM2ljbWVSODBJSWdjRDQzRTRkbVdqZFZsSWxKcjdCdUd1c3pEUU8yc2phTElhSG9sUGxxWlNnLzA?x-oss-process=image/format,png)
好，把它们都选中之后，在Inspector面板中找到Add Component按钮，点击它，然后找到Physics2D，最后点击Box Collider2D即可，如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVqbmhDTWpmRTVncEhvYUk2clhjUE9wS0dsWHFpY2tBMmQ4NUkycHJqaWFpY3lhZlJ0bWZUekIxUUEvMA?x-oss-process=image/format,png)
刚刚我们所做的操作，不用写任何一行代码，便能让一张毫无卵用的图片编程了一堵墙，太感谢Unity这个强大的游戏引擎了。

**3.5 创建食物预制体**
我们不想让我们的贪吃蛇饿死，因此，让我们在游戏中随机生成一些食物，提供给蛇食用吧。和上面的操作一样，我们将使用一张图片来制作食品。在我们的教程中，他只是一个像素的色块：
[food.png](https://noobtuts.com/content/unity/2d-snake-game/food.png)

提示：还是老样子，右击该图片，将它保存到你项目的Assets文件夹下。


还是老样子，将它的导入设置修改一下，如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVwMWpnS1BNTXFpY2MyNUFyQ0RyVzdFZHNpYVY3Y2Jlb0FxS3h6Y3M3QVB3R1JOM24zMzZiWVRpYXcvMA?x-oss-process=image/format,png)
好吧，让我们把food拖到场景中，Unity会自动的帮我们在Hierarchy面板中创建一个相对应的游戏物体，如下所示（你的位置也许跟下图不一样，这根据你拖拽的位置而定）：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVxaWFmY240bHR0RHNTTklYN3hhWjRxQlBPZlIzMXRhclpnb1A1RGdwZjFsNzFjdUh2dXZIMmx3LzA?x-oss-process=image/format,png)
每当贪吃蛇碰到food（也就是食物）的时候，应当获得一些相应的信息。因此我们也要给

food（食物）添加Collider（碰撞器）组件。

一个游戏物体没有Collider（碰撞器）组件，那么它只是一个可视化物体（就是没有交互功能的物体），它不是物理世界的一部分。一旦我们为游戏物体添加了Collider（碰撞器）组件，它如一堵墙，任何物体都不能穿透它，且能通过碰撞检测事件来进行交互，如：OnCollisionEnter2D、OnCollisionStay2D等等。假如我们勾选了Is Trigger，它便如水一般，可以穿透它，且能通过触发检测事件来进行交互，如：OnTriggerEnter2D、OnTriggerStay等等。

当贪吃蛇穿过food（食物）时，贪吃蛇应该得到一些通知（就是所谓的响应事件）。然而食物不能像墙一样不能穿过它，因此我们现在要做的就是为food（食物）添加Collider（碰撞器），并且勾上Is Trigger：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnV4bWNPeWliNGliMEtBS0tnN2ljNkZ1Tm5pY3JwdFhvSjVsQXFsMnJrVkdybEpuNWZNUHc4MlFURWNnLzA?x-oss-process=image/format,png)
好了，现在我们不想让food（食物）在游戏一开始就出现。因此我们把它做成一个预制体，以便我们使用Instantiate函数来生成它，每当我们需要它的时候。现在，让我们把food（食物）重命名为“FoodPrefab”，然后把它拖到Assets文件夹下：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnV1bEh1U1NkZkVFZkNCeWRJaWNNaWFGNU5vQjdRN2tCTDAxcVRxQjFhNkNQSWlja1lZckRpYmJwQlZRLzA?x-oss-process=image/format,png)
现在我们可以删除Hierarchy面板中的FoodPrefad了，因为我们暂时不需要它了。

**3.6 生成食物**
让我们在游戏开始之后，间隔几秒就在随机的地方生成一个food（食物）。那么，就让我们创建一个脚本来控制食物的生成吧。我们将把脚本放置在Main Camera下（因为Main Camera始终在游戏场景中）。首先，在Hierarchy中选择Main Camera，然后再Inspector面板中招到Add Component按钮，点击New Script，在Name的输入框中输入SpawnFood，脚本类型选择C Sharp，如下所示：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVPb1ZYQk5ncFdDSXBaQnVpYm5vdXY3akVBTTFmY1NtMmN4WmNSV1FWMFByaWFmUG1zZUE1ZWlhcEEvMA?x-oss-process=image/format,png)
然后打开该脚本（双击即可）：

```csharp
using UnityEngine;
using System.Collections;

public class SpawnFood : MonoBehaviour {

// Use this for initialization
    void Start () {

}

// Update is called once per frame
    void Update () {

}
}
```
我们不需要Update()函数，把它删除了：

```csharp
using UnityEngine;
using System.Collections;

public class SpawnFood : MonoBehaviour {

// Use this for initialization
    void Start () {

}
}
```
这个脚本需要获取食物预制体，因此，我们将添加一个类型为GameObject类型的公开变量：

```csharp
using UnityEngine;
using System.Collections;

public class SpawnFood : MonoBehaviour {
    // Food Prefab
    public GameObject foodPrefab;

// Use this for initialization
    void Start () {

}
}
```
食物应该是在边界内生成的。因此，我们也需要在我们的脚本中通过一些变量获得边框位置的相应信息，如下所示：

```csharp
using UnityEngine;
using System.Collections;

public class SpawnFood : MonoBehaviour {
    // Food Prefab
    public GameObject foodPrefab;

// Borders
    public Transform borderTop;
    public Transform borderBottom;
    public Transform borderLeft;
    public Transform borderRight;

// Use this for initialization
    void Start () {

}
}
```
提示：我们已经将他们声明为Transform类型了，因此我们如borderTop.transform.posion这样调用position了，直接borderTop.position即可。

让我们创建Spawn()函数在边界内生成food（食物）。首先我们通过x变量来获取左边界和右边界之间的随机位置信息，然后通过y变量来获取上边界和下边界之间的随机位置信息。然后我们便在该位置生成food（食物）：

```csharp
// Spawn one piece of food
void Spawn() {
    // x position between left & right border
    int x = (int)Random.Range(borderLeft.position.x,
                              borderRight.position.x);

// y position between top & bottom border
    int y = (int)Random.Range(borderBottom.position.y,
                              borderTop.position.y);

// Instantiate the food at (x, y)
    Instantiate(foodPrefab,
                new Vector2(x, y),
                Quaternion.identity); // default rotation
}
```

提示：x 和 y通过使用（int）强制转换来确保该food（食物）生成的位置是整数，如（1，2），而不是带有小数点的形式，如（1.234, 2.74565）。

现在，让我们的脚本在每几秒后调用Spawn()函数，我们可以通过使用 **InvokeRepeating()** 函数来做：

```csharp
// Use this for initialization
void Start () {
    // Spawn food every 4 seconds, starting in 3
    InvokeRepeating("Spawn", 3, 4);
}
```
**3.6 编辑器**

InvokeRepeating()函数用于在每几秒内重复调用某个函数。第一个参数是函数的名字，第二个参数是第一次调用的时间，第三个参数是间隔调用的时间。在上面的代码中，在游戏开始后3秒调用Spawn函数，然后每4秒再重复调用Spawn函数。

下面是SpawnFood函数的完整代码：

```csharp
using UnityEngine;
using System.Collections;

public class SpawnFood : MonoBehaviour {
    // Food Prefab
    public GameObject foodPrefab;

// Borders
    public Transform borderTop;
    public Transform borderBottom;
    public Transform borderLeft;
    public Transform borderRight;

// Use this for initialization
    void Start () {
        // Spawn food every 4 seconds, starting in 3
        InvokeRepeating("Spawn", 3, 4);
    }

// Spawn one piece of food
    void Spawn() {
        // x position between left & right border
        int x = (int)Random.Range(borderLeft.position.x,
                                  borderRight.position.x);

// y position between top & bottom border
        int y = (int)Random.Range(borderBottom.position.y,
                                  borderTop.position.y);

// Instantiate the food at (x, y)
        Instantiate(foodPrefab,
                    new Vector2(x, y),
                    Quaternion.identity); // default rotation
    }
}
```
现在让我们保存脚本，然后回到Inspector面板中，你将会发现SpawnFood脚本下多了几个卡槽，现在我们要做的就是找到对应的预制体，将其拖进卡槽中，如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnUzeXkxUEFFMkxscUt0bHVLVGoxdGIwVlRaTFNNRFd4T0lYUERYWktxNnh2TTl3TTV5d0dQRXcvMA?x-oss-process=image/format,png)
好吧，现在让我们点击Play按钮，然后等待几秒钟，我们将能看到游戏场景中有了一些小点点（食物）：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVvRVp1cm5UWDVqdmozQWljQUxwRnlpY1VLUzVrdzFoNUNScjBtTXgzMlI0Ujg5Q29XRkV5cWlhUncvMA?x-oss-process=image/format,png)
**3.7 创建贪吃蛇**
接下来让我们来完成游戏中最重要的部分：贪吃蛇。和上面一样，将图片保存到Assets文件夹下，然后修改导入设置：
[snake.png](https://noobtuts.com/content/unity/2d-snake-game/snake.png)
（由于图片太小，我直接放到文件夹中提供大伙们下载）

snake的导入设置如下（参数要一致）：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVNaWFTbXdCcHlGamxISmpKMGFUR3MzM0JEbHhmZWgyckRuN25xMUlvQTFtOVhpYVZzaWNua2ZnaWJnLzA?x-oss-process=image/format,png)
现在，我们可以拖snake图片到场景中，你将会看到如下图所示：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnU5TnBURmV2bGdhRlpiVEo3WHBheTJQaWFTNXJQOUZ0Nm9qRXFVZDZLcFk5NkZEV09FSHBOS3lRLzA?x-oss-process=image/format,png)
到目前为止，我们做好了蛇头。那么，让我们把它命名为“Head”方便查找：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVRMDJjYkFiRGlhbVVQRlg2MnBpYWQ3S283bVJPSzhXRDBWd0didU5aWDg2aWFMc0luamliTmlha0RSZy8w?x-oss-process=image/format,png)
贪吃蛇应该是物理世界的一部分，因此让我们为它添加Collider（碰撞器）组件，具体操作不用说了吧（Add -> Physics 2D -> Box Collider 2D）：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVyRzBTWlhNUXNaem5vajQxTGw1REFkTUdBQkl0eVNvQkZKZ1NrMmVKcTF6aWN6Z0lNeHVVaWNpY3cvMA?x-oss-process=image/format,png)
提示：将碰撞器的Size大小调整为（0.7， 0.7），以便它不与边界发生直接的碰撞，让蛇能在边框上爬行。如果把它调整为（1，1），当蛇到边界的时候便直接Game Over。所以，你懂的。

现在，我们要让蛇动起来，在物理世界中要想移动，那就得添加Rididbody（刚体）组件，刚体组件负责管理例如Gravity（重力）、Velocity（速度）和forces（力）。我们可以通过Add Component -> Physics 2D -> Rigidbody 2D来为其添加刚体。然后为蛇设置如下参数：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVhV2FCVVZ4ZEFuQmlja2lhYXBzMHF2V2JlZzBVd3puZlg3QW5LMWNLalVURFBzUE03RzRtRldYUS8w?x-oss-process=image/format,png)

1. 我们把Gravity Scale（重力大小）设置为 0，因为我们不想让蛇往屏幕下面掉。
2. 将Is Kinematic勾选上，用于禁用刚体的物理行为。我们只需要知道贪吃蛇与谁发生碰撞，不需要Unity的物理系统能帮我们做任何事情。（因为要想发生碰撞检测事件，其中一个游戏物体上必须要有刚体组件）


最终蛇将会包括很多个小元素，总会有一个头，和几个小元素组成的尾部，如下所示：

**3.8 组成蛇身**

在尾部元素和头部之间，唯一不同的是，稍后我们将添加一个脚本。

现在，让我们把Hierarchy中的Head拖到Assets文件夹下做成一个预制体，并且命名为“TailPrefab”。用于当蛇吃到food（食物）时加载该预制体添加到Head的尾部：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnU1ajBGU2ZRTld0TUxwZmFqcjVvZ2VMaE94dm9BU0l6RmZtOFV2Q0hPd3RIZU44Z0xUVG5CRHcvMA?x-oss-process=image/format,png)
提示：当我们制作好预制体后，确保一下Hierarchy面板中的Head是否被改名字了，如果被Unity自动修改了名字，那就将它改回“Head”。

好了，让我们在Hierarchy面板中选择Head，，然后在Inspecotr中找到Add Component按钮，为其添加Snake脚本（Add Component -> New Script）：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVGMzNCTk5Sc2w4NDJGcUwxaWMwQ3lxMGJES1YyNmFTUDVtek5ESFlpYWJGWENwNWt4dmgwVVFDZy8w?x-oss-process=image/format,png)
双击打开脚本：

```csharp
using UnityEngine;
using System.Collections;

public class Snake : MonoBehaviour {

// Use this for initialization
    void Start () {

}

// Update is called once per frame
    void Update () {

}
}
```

让我们使用using导入一些命名空间，后面我们需要：

```csharp
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
using System.Linq;

public class Snake : MonoBehaviour {

// Use this for initialization
    void Start () {

}

// Update is called once per frame
    void Update () {

}
}
```
现在，让我们为该脚本添加一个Move()函数，用于蛇的移动，然后我们在Upadate()中调用，因为直接在Upadate()中调用Move()函数的话，这将会导致蛇移动得非常的快，因此我们便用到了InvokeRepeating()函数来调用Move()函数，这个函数之前我们已经介绍过了，在这里，我们让Move每300毫秒调用一次：

```csharp
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
using System.Linq;

public class Snake : MonoBehaviour {

// Use this for initialization
    void Start () {
        // Move the Snake every 300ms
        InvokeRepeating("Move", 0.3f, 0.3f);    
    }

// Update is called once per frame
    void Update () {

}

void Move() {
        // Do Movement Stuff..
    }
}
```
蛇应该会朝一些方向移动，因此，让我们在Move()函数中定义一个方向变量来控制蛇的移动方向：

```csharp
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
using System.Linq;

public class Snake : MonoBehaviour {
    // Current Movement Direction
    // (by default it moves to the right)
    Vector2 dir = Vector2.right;

// Use this for initialization
    void Start () {
        // Move the Snake every 300ms
        InvokeRepeating("Move", 0.3f, 0.3f);    
    }

// Update is called once per frame
    void Update () {

}

void Move() {
        // Move head into new direction
        transform.Translate(dir);
    }
}
```
提示：transform.Translate 意味着根据一个向量来位移，上面的向量是Vector2.right。以为着朝右边（X轴的正轴）移动一个单位。

如果我们现在点击Play按钮，将会看到如下图所示变化：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVzV2EwcnFRRE1DS2pYM1hhRFA5QTJQSFRaYTJ1Q2liS1dYQTg1QWI0YTZNYktpYWowTzdHSnZ5QS8w?x-oss-process=image/format,png)
然而，用户应该可以通过按下某一个方向键来控制蛇朝某一个方向移动，因此我们依据上面的方法来创建用户按下方向键响应事件：

```csharp
// Update is called once per Frame
void Update() {
    // Move in a new Direction?
    if (Input.GetKey(KeyCode.RightArrow))
        dir = Vector2.right;
    else if (Input.GetKey(KeyCode.DownArrow))
        dir = -Vector2.up;    // '-up' means 'down'
    else if (Input.GetKey(KeyCode.LeftArrow))
        dir = -Vector2.right; // '-right' means 'left'
    else if (Input.GetKey(KeyCode.UpArrow))
        dir = Vector2.up;
}
```
现在点击Play按钮进行测试（记得按下方向键蛤）：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEVHeTZMQTJsT2VuTHpBd0t2WEhLRnVtcGZKV2M2MUw4V05OTnk4STl1cm0ybzJYVkk0aHVyeGJmcmZrZzRiU3BubnFkOWRoaWFORmljUS8w?x-oss-process=image/format,png)
**3.9 蛇的尾部**
现在让我们思考一下蛇的尾部该如何操作。首先，让我们假设我们有一个蛇，它有由一个Head（头）和3个尾部组成：

ooox
现在，一旦Head（头）移动，它的尾部元素也将移动，它尾部是这样工作的，当前尾部向前移动一位，它的后一个元素就移动到它原来的位置，如下面分析所示：

step 1: ooox   // snake didn't move yet
step 2: ooo x  // head moved to the right
step 3: oo ox  // first tail follows
step 4: o oox  // second tail follows
step 5:  ooox  // third tail follows
但是，这样工作的话，会让代码变得非常的复炸，让我们使用一个小小的技巧，让我们的工作变得更简单吧。我们让尾部作为一个整体，只要Head（头）移动一位，尾部作为一个整体也移动一个单位，这样便变得简单多了，如下面分析所示：

step 1: ooox   // snake didn't move yet
step 2: ooo x  // head moved to the right
step 3:  ooox  // last tail element moved into the gap
现在看上去像是一个简单的算法。在每一次移动时调用。

首先，我们将需要一些数据结构，用于追踪（存放）所有尾部元素，如下所示：

// Keep Track of Tail
List<Transform> tail = new List<Transform>();


注意：List泛型类位于System.Collections.Generic命名空间中，因此之前我们使用using System.Collections.Generic是非常重要的。

让我们添加一个变量用于存储蛇头的移动前的位置，然后让尾部最后一个元素移动到蛇头移动前的位置，然后移除尾部最后一个元素在列表中的原来的位置（妈蛋，绕，不知道大伙们能否明白），然后：

```csharp
void Move() {
    // Save current position (gap will be here)
    Vector2 v = transform.position;

// Move head into new direction (now there is a gap)
    transform.Translate(dir);

// Do we have a Tail?
    if (tail.Count > 0) {
        // Move last Tail Element to where the Head was
        tail.Last().position = v;

// Add to front of list, remove from the back
        tail.Insert(0, tail.Last());
        tail.RemoveAt(tail.Count-1);
    }
}
```
这是我们贪吃蛇教程中最复杂的部分，不过我们差不多完成了。

**3.10 喂蛇**
我们将使用O你TriggerEnter2D()函数来接收碰撞信息（它将用于当蛇撞到墙和穿过食物的时候调用）。

每当蛇触碰到食物的时候，我们将使用相同的操作（上面的思路），在间隙的地方实例化一个新的尾部元素，思路如下图所示：

ooo x  // gap
oooox  // gap filled with new element
去理解它是非常重要的，我们不让蛇在吃到食物后立马再吃到食物，就像我们的方向键检测按下一样，我们将等待它把当前动作完成之后。因此，我们需要一个新的变量，当蛇吃到食物后让它变为true：

// Did the snake eat something?
bool ate = false;
我们也需要一个公用的变量去存放尾部元素物体，用于当蛇碰到食物时再尾部生成该物体：

// Did the snake eat something?
bool ate = false;

// Tail Prefab
public GameObject tailPrefab;




注意：这两个变量是在我们的Snake脚本中定义的。

现在，让我们编写O你TriggerEnter2D()函数的代码。具体用于当蛇碰到食物的时候，让ate变量的值变为true，然后删除触碰到的food物体。如果碰到的物体时边界的话，我们也让它做一些事情（当然目前还没有让它做什么事情）：

```csharp
void OnTriggerEnter2D(Collider2D coll) {
    // Food?
    if (coll.name.StartsWith("FoodPrefab")) {
        // Get longer in next Move call
        ate = true;

// Remove the Food
        Destroy(coll.gameObject);
    }
    // Collided with Tail or Border
    else {
        // ToDo 'You lose' screen
    }
}
```
注意：我们使用coll.name.StartsWith()函数，因为我们要检测该物体是食物还是边界。当然，更好的方式是使用tag去判断，但是为了简单起见，我们就直接比较字符串即可。

好了，让我们修改我们的Move()函数，判断蛇是否吃到了食物（也就是ate是否为true），然后在我们的尾部生成一个尾部元素，最后让ate变为原来的false值：

```csharp
void Move() {
    // Save current position (gap will be here)
    Vector2 v = transform.position;

// Move head into new direction (now there is a gap)
    transform.Translate(dir);

// Ate something? Then insert new Element into gap
    if (ate) {
        // Load Prefab into the world
        GameObject g =(GameObject)Instantiate(tailPrefab,
                                          v,
                                              Quaternion.identity);

// Keep track of it in our tail list
        tail.Insert(0, g.transform);

// Reset the flag
        ate = false;
    }
    // Do we have a Tail?
    else if (tail.Count > 0) {
        // Move last Tail Element to where the Head was
        tail.Last().position = v;

// Add to front of list, remove from the back
        tail.Insert(0, tail.Last());
        tail.RemoveAt(tail.Count-1);
    }
}
```
注意：Instantiate()函数用于在Unity中生成游戏物体，第一个参数是要生成的物体，第二个参数是该物体生成的位置，第三个参数是该物体生成时的Rotation值。

现在让我们在Hierarchy面板中找到Head，然后把Assets下名为“TailPrefab”的预制体拖拽到Tail Prefab变量的卡槽中去：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEhRSzhaSVd2WmljVFhURWNtaFdoOVlhRFBwbnNLS1cwbXdLMlpqZFNFUktoRDAyQzRpYTRZRlg2ZktpYUxwSzdJWjVoZFhjYXhoeXRTR2cvMA?x-oss-process=image/format,png)
现在点击Play按钮畅玩吧~！！
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEhRSzhaSVd2WmljVFhURWNtaFdoOVlhc0o1aktkMnFVMmljbEdKa1VtWmFCeVlkcUFwS0FJd1daV3pqTjYzU1pVdmVUQVZhbXcyMVFCQS8w?x-oss-process=image/format,png)
总结
贪吃蛇是一个神奇的游戏，通过该游戏的简单制作，我们学到了好多东西，比如InvokeRepeating()函数、OnTriggerEnter2D()函数的使用、碰撞器、简单的2D物理系统等等。
