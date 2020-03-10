---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《2D光圈》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
让我们在团结中制作一个电子风格的2D游戏。两名球员将能够相互竞争。目标是移动你的光线以一种诱捕另一个玩家的方式，有点像一个多人贪吃蛇游戏。

我们的游戏将非常简单，只有不到60行代码和只有三个资产。

像往常一样，一切都会尽可能简单地解释，这样每个人都能理解它。

以下是最后游戏的预览：![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC10cm9uLWxpZ2h0Y3ljbGVzLWdhbWUvdW5pdHlfdHJvbl9saWdodGN5Y2xlcy5naWY)
## 二、项目说明
版本：Unity5.0.0f4


## 三、正文
### 1、摄像机设置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905090720643.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 2、背景图像
一个普通的黑色背景是相当无聊的，所以让我们使用我们选择的绘图工具来绘制一些我们可以用于我们的背景的网格图像：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905090744606.png)
**将图片拖入到我们的项目中Sprites文件夹中：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905090811296.png)
**修改设置：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905090820447.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注：Pixels Per Unit 2这意味着2x2像素将适合在游戏世界的一个单位。我们将使用这个值的所有纹理，因为Player Sprites将有大小为2x2像素后。其他设置只是视觉效果。我们想要使图像看起来非常清晰，没有任何压缩。*

**拖入到场景中：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905090941551.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注意：重新调整位置(0, 0, 0).*

**调整层级：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091047588.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注意：通常我们会创建一个全新的Background Sorting Layer，但对于这样一个简单的游戏，则使用Order in Layer就足够了。*

### 2、灯饰

玩家应该在他们移动的任何地方留下一个光环，所以让我们创建第一个。

**青色灯笼：**

我们将首先绘制一个仅由青色组成的2x2px图像：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091246438.png)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC10cm9uLWxpZ2h0Y3ljbGVzLWdhbWUvbGlnaHR3YWxsX2N5YW4ucG5n?x-oss-process=image/format,png)

**导入设置：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091318116.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

**拖到场景中：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091336579.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**添加物理组件：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091353742.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**制作预制体：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091412294.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**粉红灯笼：**
让我们重复上面的工作流程，以获得粉色灯塔图像：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091440506.png)
 ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC10cm9uLWxpZ2h0Y3ljbGVzLWdhbWUvbGlnaHR3YWxsX3BpbmsucG5n?x-oss-process=image/format,png)
 所以我们最终得到了另一个预制件：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091459186.png)
 ### 3、玩家
 
 现在是加入玩家的时候了。玩家应该是一个简单的白色方块，可以通过按一些键移动。玩家也会拖着灯笼到任何他去的地方。

让我们为玩家画一个白色的2x2px图像：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC10cm9uLWxpZ2h0Y3ljbGVzLWdhbWUvcGxheWVyLnBuZw?x-oss-process=image/format,png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091533115.png)
**导入设置：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091554155.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**拖入场景中：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091610852.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**修改层级：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091625609.png)
*注意：正如前面提到的，我们通常会使用排序层。然而，由于我们的游戏中只有三个元素：背景、玩家和灯光，所以我们将保持简单，只使用三个不同的元素层序价值。*

**Player添加物理组件：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091701320.png)
*注意：我们启用了IsTrigger以避免与玩家自己的“光环”发生碰撞。只要我们有IsTrigger启用后，玩家将只接收碰撞信息，而不会实际与任何东西发生冲突。这很快就有意义了。*

**添加刚体：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091727150.png)
**添加玩家移动脚本Move.cs:**

```csharp
using UnityEngine;
using System.Collections;

public class Move : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
首先，我们想知道是否按下了移动键。现在我们只想为两个玩家创建一个移动脚本，所以让我们使移动键可自定义，这样我们就可以使用箭一个玩家的键和WSAD另一个玩家的钥匙：

```csharp
using UnityEngine;
using System.Collections;

public class Move : MonoBehaviour {
    // Movement keys (customizable in Inspector)
    public KeyCode upKey;
    public KeyCode downKey;
    public KeyCode rightKey;
    public KeyCode leftKey;

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
如果我们保存脚本并查看Inspector然后，我们可以将关键变量设置为Arrow Keys：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905091826502.png)
好的，让我们检查一下我们的按键更新职能：

```csharp
// Update is called once per frame
void Update () {
    // Check for key presses
    if (Input.GetKeyDown(upKey)) {
        // Do stuff...
    }
    else if (Input.GetKeyDown(downKey)) {
        // Do stuff...
    }
    else if (Input.GetKeyDown(rightKey)) {
        // Do stuff...
    }
    else if (Input.GetKeyDown(leftKey)) {
        // Do stuff...
    }
}
```
现在，只要玩家按下其中的任何一个键，我们就想让玩家移动到那个方向。如前所述，我们将使用刚体的速度那个的财产。速度总是运动方向乘以运动速度..让我们首先添加一个运动速度变量：

```csharp
using UnityEngine;
using System.Collections;

public class Move : MonoBehaviour {
    // Movement keys (customizable in Inspector)
    public KeyCode upKey;
    public KeyCode downKey;
    public KeyCode rightKey;
    public KeyCode leftKey;

    // Movement Speed
    public float speed = 16;

    ...
}
```
剩下的就很简单了。我们要做的就是修改我们的更新函数再一次设置刚体的velocity 属性：

```csharp
// Update is called once per frame
void Update () {
    // Check for key presses
    if (Input.GetKeyDown(upKey)) {
        GetComponent<Rigidbody2D>().velocity = Vector2.up * speed;
    }
    else if (Input.GetKeyDown(downKey)) {
        GetComponent<Rigidbody2D>().velocity = -Vector2.up * speed;
    }
    else if (Input.GetKeyDown(rightKey)) {
        GetComponent<Rigidbody2D>().velocity = Vector2.right * speed;
    }
    else if (Input.GetKeyDown(leftKey)) {
        GetComponent<Rigidbody2D>().velocity = -Vector2.right * speed;
    }
}

```
*注：-向量2.向上手段降下来和-向量2.右手段左边.*

让我们也修改一下我们的启动函数非常快地给玩家一个初始速度：

```csharp
// Use this for initialization
void Start () {
    // Initial Velocity
    GetComponent<Rigidbody2D>().velocity = Vector2.up * speed;
}
```
**然后我们点击Play，用箭头可以移动我们的Player：**
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC10cm9uLWxpZ2h0Y3ljbGVzLWdhbWUvcGxheWVyX21vdmUuZ2lm)
**Player灯饰**
我们想要添加一个功能，以创建一个灯塔，无论球员去哪里。我们真正需要做的就是在玩家转变成一个新的方向后，尽快创建一个新的“光环”，然后在玩家进入另一个方向之前，始终在灯光中心进行缩放。

我们将需要一个辅助功能，产生一个新的照明大楼。首先，我们将向脚本中添加两个变量。其中一个是闪电城预制板，另一个是目前被玩家拖着的墙：

```csharp
public class Move : MonoBehaviour {
    // Movement keys (customizable in Inspector)
    public KeyCode upKey;
    public KeyCode downKey;
    public KeyCode rightKey;
    public KeyCode leftKey;

    // Movement Speed
    public float speed = 16;

    // Wall Prefab
    public GameObject wallPrefab;

    // Current Wall
    Collider2D wall;

    ...
```
现在我们可以用实例化若要创建在玩家当前位置生成新的“光环”的函数，请执行以下操作：

```csharp
void spawnWall() {
    // Spawn a new Lightwall
    GameObject g = (GameObject)Instantiate(wallPrefab, transform.position, Quaternion.identity);
    wall = g.GetComponent<Collider2D>();
}
```

*注：变换位置是玩家目前的位置四元数身份是默认的旋转。我们还保存了游戏对象Collider2D在我们墙变量来跟踪当前墙。*

让我们保存脚本，然后拖动淡青色预制件项目区在剧本里壁板插槽：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905092144552.png)
好了，是时候利用我们的助手函数了。现在我们将修改脚本的更新功能在改变方向后生成一个新的“光塔”：

```csharp
// Update is called once per frame
void Update () {
    // Check for key presses
    if (Input.GetKeyDown(upKey)) {
        GetComponent<Rigidbody2D>().velocity = Vector2.up * speed;
        spawnWall();
    }
    else if (Input.GetKeyDown(downKey)) {
        GetComponent<Rigidbody2D>().velocity = -Vector2.up * speed;
        spawnWall();
    }
    else if (Input.GetKeyDown(rightKey)) {
        GetComponent<Rigidbody2D>().velocity = Vector2.right * speed;
        spawnWall();
    }
    else if (Input.GetKeyDown(leftKey)) {
        GetComponent<Rigidbody2D>().velocity = -Vector2.right * speed;
        spawnWall();
    }
}
```
当游戏开始时，我们还会孕育出一个新的“光塔”：

```csharp
// Use this for initialization
void Start () {
    // Initial Velocity
    GetComponent<Rigidbody2D>().velocity = Vector2.up * speed;
    spawnWall();
}
```
如果我们保存脚本并按下Player，然后我们可以看到，在每个方向改变之后，一个新的光塔是如何产生的：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC10cm9uLWxpZ2h0Y3ljbGVzLWdhbWUvcGxheWVyX2xpZ2h0d2FsbF9kb3RzLmdpZg)
到现在为止还好。

现在灯台只是个小方块，我们还得把它们放大。让我们创建一个新的适口函数，它接受对撞机和两点，然后将对撞机放在这两个点之间：

```csharp
void fitColliderBetween(Collider2D co, Vector2 a, Vector2 b) {
    // Calculate the Center Position
    co.transform.position = a + (b - a) * 0.5f;

    // Scale it (horizontally or vertically)
    float dist = Vector2.Distance(a, b);
    if (a.x != b.x)
        co.transform.localScale = new Vector2(dist, 1);
    else
        co.transform.localScale = new Vector2(1, dist);
}
```
*注意：这个函数一开始看起来有点混乱。在两点之间安装对撞机的明显方法是对撞机.setMinMax()但团结组织不允许这样做。相反，我们将简单地使用变换位置属性将其精确定位在两个点之间，然后使用地区规模属性使其变得非常长，因此它正好适合于点之间的位置。公式A+(b-a)*0.5也很容易理解。首先，我们从a到b用(b-a)..然后我们简单地把这个方向的一半加到这个点上。a，这就产生了中心点。之后，我们通过比较这两条线，找出这条线应该是水平的还是垂直的。x坐标。如果它们相等，则这条线是水平的，否则是垂直的。最后，我们调整比例，使墙是区单位长和1单位范围。*

让我们利用我们的适口功能。我们总是想要适应对撞机之间的最后一个对撞机结束和球员的当前位置。所以首先，我们必须跟踪最后一架对撞机的尾声。

我们将添加一个lastWallEnd变量到我们的脚本中：

```csharp
public class Move : MonoBehaviour {
    // Movement keys (customizable in Inspector)
    public KeyCode upKey;
    public KeyCode downKey;
    public KeyCode rightKey;
    public KeyCode leftKey;

    // Movement Speed
    public float speed = 16;

    // Wall Prefab
    public GameObject wallPrefab;

    // Current Wall
    Collider2D wall;

    // Last Wall's End
    Vector2 lastWallEnd;

    ...
```
并在我们的spawnWall函数：

```csharp
void spawnWall() {
    // Save last wall's position
    lastWallEnd = transform.position;

    // Spawn a new Lightwall
    GameObject g = (GameObject)Instantiate(wallPrefab, transform.position, Quaternion.identity);
    wall = g.GetComponent<Collider2D>();
}
```
*注意：从技术上讲，最后一道墙的位置应该是变位，但我们用了玩家的变换位置这里。原因是在第一堵墙产卵的时候，还没有最后一堵墙，所以我们不能设置最后沃伦德就位。相反，我们总是把它设置在球员的当前位置，然后再打下一道墙，结果几乎是一样的。*

快好了。现在我们可以修改我们的更新再次作用，以始终适合当前墙之间的最后一面墙的结束位置和球员的当前位置：

```csharp
// Update is called once per frame
void Update () {
    // Check for key presses
    if (Input.GetKeyDown(upKey)) {
        GetComponent<Rigidbody2D>().velocity = Vector2.up * speed;
        spawnWall();
    }
    else if (Input.GetKeyDown(downKey)) {
        GetComponent<Rigidbody2D>().velocity = -Vector2.up * speed;
        spawnWall();
    }
    else if (Input.GetKeyDown(rightKey)) {
        GetComponent<Rigidbody2D>().velocity = Vector2.right * speed;
        spawnWall();
    }
    else if (Input.GetKeyDown(leftKey)) {
        GetComponent<Rigidbody2D>().velocity = -Vector2.right * speed;
        spawnWall();
    }

    fitColliderBetween(wall, lastWallEnd, transform.position);
}
```
如果我们保存脚本并按下Player，然后我们就可以看到在播放器后面是如何创建的：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC10cm9uLWxpZ2h0Y3ljbGVzLWdhbWUvcGxheWVyX2xpZ2h0d2FsbF93YWxscy5naWY)
如果我们仔细看一看，我们就能看到墙角上的墙少了一点：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905092611978.png)
我们的问题有一个很容易解决的办法。我们要做的就是回到我们的适口功能和始终使墙壁一个单位长：

```csharp
void fitColliderBetween(Collider2D co, Vector2 a, Vector2 b) {
    // Calculate the Center Position
    co.transform.position = a + (b - a) * 0.5f;

    // Scale it (horizontally or vertically)
    float dist = Vector2.Distance(a, b);
    if (a.x != b.x)
        co.transform.localScale = new Vector2(dist + 1, 1);
    else
        co.transform.localScale = new Vector2(1, dist + 1);
}
```
如果我们保存脚本并按下Play，踢然后我们可以看到一些完全匹配的角：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC10cm9uLWxpZ2h0Y3ljbGVzLWdhbWUvbGlnaHR3YWxsX2Nvcm5lcl9leGFjdC5wbmc?x-oss-process=image/format,png)
### 4、加入另一个玩家
点击玩家，右键点击Duplicate复制一个对象，或者Ctrl+C Ctrl+D也行：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905092701615.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们将把复制的Player重命名为player_pink并将其坐标改为(-3, 0, 0):
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905092811311.png)
我们还可以将移动键更改为WSAD并拖动淡粉色预制件墙体预制件插槽：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190905092822623.png)
如果我们按下Play，然后，我们现在可以控制两个球员，一个与WSAD钥匙和一个带Arrow keys：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC10cm9uLWxpZ2h0Y3ljbGVzLWdhbWUvdW5pdHlfdHJvbl9saWdodGN5Y2xlcy5naWY)

### 5、碰撞检测
好的，让我们添加一个输掉我们比赛的条件。一名球员一进墙就会输掉这场比赛。

我们已经添加了物理成分(对撞机和刚体)我们现在要做的就是增加一个新的OnTriggerEnter2D对我们移动剧本。如果玩家与某物发生碰撞，统一将自动调用此函数：

```csharp
void OnTriggerEnter2D(Collider2D co) {
    // Do Stuff...
}
```

“Collider2D Co参数是玩家与之碰撞的对撞机。让我们确保这个对撞机不是玩家目前拖在身后的墙：

```csharp
void OnTriggerEnter2D(Collider2D co) {
    // Not the current wall?
    if (co != wall) {
        // Do Stuff...
    }
}
```

在这种情况下，它必须是任何其他墙壁，这意味着玩家输了游戏。我们只会在这里保持简单毁灭玩家：

```csharp
void OnTriggerEnter2D(Collider2D co) {
    // Not the current wall?
    if (co != wall) {
        print("Player lost:" + name);
        Destroy(gameObject);
    }
}
```

*注意：在这一点上可以随意添加一些输赢的屏幕。*

### 6、内容扩展
我们刚刚在Unity中创建了一个电子风格的光循环2D游戏。和往常一样，大多数功能都很容易实现-多亏了这个强大的游戏引擎。这款游戏提供了很多潜力，还有各种各样的功能可以添加：
- 输赢屏幕
- 2名以上玩家
- 在线多人
- AI
- 一些特效
- 更好的Sprites等等
和往常一样，现在该由读者来让游戏变得有趣了。
