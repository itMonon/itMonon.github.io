---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《植物大战僵尸游戏》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
今天我们要用Unity3D做一个植物大战僵尸的仿制版本。为了保持简单，我们将忽略所有花哨的东西，如菜单，多层次或剪裁场景，并专注于在后院与僵尸作战。

以下是游戏的预览：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wbGFudHMtdnMtem9tYmllcy1nYW1lL3VuaXR5X3BsYW50c192c196b21iaWVzLmdpZg)
## 二、版本
==Unity5.0.1f1==

## 三、正文
### 1.主摄像机设置
如果我们选择主照相机在层次性然后我们可以设置背景色若要黑色，请调整大小而位置如下图所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912142649106.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 2.创造草木
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wbGFudHMtdnMtem9tYmllcy1nYW1lL2dyYXNzLnBuZw?x-oss-process=image/format,png)
注意：右击图像，选择另存为.。，保存到项目的资产文件夹并将其保存到新的Sprites文件夹。

让我们在项目区选择它：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912142822782.png)
然后修改导入设置在==Inspector==:
*[Inspector]:自定义设置面板
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912142910832.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注：Pixels to Unit 值设成32这意味着32 x 32像素将适合在游戏世界的一个单位。我们将使用这个值作为我们所有的纹理。

之后，我们可以将图片从项目区拖入到场景中:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912143410718.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们会把它定位在(0, 0)所以它在我们游戏的左下角：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912143400254.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**添加排序层**
我们正在制作一个2D游戏，所以我们不能用三维作为深度效果，也不能轻易区分背景和前景。相反，我们将使用Sorting Layer告诉统一它应该先画哪些元素。例如，我们应该先画背景图，然后再画植物。(因此在背景之上).

我们可以换草Sorting Layer如果我们看看Inspector中的Sprite Renderer组件：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912143456796.png)
让我们选择添加排序层.。从分选层列表，添加Background层，并将其移动到顶部，如下所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912143515432.png)
之后，我们再次选择草，并分配先前创建的Background分类层：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912143530762.png)
==注意：Unity从上到下画层，因此背景中的任何内容都将位于列表的顶部。==

现在，草将永远被植物和僵尸所吸引。

**让草皮发出咔嗒声**
稍后，在处理“构建”菜单时，我们需要一种方法来确定是否单击了草瓦。在United中有各种不同的方法来实现这一点，但最简单的方法是只使用OnMouseUpAsButton函数，如果单击了草，则由United自动调用该函数。

现在这里有一件事要记住：只有当游戏对象有对撞机时，Unity才能做到这一点。所以让我们选择添加组件->物理二维->Box Collider 2D在Inspector并启用Is Trigger 选项：
- [x] Is Triggers
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912143633341.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
==注：启用触发意味着草会收到各种各样的碰撞信息，但实际上不会发生碰撞。所以，如果僵尸走进草瓦，它就不会与它相撞。==

**复制草**
现在我们的游戏中只有一片草瓦。让我们在Hierarchy，选择Duplicate然后把它放在(1, 0)..然后我们将再次复制它，并将其放置在(2, 0)..我们会继续复制，直到我们有10 * 5左下角瓷砖位于(0, 0)右上方的瓷砖在(9, 4):

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912143903857.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
==注意：重要的是所有的瓷砖都有圆形的位置(2, 3)从来没有(2.01, 3.023).==

### 3.Health脚本
僵尸应该能够攻击植物，而燃烧植物应该能够对僵尸造成伤害。我们将坚持Unity基于组件的本质，对于所有的实体，我们只需要创建一个Health脚本

我们可以通过右键单击项目区然后选择Create->C#脚本:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912144048996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们给它起个名字Health然后把它变成一个新的Scripts文件夹：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912144117500.png)
我们可以通过双击脚本打开并修改脚本：

```csharp
using UnityEngine;
using System.Collections;

public class Health : MonoBehaviour {

    // Use this for initialization
    void Start () {
    
    }
    
    // Update is called once per frame
    void Update () {
    
    }
}
```
我们不需要启动或者更新函数，所以让我们移除这两个函数。相反，我们将添加一个INT变量，该变量跟踪当前的健康状况，并添加一个减少该变量的函数。如果当前的健康状况低于0然后，应摧毁该实体：

```csharp
using UnityEngine;
using System.Collections;

public class Health : MonoBehaviour {
    // Current Health
    [SerializeField]
    int cur = 5;

    public void doDamage(int n) {
        // Subtract damage from current health
        cur -= n;

        // Destroy if died
        if (cur <= 0)
            Destroy(gameObject);
    }
}
```
==注：我们使用SerializeField让Unity知道我们希望能够在Inspector面板中修改cur变量..通常这是通过public，但在这种情况下，我们不希望其他脚本能够访问cur变量。相反，他们应该始终使用doDamage功能。
该脚本将添加到所有植物和僵尸稍后。==

### 4.创建向日葵植物
**画动画**
由于我们正在开发一个2D游戏，动画是非常容易用我们的绘图工具的选择和几个小时的时间。

对于我们的向日葵，我们只需要一个闲置的动画，它的头部轻微移动。以下是我们得出的结论：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wbGFudHMtdnMtem9tYmllcy1nYW1lL3N1bmZsb3dlci5wbmc?x-oss-process=image/format,png)
注意：右击图像，选择但作为.。并将其保存在项目的资产/Sprites文件夹。

让我们在项目区然后在Inspector面板中修改导入设置：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912144435373.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
这个Filter Mode和Format影响你看起来的样子。设置Sprite Mode到Multiple告诉Unity，在一张照片中有几个向日葵部分(亦称Tiles)。

**进口动画**
现在我们必须告诉Unity，这些Tiles在图像中的位置。我们可以打开Sprite Editor通过Inspector面板中按Sprite Editor按钮(如上图所示).

之后，我们可以在Sprite Editor中看到我们的向日葵：![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912144619864.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们要做的就是打开Slice菜单，将类型设置为格网像素大小为32 x 32..之后，我们按下Slice按钮：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912144731904.png)
让我们按Apply然后关闭 Sprite Editor.

如果我们看看项目区然后我们可以看到我们的向日葵现在有两个子节点(slices):
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912144809387.png)
**创造动画**
从这两个slices创建一个Unity动画是非常容易的。我们所要做的就是在项目区然后把他们拖到场景中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912144841282.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
一旦我们把它拖到场景中，Unity会问我们在哪里保存动画。我们可以创造一个新的SunflowerAnimation文件夹放在我们的项目区中，然后将其保存为idle.anim.

Unity会为我们创建两个文件：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912144920463.png)
这就是我们的动画，如果我们按下Play：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wbGFudHMtdnMtem9tYmllcy1nYW1lL3N1bmZsb3dlcl9hbmltYXRlZC5naWY)
==注意：我们可以通过双击sunflower_0文件中的SunflowerAnimation文件夹中，选择idle状态，然后可以在Inspector面板更改速度.==

**物理学、标签与生命值**
我们的植物应该是物理世界的一部分，这意味着我们必须分配一个Collider给它。一个Collider确保物体会与植物发生碰撞，而植物也会与其他物体发生碰撞。

让我们选择植物，然后在Inspector面板按下添加组件->Physics 2D->Box Collider 2D:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912145206699.png)
我们还需要找出某个游戏对象是一个植物，一个僵尸，还是完全不同的东西。这就是Unity标签系统是为了。如果我们看看Inspector，我们可以看到当前标记是Untagged:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912145234809.png)
我们可以分配Plant通过选择标签加上标签.。在标记列表中，然后添加Plant标记到可用标签列表，再次选择工厂，然后从标记列表中分配标记：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912145314960.png)
最后我们会选择在Inspector那里添加组件->Scripts->Health，僵尸可以在稍后对草地plant造成损害：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091214541136.png)
注：我们的向日葵刚刚被随意放置在现场的某个地方。我们将把它保存在那里，现在，只要我们正在努力在太阳产卵在下一步。在草瓦上的正确定位将在稍后实施。

### 5.生成太阳
**画太阳**
向日葵植物每隔几秒钟就会孕育出一个新的太阳，所以让我们先画一个：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wbGFudHMtdnMtem9tYmllcy1nYW1lL3N1bi5wbmc?x-oss-process=image/format,png)
==注意：右击图像，选择另存为.。并将其保存在项目的Assets/Sprites文件夹。==

我们将使用以下方法导入设置:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912145503234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**Foreground Layer前景层**
我们希望太阳被画在背景前面和植物前面。让我们点击添加排序层.。再一次创造另一个分选层，说出来前景并将其移至列表的底部：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912145558668.png)
之后，我们可以再次选择Sun并分配Foreground排序层：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912145617127.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**创建预制件**
让我们把太阳拖到Hierarchy (或进入场景)一次，从我们的纹理创建一个游戏对象：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912145700439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在我们来看一下Inspector我们把它重命名为SunPrefab然后按下添加组件按钮Physics 2D>Circle Collider 2D..Collider 让我们以后可以和太阳做一些物理方面的事情：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091214574461.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
==注意：在最初的植物大战僵尸游戏中，太阳并没有真正地与植物或僵尸发生碰撞。我们可以通过选择Is Trigger在Circle Collider 2D中。这意味着太阳接收到碰撞信息，但从未真正与任何物体发生碰撞。僵尸将能够穿过它，而不是与它相撞。==

之后，我们从Hierarchy变成一个新的预制文件夹中的项目区若要创建一个Prefab:
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091214584110.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在我们可以将sun从场景中删除。

注意：太阳稍后将通过使用实例化功能。我们不能仅仅使用Sun纹理，因为我们确实需要一个带有对撞机和所有东西的游戏对象。

**创建生成脚本**
我们希望向日葵每隔几秒钟产生一个新的太阳。这种行为可以通过脚本实现。让我们选择向日葵，然后点击添加组件->New Script:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912145940467.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们给它起个名字SunSpawn，选择CSHARP作为语言，然后将其移动到ProjectArea中的Scrips文件夹中。之后，我们将打开它：

```csharp
using UnityEngine;
using System.Collections;

public class SunSpawn : MonoBehaviour {

    // Use this for initialization
    void Start () {
    
    }
    
    // Update is called once per frame
    void Update () {
    
    }
}
```

我们将添加一个公共GameObject变量，允许我们稍后在检查器中指定Prefab：

```csharp
public class SunSpawn : MonoBehaviour {
    public GameObject prefab;
    ...
```

这个实例化函数允许我们将预置文件加载到场景中。这个InvokeRepeting函数允许我们重复调用某个函数，从将来的某个时候开始。例如，如果我们想在1秒内第一次产卵，然后每2秒重复一次，我们将使用InvokeRepeting(“产卵”，1，2)..我们希望在10秒内产生第一个太阳，然后每10秒继续生成更多的太阳，下面是我们的代码：


```csharp
using UnityEngine;
using System.Collections;

public class SunSpawn : MonoBehaviour {
    public GameObject prefab;

    // Use this for initialization
    void Start() {
        // Spawn first Sun in 10 seconds, repeat every 10 seconds
        InvokeRepeating("Spawn", 10, 10);
    }
    
    void Spawn() {
        // Load prefab into the Scene
        // -> transform.position means current position
        // -> Quaternion.identity means default rotation
        Instantiate(prefab,
                    transform.position,
                    Quaternion.identity);
    }
}
```

现在我们可以保存脚本并再一次查看Inspector面板。脚本有一个预制件因为我们的预制件变量是公开的。让我们拖着我们的SunPrefab从项目区到预制件脚本的插槽：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912150039934.png)
如果我们按下Play然后我们每10秒就能看到一个新的太阳生成。

**太阳运动**
太阳射出后，它应该慢慢地消失在屏幕的顶端。我们将使用刚体2D为了这个。刚体通常被用于物理世界中的任何东西，而这些东西应该是在周围移动的。

让我们在项目区然后在Inspector那里单击添加组件->Physics2D->Rigidbody2D。我们将为它分配以下属性：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091215014459.png)
现在我们要做的就是给刚体一些速度(这是movement direction * speed)..以下图像显示某些运动方向所需的不同矢量：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912150210433.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们将创建另一个C#脚本，命名为DefaultVelocity然后一直用它来设定速度：

```csharp
using UnityEngine;
using System.Collections;

public class DefaultVelocity : MonoBehaviour {
    // The Velocity (can be set from Inspector)
    public Vector2 velocity;

    void FixedUpdate () {
        GetComponent<Rigidbody2D>().velocity = velocity;
    }
}
```

注意：我们在每个FixedUpdate调用，调用此函数，以便无论发生什么情况，该脚本所附加的内容都将尝试继续移动。这对于僵尸来说是很有用的，当他们跑进一个植物时，可能无法继续移动，但是我们的脚本将确保他们在植物被摧毁后再次开始移动。

之后，我们再次选择Sun预置，然后单击添加组件->Scripts->Default Velocity..我们也会指定一个速度向上 (进入y方向):

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912150501135.png)
如果我们按下Play然后，我们现在可以看到太阳阳光生成，然后向上移动：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1wbGFudHMtdnMtem9tYmllcy1nYW1lL3N1bl9tb3Zpbmd1cHdhcmRzLmdpZg)
**收集阳光**
当我们谈到太阳的时候，我们必须做的最后一件事就是让Player收集它。我们将需要一个全球得分，以跟踪多少太阳被收集，我们将需要增加，每当球员点击太阳。

Unity伴随着Onmousedown函数，该函数允许我们确定是否单击了GameObject。所以让我们创建一个新的C#脚本SunCollect然后使用Onmousedown职能：

```csharp
using UnityEngine;
using System.Collections;

public class SunCollect : MonoBehaviour {
    // Global score
    public static int score = 100;

    void OnMouseDown() {
        // Increase Score
        score += 20;

        // Destroy Sun
        Destroy(gameObject);
    }
}
```

注：我们必须使用静态以使分数变量全局化。这意味着其他脚本可以使用SunCollection.得分随时都行。最初的分数是100，所以玩家有一些太阳开始建造工厂。

一旦我们将脚本添加到SunPrefab中，我们就可以按Play，等待太阳生成阳光，然后点击它收集它。

**清理层级**
现在向日葵还在等级体系中，但我们不希望它从一开始就在那里。玩家应该在稍后手动构建它。因此，让我们从它创建一个预置文件，方法是将它拖到项目区:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190912150644256.png)
### 6.植物开火
**植物图片**
好吧，让我们再创造一种植物，这种植物能够向僵尸射击。

...未完待续
