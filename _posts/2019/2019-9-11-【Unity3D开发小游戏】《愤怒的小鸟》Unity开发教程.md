---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《愤怒的小鸟》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

@[TOC](《愤怒的小鸟》开发教程)
## 一、前言
“愤怒的小鸟”在2009年12月发布，由于它的高度上瘾的游戏，它很快成为有史以来最成功的移动游戏。

在本教程中，我们将在“Unity”中实现“愤怒的小鸟”翻版。游戏中最复杂的部分是物理系统，但是多亏了Unity，我们就不用担心太多了。使用Unity将使它如此容易，我们将只需要大约100行的代码！



像往常一样，一切都会尽可能简单地解释，这样每个人都能理解它。

以下是项目的==预览==：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3VuaXR5X2FuZ3J5YmlyZHMuZ2lm)
## 二、项目版本
==Unity5.0.0f4==
*[Unity5.0.0f4]:   最好也是使用这个版本，不然可能会出现各种各样的问题，比如有些API不能使用，或者预制体的制作等等操作的问题
## 三、正文
### 1.设置相机
点击Main Cameras，在==Hierarchy面板==设置背景色以友好的蓝色色调(红色=187, 绿色=238, 蓝色=255)并调整大小而位置如下图所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091109215659.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

### 2.地面设置
**地面贴图设置**
为了防止版权问题，我们不能在本教程中使用原“愤怒的小鸟”图形。相反，我们将画我们自己的Sprite，使他们看起来像原来的游戏。

让我们从用我们选择的绘图工具开始：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2dyb3VuZC5wbmc?x-oss-process=image/format,png)
将其保存到我们的项目中后，我们可以在项目区可以看到:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911092258182.png)
然后在==Inspector==修改导入设置:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911092315930.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
==注：Pixels Per Unit像素转到单位价值16这意味着16x16像素将适合在游戏世界的一个单位。我们将使用这个值作为我们所有的纹理。我们选择16，因为鸟的大小将有一个16x16像素后，我们希望在游戏世界它有一个单位的大小。==

好了，现在我们可以将图片从项目区拖入到场景中:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911092346383.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
让我们看看==Inspector==把地面定位在(0, -2)，所以作为不为y=0的都不是地面的一部分：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911092404503.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**地面物体设置**
现在地面只是一幅图像，仅此而已。它不是物理世界的一部分，事物不会与它相撞，也不会站在它上面。我们需要添加一个Collider让它成为物理世界的一部分，这意味着事物将能够站在它的顶端，而不是掉进它的正中。


添加BoxCollider2D组件：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911092541814.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

### 3.边界设置
创建空对象，命名为borders
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911092619668.png)
位置归零：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911092630753.png)
现在，我们将在我们的水平的左边、右边和顶部添加某种不可见的区域。每当有东西进入那个区域，它就应该被摧毁。此类行为可以通过Trigger，这几乎只是一个Trigger它接收到碰撞信息，但不会与任何东西发生冲突。

添加碰撞器：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911092748277.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
勾选
- [x] Is Trigger

之后，我们可以为级别的右侧和顶部再添加两个triggers ：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911092822626.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
如果我们看看场景然后，我们可以看到触发器是如何与我们的背景很好地对齐的：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911092836971.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在我们仍然必须确保任何进入边界的东西都会立即被销毁。此类行为可以通过脚本Borders：

创建脚本Borders.cs:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911092912725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
将其添加到边界对象物体上面：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911092921870.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
让我们也将脚本移动到一个新的Scripts文件夹，只是为了保持清洁：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911093019582.png)
编辑Borders.cs脚本：

```csharp
using UnityEngine;
using System.Collections;

public class Borders : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
我们不需要启动或者更新函数，所以让我们移除它们。相反，我们将使用OnTriggerEnter2D函数，每当有东西进入其中一个边界触发器时，统一将自动调用该函数：

```csharp
using UnityEngine;
using System.Collections;

public class Borders : MonoBehaviour {

    void OnTriggerEnter2D(Collider2D co) {

    }
}
```
在这个函数中，无论什么东西进入Triggers，我们都将Destroy这个物体：

```csharp
using UnityEngine;
using System.Collections;

public class Borders : MonoBehaviour {

    void OnTriggerEnter2D(Collider2D co) {
        Destroy(co.gameObject);
    }
}
```
保存脚本后，我们的边界就完成了。我们稍后会看到，如果我们试图将一只鸟射出水平之外，它就会消失。

### 4.云彩设置
我们将花几分钟额外添加云到背景，以使水平看起来更好。像往常一样，我们首先画一个：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2Nsb3VkLnBuZw?x-oss-process=image/format,png)
让我们在项目区，然后在==Inspector==修改云的导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911093227165.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在我们要做的就是把它从项目区进入场景几次，将每一片云放置在我们想要的位置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911093240133.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注意：只要使用一些重复的模式和一些非常颜色，我们可以使水平看起来相当好，无需付出很大的努力。

### 5.弹弓设计
**弹弓图片**
一个飞弹将产生新的鸟类，并允许用户发射到水平。和往常一样，我们将从画Sprites开始：
这里是导入设置:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911093335609.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
稍后，我们将创建一个脚本，在弹弓的位置生成一只新的鸟，或者确切地说是在弹弓的Pivot位置生成一只鸟。
我们想要在弹弓顶部而不是中间处出现，这就是为什么我们要在“导入设置”中设置Pivot在顶部。

下面的图像显示了中心和顶:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911093436891.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注意：如果我们将数据透视设置为中心然后变换位置是弹弓中心的点。如果我们把Pivot 设为顶，然后变换位置是弹弓顶端的点。

好了，现在我们可以将弹弓拖到场景中去了(-22, 3):

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911093506404.png)
**生成鸟脚本**
如前所述，我们的弹弓应该是生成鸟。确切地说，它应该在一开始就生成一个，然后等待用户启动它，然后在所有的物理计算完成之后再生成另一个。(当什么都不动的时候).

我们可以通过脚本来实现这样的行为。
添加脚本Spawn.cs:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911093603690.png)
我们可以双击脚本来打开它：

```csharp
using UnityEngine;
using System.Collections;

public class Spawn : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
这个启动函数在开始游戏时由Unity自动调用。这个更新函数被一次又一次地自动调用，大约每秒60次。我们不需要它们中的任何一个，这样我们就可以从脚本中删除它们。

还有另一种类型的更新函数，它被称为FixedUpdate..它也被一次又一次的调用，但是是在单位物理完全相同的时间间隔内计算的，所以在做物理工作的时候使用FixedUpdate是一个好主意(我们很快就会这么做).

下面是修改后的脚本FixedUpdate脚本：

```csharp
using UnityEngine;
using System.Collections;

public class Spawn : MonoBehaviour {

    void FixedUpdate() {

    }
}
```
好的，让我们添加一个变量，允许我们稍后指定BirdPrefab(我们想生的鸟):

```csharp
using UnityEngine;
using System.Collections;

public class Spawn : MonoBehaviour {
    // Bird Prefab that will be spawned
    public GameObject birdPrefab;

    void FixedUpdate() {

    }
}
```
以下是我们如何生成它的方法：

```csharp
void spawnNext() {
    // Spawn a Bird at current position with default rotation
    Instantiate(birdPrefab,transform.position,Quaternion.identity);
}
```
**生成的触发区域**

现在我们不能只生一只又一只鸟。相反，我们将不得不生成一个，然后等待它被发射。有几种方法可以实现这一点，但最简单的方法是使用Triggers.

Trigger是一个简单的Collider ，接收碰撞信息，但实际上并不是物理世界的一部分。所以如果我们添加一个Trigger然后，每当有东西进入Trigger、留在Trigger中或离开Trigger时，我们都会收到通知。然而，由于它只是一个Trigger，事情不会像普通Collider 那样与它相撞(这很快就更有意义了).

我们可以将Trigger添加到弹弓中，方法是在Hierarchy面板中，然后点击添加组件Circle Collider 2D，给它一个合适的半径和中心然后启用触发:
- [x] Is Trigger
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911093943385.png)
我们还可以在场景中查看:
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091109395477.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
在添加触发器之后，每当有东西进入时，我们都会收到通知。(OnTriggerEnter2D)，停留(OnTriggerStay2D)或离开(OnTriggerExit2D)上面那个绿色的圆圈。

现在，我们可以通过创建一个使用中变量，然后将其设置为bool值，当生下一只鸟的时候为false，当它离开触发器时为true：

```csharp
using UnityEngine;
using System.Collections;

public class Spawn : MonoBehaviour {
    // Bird Prefab that will be spawned
    public GameObject birdPrefab;

    // Is there a Bird in the Trigger Area?
    bool occupied = false;

    void FixedUpdate() {

    }

    void spawnNext() {
        // Spawn a Bird at current position with default rotation
        Instantiate(birdPrefab, transform.position, Quaternion.identity);
        occupied = true;
    }

    void OnTriggerExit2D(Collider2D co) {
        // Bird left the Spawn
        occupied = false;
    }
}
```
之后我们可以修改我们的FixedUpdate函数，因此每当触发区域不再被占用时，它总是生成一只鸟：

```csharp
void FixedUpdate() {
    // Bird not in Trigger Area anymore?
    if (!occupied)
        spawnNext();
}
```
*注：！occupied意思是还没有被占用..我们也可以用if(occupied==false).

我们的生成脚本现在可以正常工作了，但是让我们再添加一个特性。在射杀一只鸟之后，会有很多东西相互碰撞，坠落，滚来滚去，甚至爆炸。在最初的“愤怒的小鸟”游戏中，只有在水平上的所有东西停止移动之后，才会产生一只新的鸟。

我们可以很容易地创建一个sceneMoving函数，该函数查找场景中是否有任何对象仍在移动，而不仅仅是一点点：

```csharp
bool sceneMoving() {
    // Find all Rigidbodies, see if any is still moving a lot
    Rigidbody2D[] bodies = FindObjectsOfType(typeof(Rigidbody2D)) as Rigidbody2D[];
    foreach (Rigidbody2D rb in bodies)
        if (rb.velocity.sqrMagnitude > 5)
            return true;
    return false;
}
```
==注意：我们使用了FindObjectsOfType找到所有带有刚体的物体，之后我们会检查每个物体的velocity，如果这个刚体的sqrMagnitude大于5，说明这个刚体还在移动就返回True，没有就返回false==


使用这个整洁的小脚本，我们可以轻松地修改FixedUpdate功能，因此只有在没有任何移动的情况下才会产生新的鸟：

```csharp
void FixedUpdate() {
    // Bird not in Trigger Area anymore? And nothing is moving?
    if (!occupied && !sceneMoving())
        spawnNext();
}
```
现在我们已经完成了生成鸟的脚本，我们可以在==Inspector面板==看到弹弓上面挂载的脚本：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091109451834.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

注意：我们还不能在没有鸟的情况下测试生成鸟脚本，但是它确实工作得很好，我们将在创建鸟之后看到这一点。

### 6.鸟的设置
**鸟的图片**
让我们开始更有趣的事情：鸟。我们首先画一个16 x 16一只大圆身躯和一些小小的翅膀和眼睛的鸟的像素图像：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2JpcmQucG5n?x-oss-process=image/format,png)
我们将使用以下方法导入设置为此：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911094646587.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
让我们从项目区进入场景若要从其中创建游戏对象，请执行以下操作：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911094657618.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**鸟的物理**

让我们为鸟添加碰撞器Circe Collider 2D：

![在这里插入图片描述](https://img-blog.csdnimg.cn/201909110947471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在有一个Physics Material 2D对撞机的缝隙，让我们可以给鸟一些特殊的物理特性。在“Unity愤怒的小鸟”教程中，物理材料将是非常有用的，因为现在，如果这只鸟掉在地上，它看起来会是这样的：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2JpcmRfaGl0c2dyb3VuZF9ub21hdGVyaWFsLmdpZg)
看起来有点不自然。相反，我们想让这只鸟从下面的东西中跳出来：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2JpcmRfaGl0c2dyb3VuZF93aXRobWF0ZXJpYWwuZ2lm)
要在第二张图片中创建弹跳效果，我们所要做的就是在项目区并选择Create>Physics2D Material，说出来鸟类材料把它变成一个新的物理材料文件夹：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911095145712.png)
一旦被选中，我们就可以修改==Inspector:==
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911095205401.png)
==注：Bounciness值越大，鸟就越会反弹。==

最后，我们可以再次选择鸟，然后拖动鸟类材料从项目区进入Collider Material插槽：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911095243908.png)
这只鸟也应该四处走动。刚体负责物体的重力、速度和其他使物体运动的力。根据经验法则，在物理世界里，所有应该移动的东西都需要一个刚体.:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911095306690.png)
==注意：我们设置了Gravity Scale到4因为它能让鸟飞得更快。==

如果我们按下Play现在我们可以看到鸟从地上掉下来并弹跳起来：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2JpcmRfaGl0c2dyb3VuZF93aXRobWF0ZXJpYWwuZ2lm)
我们的鸟类物理已经完成了，但是有一个小的调整是我们必须在这里进行的。现在，如果我们在弹弓中生成的话，由于它的刚体引力，它会立即坠落到地面。我们只希望用户一开火，鸟就会受到重力的影响，所以让我们现在启用Is Kinematic，然后在脚本中禁用它：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911095346273.png)
现在，刚体是运动学的，这意味着它不受重力或速度的影响，因此不会立即坠落。

==注意：为了更清楚地说明这一点，任何像英雄、汽车或鸟之类的东西都应该有一个刚体，它是运动学的。我们只使能只要鸟还在弹弓里就能运动。==

**鸟预制体**
如前所述，这只鸟从一开始就不应该出现在场景中。相反，弹弓应该在需要的时候生成出一只新的鸟。为了使弹弓能够生成鸟，我们必须创建一个预制件 (换句话说，我们必须在我们的项目区有鸟的资源).

要创建预制件，我们所要做的就是在==hierarchy面板==，将物体拖入到项目区的Prefabs文件夹中：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911095420322.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在，我们可以在任何时候将鸟加载到场景中，这意味着我们现在也可以从==Hierarchy==中删除这个对象：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911095435567.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**生成鸟**
让我们将预制体bird拖到到我们的Spawn.cs的脚本中的BirdPrefab插槽中：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911095559309.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
如果我们按下Play现在我们可以看到弹弓是如何生出一只鸟的：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911095615428.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

**拉and释放脚本**
用户应该能够把鸟在弹弓周围，然后释放它，以便把它射向所希望的方向。

我们将创造一个新的C#脚本给它起个名字PullAndRelease :

```csharp
using UnityEngine;
using System.Collections;

public class PullAndRelease : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
用户将能够拖动鸟绕一个圆圈。每个圆都需要一个中心，在我们的例子中，它是鸟的生成位置，所以让我们确保将它保存在一个变量中：

```csharp
using UnityEngine;
using System.Collections;

public class PullAndRelease : MonoBehaviour {
    // The default Position
    Vector2 startPos;

    // Use this for initialization
    void Start () {
        startPos = transform.position;
    }
}
```

*注意：我们还删除了更新函数因为我们不需要它。

好的，为了让用户把鸟拖曳成一个圆圈，我们必须找出这只鸟是否被点击了。(确切地说：拖着)..我们还需要知道用户是否释放了鼠标，在这种情况下，我们必须在我们目标方向发射鸟。

当然，如果没有这方面的功能，那就不是Unity了。Unity自动调用Onmouseup和OnmouseDrag函数，当我们用鼠标拖动游戏对象或随后释放鼠标时：

```csharp
using UnityEngine;
using System.Collections;

public class PullAndRelease : MonoBehaviour {
    // The default Position
    Vector2 startPos;

    // Use this for initialization
    void Start () {
        startPos = transform.position;
    }

    void OnMouseUp() {
        // ToDo: fire the Bird
    }

    void OnMouseDrag() {
        // ToDo: move the Bird
    }
}
```
*注：鼠标拖动，意思是用户在GameObject上按住鼠标按钮，然后移动鼠标。


移动鸟真的很容易。我们所要做的就是将当前的鼠标位置转换到游戏世界的某个点，然后将鸟移到那里。当然，只有在一定半径内：

```csharp
void OnMouseDrag() {
    // Convert mouse position to world position
    Vector2 p= Camera.main.ScreenToWorldPoint(Input.mousePosition);

    // Keep it in a certain radius
    float radius = 1.8f;
    Vector2 dir = p - startPos;
    if (dir.sqrMagnitude > radius)
        dir = dir.normalized * radius;

    // Set the Position
    transform.position = startPos + dir;
}
```
*注意：我们可以使用ScreenToWorldPoint获取到手指点击的位置，但是这个位置是不固定的，在找到手指点击的位置p之后，我们只需要计算从startPos到p的距离，如果这个距离太长dir.sqrMagnitude > radius，就让这个位置等于一个最大值dir = dir.normalized * radius


如果我们按下Play然后我们可以把鸟绕个圈：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2JpcmRfcHVsbC5naWY)
把鸟射向一个方向也同样容易。我们可以用我们的Onmouseup函数来知道鼠标何时释放。然后，我们将计算出从鸟到startPos然后使用rigidbody的AddForce在那里启动它的功能：

```csharp
// The Force added upon release
public float force = 1300;

void OnMouseUp() {
    // Disable isKinematic
    GetComponent<Rigidbody2D>().isKinematic = false;

    // Add the Force
    Vector2 dir = startPos - (Vector2)transform.position;
    GetComponent<Rigidbody2D>().AddForce(dir * force);

    // Remove the Script (not the gameObject)
    Destroy(this);
}
```
*注意：如前所述，我们也将禁用运动学等使刚体再次受到重力和速度的影响。我们只需将当前位置减去startPos..最后，我们删除这个对象，这样它就不能再被发射了。

如果我们按下Play然后我们就可以拉着这只鸟开火了：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2JpcmRfcHVsbF9hbmRfcmVsZWFzZS5naWY)
 **Feather Particle Effect羽毛的粒子效果**
 让我们通过增加鸟的碰撞效果来使游戏更加流畅。一旦它第一次落地，它就应该像这样在自己周围随意地长出羽毛：
 ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2ZlYXRoZXJzLmdpZg)
 当我们需要随机粒子产生、旋转和向某个方向移动时，就会使用粒子系统。粒子系统的一个简单的例子是烟雾，它产生灰色纹理，然后以锥状向上移动。

我们将修改我们的粒子系统，使其不是使粒子向上飞，而是使它们飞向四面八方。我们还将修改一些更具体的东西，如大小，速度和旋转。我们的羽毛没有正确或错误的粒子系统，所以你可以随意使用它，直到它看起来像你想让它看起来那样。以下是我们得出的结论：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2ZlYXRoZXJzX3BhcnRpY2xlc3lzdGVtLnBuZw?x-oss-process=image/format,png)
这是我们用来做这件事的图像：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2ZlYXRoZXIucG5n?x-oss-process=image/format,png)
*注意：右击图像，选择另存为.。并将其保存在项目的Assets/Sprites文件夹。

导入设置：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2ZlYXRoZXJfaW1wb3J0c2V0dGluZ3MucG5n?x-oss-process=image/format,png)
之后，我们可以从项目区进入我们粒子系统所以它使用图像对所有的粒子。

现在我们可以将场景中的羽毛对象拖入到我们项目区的Prefabs文件夹，做成一个预制体：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911100456862.png)
然后我们可以在Hierarchy中删除羽毛游戏对象

最后一件事是给我们的鸟添加一个脚本，这样羽毛粒子系统就会在发生碰撞时产生。让我们在项目区然后创建新脚本..我们给它起个名字CollisionSpawnOnce。我们也会把它移到我们的Sprits文件夹，然后双击它以打开它：

```csharp
using UnityEngine;
using System.Collections;

public class CollisionSpawnOnce : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
我们不需要启动或更新函数。相反，我们将使用OnCollisionEnter2D函数和effect公共游戏对象应生成的预制件的变量：

```csharp
using UnityEngine;
using System.Collections;

public class CollisionSpawnOnce : MonoBehaviour {
    // Whatever should be spawned (Particles etc.)
    public GameObject effect;

    void OnCollisionEnter2D(Collision2D coll) {
        // Spawn Effect, then remove Script
        Instantiate(effect,transform.position,Quaternion.identity);
        Destroy(this);
    }
}
```

*注意：为了确保只产生一次效果，我们将从Destroy(this)(这只会关闭脚本，而不是整只鸟)。

保存脚本后，我们可以看到Effect插槽..现在我们可以拖着羽毛粒子系统预制件项目区进入Effect插槽：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911100651760.png)
如果我们按下Play然后把这只鸟发射到地上，然后我们就可以看到它周围的羽毛在生成：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2ZlYXRoZXJzLmdpZg)
**路径**
我们还会给我们的鸟添加另一个效果，让它看起来更流畅：一条白点的轨迹，显示鸟的轨迹：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3RyYWlsLmdpZg)
首先，我们需要一些大小不同的跟踪图像：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3RyYWlsX3NtYWxsLnBuZw?x-oss-process=image/format,png)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3RyYWlsX21lZGl1bS5wbmc?x-oss-process=image/format,png)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3RyYWlsX2JpZy5wbmc?x-oss-process=image/format,png)
我们会用同样的导入设置对于每一幅图像：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911100907887.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们希望能够在我们想要的任何时候产生轨迹部分，这意味着我们将需要三个预制件。因此，让我们选择三个图像并将其拖到场景中，然后拖回到Prefabs文件夹。直到我们有三个预制体：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911100919126.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在我们只需要一个脚本来生成一个又一个的TRAIL元素，大约每秒钟一次。让我们创建一个新的C#脚本给它起个名字Trail ：

```csharp
using UnityEngine;
using System.Collections;

public class Trail : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
我们可以移除更新因为我们不需要它。让我们添加一个public GameObject[]保存所有跟踪元素的变量。我们将使用Array，这意味着它不仅仅是一个GameObject：

```csharp
using UnityEngine;
using System.Collections;

public class Trail : MonoBehaviour {

    // Trail Prefabs
    public GameObject[] trails;

    // Use this for initialization
    void Start () {

    }
}
```
我们还需要一个函数来生成下一条线索。例如，它应该产生第一个TRAIL元素，然后下一次应该生成第二个，然后是第三个，然后是第一个。这可以通过使用实例化有一个额外的计数器变量：

```csharp
using UnityEngine;
using System.Collections;

public class Trail : MonoBehaviour {

    // Trail Prefabs
    public GameObject[] trails;
    int next = 0;

    // Use this for initialization
    void Start () {

    }

    void spawnTrail() {
        Instantiate(trails[next], transform.position, Quaternion.identity);
        next = (next+1) % trails.Length;
    }
}
```
我们将其设置为0，这意味着trails spawnTrail中的第一个元素被调用。然后使用next+1来增加next。为了保持它在trails数组的范围内，我们还将使用% trails.Length，它使用模(%)运算。对于那些不了解模的人，这里有一个更明显的版本:

```csharp
void spawnTrail() {
    Instantiate(trails[next], transform.position, Quaternion.identity);
    next = next + 1;
    if (next == trails.Length) next = 0;
}
```

现在我们有了一个生成轨迹函数，我们可以使用它生成一个新的trail 元素通过使用InvokeRepeting函数100 ms生成一个：

```csharp
using UnityEngine;
using System.Collections;

public class Trail : MonoBehaviour {

    // Trail Prefabs
    public GameObject[] trails;
    int next = 0;

    // Use this for initialization
    void Start () {
        // Spawn a new Trail every 100 ms
        InvokeRepeating("spawnTrail", 0.1f, 0.1f);
    }

    void spawnTrail() {
        Instantiate(trails[next], transform.position, Quaternion.identity);
        next = (next+1) % trails.Length;
    }
}
```
现在，小径元素会一直生成，甚至当鸟不飞的时候也是如此。让我们添加一个小小的修改，只在鸟飞得足够快的情况下才会产生轨迹：

```csharp
void spawnTrail() {
    // Spawn Trail if moving fast enough
    if (GetComponent<Rigidbody2D>().velocity.sqrMagnitude > 25) {
        Instantiate(trails[next], transform.position, Quaternion.identity);
        next = (next+1) % trails.Length;
    }
}
```

好的，让我们保存脚本。在这里，我们将从我们的三条小径预制体中拖到插槽中:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911101353999.png)
如果我们按下Play然后我们就可以看到这只鸟射击后的踪迹：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3RyYWlsLmdpZg)
### 7.木片
让我们添加一些结构，如石头，冰和木材，我们的Unity2D愤怒的小鸟游戏更加丰富。

我们先画一块木片：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3dvb2RfYmlnLnBuZw?x-oss-process=image/format,png)
注意：右击图像，选择另存为。并将其保存在项目的Assetes/Sprits文件夹。

这里是导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911101444378.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在我们可以把它拖到场景把它放在地上的某个地方：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911101456235.png)
木片应该是物理世界的一部分，所以我们将一如既往地在添加Box Collider 2D组件:
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091110153866.png)
木片也应该能够四处移动。现在它不会自己移动，但是如果鸟飞进它，它就会移动。它也应该受到重力的影响，所以我们需要的是刚体..我们可以通过选择添加组件->物理二维->Rigidbody 2D..我们也会增加Mass到4所以它更重了一点：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911101551655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在我们有一块木头，它是物理世界的一部分！

对于这个略有不同的木片，我们将重复相同的工作流程：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3dvb2RfbG9uZy5wbmc?x-oss-process=image/format,png)
这是我们的游戏如何看待添加第二块木材和旋转第一个90°：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911101608642.png)
### 8.石头
为了在我们的游戏中有几种不同的结构，我们还将添加两种不同类型的石头：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3N0b25lX2JpZy5wbmc?x-oss-process=image/format,png)![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3N0b25lX3NtYWxsLnBuZw?x-oss-process=image/format,png)
操作流程和以前一样，这次我们将Mass设置为10：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911101641871.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
下面是我们的游戏中有一些石头的样子：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911101651440.png)
注：我们再次实现了一些体面的外观与基本的形状，只有少数颜色和抖动。

### 9.冰
**冰的图片**
我们将为我们的游戏增加一个结构：冰。不过，这一次会更有趣一些。

像往常一样，我们首先画一块冰：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2ljZS5wbmc?x-oss-process=image/format,png)
这个导入设置与以往相同：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2ljZV9pbXBvcnRzZXR0aW5ncy5wbmc?x-oss-process=image/format,png)
**冰物理**
添加 Boxcollider2D组件 刚体组件：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911101816140.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
冰应该很滑，所以让我们右击项目区并选择创造->物理二维材料给它起个名字冰IceMaterial:
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2ljZV9tYXRlcmlhbF9wcm9qZWN0YXJlYS5wbmc?x-oss-process=image/format,png)
设置参数：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091110184890.png)
之后，我们可以在Hierarchy面板中选择冰然后将IceMaterial从项目区拖入到BoxCollider2D>Material插槽：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911101858664.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**撞击时摧毁冰**
如果被足够的力量撞击，我们也希望冰层被摧毁，因为这就是冰的自然作用。我们添加脚本BreakOnImpact.cs脚本：

```csharp
using UnityEngine;
using System.Collections;

public class BreakOnImpact : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
我们不需要启动或者更新函数，所以让我们移除这两个函数。我们需要一种方法来估计碰撞的力量。我们将保持简单，并将速度与质量相乘：

```csharp
float collisionForce(Collision2D coll) {
    // Estimate a collision's force (speed * mass)
    float speed = coll.relativeVelocity.sqrMagnitude;
    if (coll.collider.GetComponent<Rigidbody2D>())
        return speed * coll.collider.GetComponent<Rigidbody2D>().mass;
    return speed;
}
```
*注：Collision2D继承OnCollisionEnter2D功能，我们将获取一个估计碰撞的力量，它包含方向与速度相乘。如果我们只关心速度，那么我们可以使用coll.relativeVelocity.sqrMagnitude..现在，如果造成碰撞的对象有刚体然后我们把速度乘以刚体的质量..否则我们只返回速度。

好了，现在我们可以用OnCollisionEnter2D函数以获得有关碰撞的通知。然后，我们将比较碰撞的力量和一个可配置的变量。如果它比力大，那么冰就会破裂：

```csharp
using UnityEngine;
using System.Collections;

public class BreakOnImpact : MonoBehaviour {
    public float forceNeeded = 1000;

    float collisionForce(Collision2D coll) {
        // Estimate a collision's force (speed * mass)
        float speed = coll.relativeVelocity.sqrMagnitude;
        if (coll.collider.GetComponent<Rigidbody2D>())
            return speed * coll.collider.GetComponent<Rigidbody2D>().mass;
        return speed;
    }

    void OnCollisionEnter2D(Collision2D coll) {
        if (collisionForce(coll) >= forceNeeded)
            Destroy(gameObject);
    }
}
```
如果我们保存脚本，请按Play把鸟碰撞冰层，然后它就会破裂：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL2ljZV9icmVha2luZy5naWY)
现在，我们可以花几分钟来复制这些结构，并将它们放在一起，这样我们就可以在它们之间添加猪了：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911102157169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 10.绿猪
鸟儿想要消灭所有的猪，所以让我们把一些猪加入到我们的游戏中，这样鸟儿就不会觉得无聊了。

我们首先画一个：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3BpZy5wbmc?x-oss-process=image/format,png)
导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911102231110.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
添加碰撞器和刚体：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911102247187.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
如果有足够大的力量袭击猪，猪就会死。幸运的是，我们已经有了一个脚本。给我们的pig物体添加脚本BreakOnImpact.cs，并且设置Force Needed的值：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911102337923.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注意：能够重用这样的脚本是基于组件的开发。

现在我们可以复制这头猪并把它移到一些结构之间：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911102404391.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
如果我们按下Play然后我们就可以试着消灭猪了：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3BpZ3NfaW5nYW1lLmdpZg)
### 11.橡胶
我们将为我们的游戏添加最后一个功能：弹弓橡胶，所以拖拽和释放鸟看起来要好得多：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3J1YmJlcl9pbmdhbWUuZ2lm)
我们首先画一半的橡胶，这几乎只是一条粗线：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3J1YmJlci5wbmc?x-oss-process=image/format,png)
这里是导入设置:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911102622155.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注意：这次我们设置了Pivot到右（边），正确的让一些旋转变得更容易。

现在我们可以从项目区进入场景两次，说出其中一个左橡胶，另一个右橡胶把它们放在弹弓的上部：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911102643568.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们要确保左边的那个总是画出来的。后面弹弓和右弹弓总是被拉进去的。前面其中的一部分。我们可以再加两个分类层对于我们的游戏，但我们将保持简单，只需更改层序到-1左边的橡胶和1右边的橡胶：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911102658946.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911102704221.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在橡胶看起来就像在外弹弓：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911102734869.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
在开始编写脚本之前，让我们在Hierarchy然后将这两个对象设置为slingshot的子对象：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3J1YmJlcl9jaGlsZF9vZl9zbGluZ3Nob3QucG5n?x-oss-process=image/format,png)
*注意：每当我们移动弹弓时，橡胶部件就会随之移动。

让我们创建一个新的C#脚本给它起个名字Rubber:

```csharp
using UnityEngine;
using System.Collections;

public class Rubber : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
这个脚本的目的是让两个橡胶部分跟随鸟，直到它离开生成鸟的触发圈。

我们需要两个变量leftRubber 和rightRubber，让我们在以后指定橡胶。我们不需要启动或者更新函数，让我们删除掉它们：

```csharp
using UnityEngine;
using System.Collections;

public class Rubber : MonoBehaviour {
    // The Rubber objects
    public Transform leftRubber;
    public Transform rightRubber;
}
```

现在，有一些稍微复杂一些的功能。我们将需要一个功能，定位橡胶在弹弓和鸟的位置，我们必须先将橡胶旋转到鸟的方向，然后根据鸟的距离使橡胶变成或变短：

```csharp
void adjustRubber(Transform bird, Transform rubber) {
    // Rotation
    Vector2 dir = rubber.position - bird.position;
    float angle = Mathf.Atan2(dir.y, dir.x) * Mathf.Rad2Deg;
    rubber.rotation = Quaternion.AngleAxis(angle, Vector3.forward);

    // Length
    float dist = Vector3.Distance(bird.position, rubber.position);
    dist += bird.GetComponent<Collider2D>().bounds.extents.x;
    rubber.localScale = new Vector2(dist, 1);
}
```

*注意：首先我们计算从鸟到橡胶的距离，然后计算这个方向的角度。然后我们，通过Quaternion.AngleAxis(angle, Vector3.forward)将橡胶旋转到这个角度。最后，我们计算从鸟到橡胶的距离，之后，我们将橡胶的缩放设置为这个距离加上碰撞器的x的长度既dist += bird.GetComponent<Collider2D>().bounds.extents.x

这个OnTriggerStay2D功能将通知我们，每当鸟改变它的位置时，它还在弹弓。我们可以使用这个函数来调整左右橡皮筋：

```csharp
using UnityEngine;
using System.Collections;

public class Rubber : MonoBehaviour {
    // The Rubber objects
    public Transform leftRubber;
    public Transform rightRubber;

    void adjustRubber(Transform bird, Transform rubber) {
        // Rotation
        Vector2 dir = rubber.position - bird.position;
        float angle = Mathf.Atan2(dir.y, dir.x) * Mathf.Rad2Deg;
        rubber.rotation = Quaternion.AngleAxis(angle, Vector3.forward);

        // Length
        float dist = Vector3.Distance(bird.position, rubber.position);
        dist += bird.GetComponent<Collider2D>().bounds.extents.x;
        rubber.localScale = new Vector2(dist, 1);
    }

    void OnTriggerStay2D(Collider2D coll) {
        // Stretch the Rubber between bird and slingshot
        adjustRubber(coll.transform, leftRubber);
        adjustRubber(coll.transform, rightRubber);
    }
}
```

快好了。我们将再增加一个离开时候触发的事件，使橡皮筋在发射后变短：

```csharp
void OnTriggerExit2D(Collider2D coll) {
    // Make the Rubber shorter
    leftRubber.localScale = new Vector2(0, 1);
    rightRubber.localScale = new Vector2(0, 1);
}
```

现在，我们可以通过首先选择弹弓对象中的游戏对象。层次性然后点击添加组件->Scitps->Rubber..我们亦会把这两个橡胶拖到相应的槽内：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911102953907.png)
如果我们按下Play现在我们可以看到小鸟和弹弓之间的橡皮筋：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3J1YmJlcl9pbmdhbWUuZ2lm)
当然，现在我们也可以玩一轮愤怒的小鸟了：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1hbmdyeS1iaXJkcy1nYW1lL3VuaXR5X2FuZ3J5YmlyZHMuZ2lm)

### 12.总结
我们刚刚在Unity中创造了一个漂亮的小愤怒的小鸟翻版。有很多东西要学。我们使用简单的形状和颜色来达到良好的视觉效果。我们广泛使用了Unity的2D物理系统，给我们的鸟增加了很多的效果。

所有这些小把戏，比如易碎的冰，一只在与某物相撞时会长出羽毛的鸟，以及一条标志着游戏轨迹的痕迹，都会让游戏在稍后的时候感觉到。这就是为什么保持小游戏和专注于每一个小细节使游戏感觉正确是如此重要。
