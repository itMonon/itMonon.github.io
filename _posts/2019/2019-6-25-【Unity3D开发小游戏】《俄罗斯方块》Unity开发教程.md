---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《俄罗斯方块》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
 《俄罗斯方块》（Tetris， 俄文：Тетрис）是一款由俄罗斯人阿列克谢·帕基特诺夫于1984年6月发明的休闲游戏。
该游戏曾经被多家公司代理过。经过多轮诉讼后，该游戏的代理权最终被任天堂获得。   任天堂对于俄罗斯方块来说意义重大，因为将它与GB搭配在一起后，获得了巨大的成功。
《俄罗斯方块》的基本规则是移动、旋转和摆放游戏自动输出的各种方块，使之排列成完整的一行或多行并且消除得分。
那么怎么用Unity开发《俄罗斯方块呢》，接下来跟随作者一起来看看吧。

效果图：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191202163616482.gif)

## 二、原文链接
GitHub地址：https://github.com/764424567/Game_Snake
**注意：可以直接在GitHub仓库克隆或者下载源代码*

## 三、正文
这篇文章的目的是教你怎么使用Unity制作经典游戏－俄罗斯方块。它是在1984年6月份发布的，它只因为玩法简单，且可玩性高，也容易上瘾（至少克森的童年可以玩一个通宵，，233），而被流行起来。

我们将简单的使用Unity制作俄罗斯方块游戏，大概只有130行代码和2个资源。然而游戏似乎很简单，但是仍然有相当高的可玩性。对于初学者来说，将会获得非常大的收获哟。

照例的，我会用简单的方式来解释，让每个人都能理解它。


在教程中不需要任何高难度的操作。如果你已经了解了Unity中的GameObjects,Prefabs和Transforms的基本操作，那么你就已经做好了一切的准备。如果你没有了解，那么也不用担心太多。

**Unity版本**

在教程中，我们使用的Unity版本是Unity 4.5.1f3。不过对于新的版本也可以正常的运行。

我们不需要任何高级的效果，所有对于Unity使用免费版本就足以。

项目设置
好吧，让我们一起创建一个俄罗斯方块游戏吧！首先创建一个项目，选择2D开发模式，项目路径由你喜欢的来。如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqN1VuTmlhQ0hlbjNaeGJpY3Y1bWtpY21uV2ljWUZyWVppYUpKWHlhQ0pveWNsalFoNG51WklPdmljb2ljUS8w?x-oss-process=image/format,png)
选择场景中的Main Camera（主相机），然后在Hierarchy面板中将Main Camera的Background改为黑色，对于Transform的设置如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqUmdOdk9NVmhxZDFRejVmNW1Ub1hveWprSEFCcWhqWUZtbDFEUXRsdUFtaWNoWHprdFlvUko2QS8w?x-oss-process=image/format,png)
Note
将主相机的X设置为4.5是非常重要的，因为这将是后面场景的中心位置。

关于方块和方块组
让我们做一些定义，我们将有方块和方块组，一个方块组是有一些方块组成：


![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqaWJGWGliaWJCb0NDaWFoTzhnYmdPRGpiU1JURXhZY3loUXkwaDhiQnBkbWt6UTFhMktISmM3NFJpYWcvMA?x-oss-process=image/format,png)
我们都知道，在俄罗斯方块中有很多方块组，在教程中，我们分别给它们取名为 I, J, L, O, S, T, Z：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqcEwyemtPUHYxRU1vODduczQwbUsyUDF4dHJUZVFoYjB2YjNzUXIwWG04NXdDVWF0N3dpY2FRQS8w?x-oss-process=image/format,png)
创建游戏布局
显然，在上面的图片中，我们将保持简单的艺术风格。每一个方块组都是有一些绿色的圆角矩形方块组成：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqVmljM2JNamNmeXZVaWI0RnRPaWFwM1ZDNzdpYW9YQk5BazdkRTlNMTNZYVBZTE9tRzlTSXltaWJxQVEvMA?x-oss-process=image/format,png)
Note
你可以右键上面的绿色方块，将它保存到你的项目Assets文件夹下。

让我们在项目中找到这个图片，然后再Inspector中调整一下导入设置（注意数值要跟教程中的一样啊，哥们）：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqR2ViSEJkSEIxeXFPQUlaMFFJQmpTQkZtVjZWaWNWRDNOQWtIZUR4VUQ4Y01yWUx3cHkwa0pKZy8w?x-oss-process=image/format,png)
Note
这个Pixels To Units的指该图片在游戏中的大小

我们将使用下图来制作游戏的边界，增强视觉感：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqQlJydXNqVVRpYVliQmZsdUJ2Wk1laWI4aWJobTVFRFhGQ092YXF0bnFxSVMyRnlGN0lRZENUMGNRLzA?x-oss-process=image/format,png)
这个是图片的下载链接：
http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48EvIkR3mP7W22XVia6uueTWjBRrusjUTiaYbBfluBvZMeib8ibhm5EDXFCOvaqtnqqIS2FyF7IQdCT0cQ/0
Note
还是老样子，右键图片，将它保存到你的项目Assets文件夹下。

让我们在项目中找到这个图片，然后再Inspector中调整一下导入设置（注意数值要跟教程中的一样啊，哥们）：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqYUZydWliYnd4a2NOaGxyV1VQZHUydzk3YUEyTWc0QWlid1pzTkZkYXdQQWZWTDBqaFlqUTJEcEEvMA?x-oss-process=image/format,png)
添加边框（边界）
好了，现在开始布局我们的场景。我们将border拖拽到Hierarchy面板中（重复两次），如下图所示；
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqTzdnYlF4VWNSdzhCYllXaWFGVEhhSEZ0aWNHeU1SOUE4UmJCSzBnVkpEOUhzY1pBSVBqdTFTOFEvMA?x-oss-process=image/format,png)
在我们游戏场景中，宽度正好为10个方块的大小，高度为20个方块或者更多。所以呢，一个方块坐标的范围总是在（0，0）和（9，19）之间。

Note
如果你想计算它,让它从0开始，水平放置10个方块，垂直放置20个方块。

好吧，两个border（边框）应该放在游戏的左边和右边，所以它们的X值分别为：X=0，X=9。添加一些间距，也许是个好主意，所以让我们调整一下它们的Transform，如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqR1VKU2UydFZvclllUHAzOFZqMmV4RWlidGsxdXM0bzZiR1FWMDEzc3BrVGNsMDdLSk4zaWF3R2cvMA?x-oss-process=image/format,png)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqcTFQam56OGJSMFJhMEtSTmYzVVhFcjFhb2RaVDhzcGxFYnd5VmR5eXE4UHVGcnQ3ME1FSERBLzA?x-oss-process=image/format,png)
现在让我们点击Play按钮，便能看到如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqMlJWNEcxQVBEYWljVW1leDR5UGF5Sk1ZZllieVRuZGljMENSTW5UZzZGVWxrMjh2dm8xaWNRMmVnLzA?x-oss-process=image/format,png)
创建方块组
好的，现在是时候去组装 I, J, L, O, S, T, Z 方块组。现在创建一个空物体（在菜单栏中依次选择GameObject->Create Empty）。创建好后，便能在Hierarchy中看到：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqNHJKa2NjVHFzdXpBUWRsOEpNMXVOcVpnN1hqVnU5YmlhQ1A2bFo3MWhWbGc0bjBjYm5kQ0c2Zy8w?x-oss-process=image/format,png)
我们可以拖动四个方块图片到GameObject（就是刚刚创建的空物体）中 ，让这四个方块成为GameObject的子物体。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqUlZZUXBTSzZpYVNQemFUWDI5RWZpYm5BaG5kUVlCTUhWYnV6c1Q1TEtjbklIMUFXc1BYMllRRUEvMA?x-oss-process=image/format,png)
现在让我们组装O方块组：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqbkd5c05ydEdtZlp5SGhGZ3BFdTdqdkhYRVdmSzBvSWlhNms2aWFacE9Fd0NLSTFsSVBManJnUGcvMA?x-oss-process=image/format,png)
下面是各个方块的坐标：

- X=0 Y=0

- X=0 Y=1

- X=1 Y=0

- X=1 Y=1

注意了，这里设置都是以1为单位的，因为我们的方块的大小就是1X1。例如，我们按下左键，那就让我们的方块组向左移动1（如果当前X = 5，那移动后就是 X = 4）。因此，注意不要把值设置错了。

话句话说：只要我们使用圆形坐标就没事儿了。

PS：圆形坐标（就是由第一个点，经过四步操作又回到原来的点。。不知道你们能不能懂）

好吧，让我们为这个GameObject（原来的那个空物体）重命名吧。改为GroupO，如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqdFQzcDBiWFRvUTB4SUZyQVpMaWJhbkpuYkh6RFhpYWVOd0JKNzhsQmNVcUY1WnhsaGliV3VXVHJ3LzA?x-oss-process=image/format,png)
现在，让我们把GroupO拖拽到Assets文件夹下，让它成为一个Prefab（预制体）。
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqdGJ4c1BadUpBV21tYnNJQlV0dXZ0ekRuZERXZXl4T1Rjc1IyajlpYnFOenI3M3VZSU1ZQmljOFEvMA?x-oss-process=image/format,png)
然后把Hierachy中的GroupO给删了，我们不需要他了，我只需要GroupO的预制体即可。

我们将重复相同的操作，把 I，J，L，S，T，Z方块组也一起制作了。如下图所示：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqa2ZaV2N2QzVUQzFsd1F5UVZXSmtjTThHSFNZaWF2Uno5RTBzaWFpYTh3eUhiVG9RckkwdElhT0R3LzA?x-oss-process=image/format,png)
**Spawn 类**
让我们创建另一个空物体，命名为“Spawner”，并设置它的Position，如下所示：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqNUJXSVM5VkEyVW5NVGQ4Zm5ncDkwMXZHaGRncGEzczBNZnNIOGRYcVJRcklpYXd0aWNlNWliY3RBLzA?x-oss-process=image/format,png)

```csharp
using UnityEngine;
using System.Collections;

public class Spawner : MonoBehaviour {

// Groups
    public GameObject[] groups;
}
```
首先，我们添加一个公开的（public）GameObject[]数组，这个数组用于存放所有的方块组。

Note
数组意味着可以存放一堆的GameObject类型的对象，而不是一个。

现在，我们可以写这个spawnNext方法吧。下面利用了Random.Range（）去随机生成一个方块组的下标，然后利用Instantiate函数去生成一个方块组。

```csharp
public void spawnNext() {
    // Random Index
    int i = Random.Range(0, groups.Length);

// Spawn Group at current Position
    Instantiate(groups[i],
                transform.position,
                Quaternion.identity);
}
```
Note
transform.position是生成出来的方块组的位置（position），Quaternion.identity是该方块组的默认旋转方向。

在游戏开始的时候，我们也应该随机生成一个方块组，因此我们在Start（）中调用一次spawnNext（）：

```csharp
void Start() {
    // Spawn initial Group
    spawnNext();
}
```
Note
Start（）函数是在游戏开始时调用，且只调用一次。

目前为止一切都还是很顺利的。让我们在Hierarchy中选择Spawner物体，然后在Inspector视图中找到Add Component按钮，点击它，然后添加Spawner脚本（Add Component -> Scripts -> Spawner）。最后我们把所有方块组的预制体拖拽到Groups数组槽中，如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqdm0ydEwwU2JZd3o3MU5reGZQQVVnZ252OUVsWHFBaWJ3WnU0QlV1TEJGWnFIRHNIWWgycmRsdy8w?x-oss-process=image/format,png)
在让我们点击Play按钮，我们便能看到Spawner是如何生成第一个方块组的，如下图所示（因为是随机的，也许你的第一个方块组会和我不一样）：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEV2SWtSM21QN1cyMlhWaWE2dXVlVFdqUDVhNlJUamZhTmg4WTJBdTlWTmhNSkhuN2owaWFpY3hHQU00a0JiRU9nVmgxc043bTl0QUFMdEEvMA?x-oss-process=image/format,png)
**Gird 类**

实现游戏的某些功能，我们将需要一些函数的帮助：

- 检查所有的块是否在边界内

- 检查所有的块是否都在y=0之上

- 检查一个组是否能移动到某一个位置

- 检查一行是否填满了块

- 删除一行

- 减少一行的Y坐标

最明显的方式是使用FindGameObjectWithTag函数去检查是否塞满了一行。除了性能问题之外，使用这个函数的主要问题是我们不能确定是否能找到某个块的给定位置。反之，我们总是需要去循环所有的块，然后找到他们的位置。

数据结构
解决方案是使用一个网格（grid），或者换句话说：它就是一个二维数组（或者就是一个2x2的矩阵）。这个数据结构看起来很像这个样子：

___|_0_|_1_|_2_|...
 0 | o | x | x |...
 1 | o | x | o |...
 2 | x | x | o |...
...|...|...|...|...

这里的X意味这有一个方块，这个0意味这里没有方块。所以在坐标为（0，0）的地方没有方块，在坐标（0，1）有一个方块等等（第一排代表Y轴，第一列代表X轴）。

这便变得简单了许多，这样我便能在某个地方访问到某个方块了：

```csharp
// Is there a block at (3,4)?
if (grid[3,4] != null)
    // Do Stuff...
```

好吧，那么让我们来创建一个C#脚本，命名为“Grid”。它用于储存一个网格（或者一个二维数组）和几个有用的函数。如下所示：


```csharp
using UnityEngine;
using System.Collections;

public class Grid : MonoBehaviour {
    // The Grid itself
    public static int w = 10;
    public static int h = 20;
    public static Transform[,] grid = new Transform[w, h];
}
```

这个grid二维数组也可能使一个GameObject类型。在这里我们定义的却是一个Transform类型的数组，但是这样便简化了写法（GameObject.Transform.position），因为每一个GameObject（游戏对象）都有Transform属性，因此这便减少了一个环节。

**roundVec2()函数**

在这个函数里，我们主要使用了Mathf.Round这里函数，用于返回一个X和Y都是整数的二维向量。（Round（）函数就是我们所说的四舍五入，例如Round(1.6)，则返回1；如果是Round（1.2），则返回1，懂了吧。。233）

```csharp
public static Vector2 roundVec2(Vector2 v) {
    return new Vector2(Mathf.Round(v.x),
                       Mathf.Round(v.y));
}
```


**Note**
这个函数是一个公开的静态函数，因此可以被其他脚本直接访问。

**insideBorder()函数**

这个函数明显比上面那个简单多了。它将用于检查某个方块是否在边界之内：

```csharp
public static bool insideBorder(Vector2 pos) {
    return ((int)pos.x >= 0 &&
            (int)pos.x < w &&
            (int)pos.y >= 0);
}
```


**Note**
它为什么不检查pos.y<h呢，因为我们的方块组是从上往下掉的，因此就不必再判断pos.y<h了。

**deleteRow()函数**

这个函数用于删除某一被堆满方块的行。就是当玩家成功把一行填满的时候，便删除那一行：

```csharp
public static void deleteRow(int y) {
    for (int x = 0; x < w; ++x) {
        Destroy(grid[x, y].gameObject);
        grid[x, y] = null;
    }
}
```

这个函数接收了一个名为y的整数类型，这个y便表示要删除的某一行。然后通过遍历位置这一行的方块，最后把它们都删除咯。

**decreaseRow()函数**

当某一行被删除后，便让上一行的所有方块移到这一行上（就是一堆满一行，删除后，把该行的上一行移动到这一行上）：


```csharp
public static void decreaseRow(int y) {
    for (int x = 0; x < w; ++x) {
        if (grid[x, y] != null) {
            // Move one towards bottom
            grid[x, y-1] = grid[x, y];
            grid[x, y] = null;

// Update Block position
            grid[x, y-1].position += new Vector3(0, -1, 0);
        }
    }
}
```

这个函数也接收了一个名为y的整数类型，这个y表示被删除的那一行，然后通过遍历让在该行的上一行的所以方块都往下移动一行（在脚本里就是y-1）。

**Note**

```csharp
grid[x, y-1] = grid[x,y];

grid[x, y] = null;
```

这两行用于在二维数组中移动相对应的元素。

```csharp
grid[x, y-1].position += new Vector3(0, -1, 0);
```

这行用于移动游戏中二维数组所对应的方块的位置

**decreaseRowsAbove()函数**

这个函数将使用之前的decreaseRow()函数，用于将所有被删除行之上的所有方块都往下移动一行（因为之前的函数只移动了一行，便使用这个函数把所有方块都移动一行）。

PS：是时候说一声了，妈蛋感觉好绕口，不知道大伙们能否明白，不明白的可以来问我。。

```csharp
public static void decreaseRowsAbove(int y) {
    for (int i = y; i < h; ++i)
        decreaseRow(i);
}
```


**Note**
这个的y就是被删除的哪一行。然后通过i<h该行遍历所有行，然所有行上的方块都向下移动一行。

**isRowFull()函数**

这个函数的功能只要是判断某一行是否被填满方块：

```csharp
public static bool isRowFull(int y) {
    for (int x = 0; x < w; ++x)
        if (grid[x, y] == null)
            return false;
    return true;
}
```

这函数是相当的容易，对吧。该函数接收一个参数y（int类型），然后通过这个y，也就是这一行去循环遍历这行上的方块，检查是否该行上都有方块了，如果该行上对应的x都有方块的话，则返回true。反之，返回false。

**deleteFullRows()函数**

现在，做好了准备工作，是时候删除所有被填满的行上的方块了。这个函数通过--y（也就是y=y-1），去遍历每一行，并且通过前面几个函数的配合，删除所有被填满的行上的方块。

```csharp
public static void deleteFullRows() {
    for (int y = 0; y < h; ++y) {
        if (isRowFull(y)) {
            deleteRow(y);
            decreaseRowsAbove(y+1);
            --y;
        }
    }
}
```


**Note**
该函数的isRowFull(y)用于判断某一行上是否填满了方块，如果填满了则调用deleteRow(y)删除该行，然后再调用**decreaseRowsAbove(y+1)**让所用被删除行上的方块都向下移动一位（就是移动一行）。

以上就是我们的Grid类。我们刚刚做的事情被称为自底向上编程，即先从最简单的函数编写，然后通过这些简单的函数再一一扩展更多的功能。（这种设计思路是不是对大家很有帮助啊，至少对于克森觉得还是不错的。）

**Group脚本**
创建该脚本

这个是我们游戏的最后一个脚本了。让我们创建一个C#脚本，命名为“Group”：


```csharp
using UnityEngine;
using System.Collections;

public class Group : MonoBehaviour {

// Use this for initialization
    void Start () {

}

// Update is called once per frame
    void Update () {

}
}
```

创建一些函数

我们将为该脚本添加两个函数。还记得我们创建的那些方块组吗？我们将需要一个函数帮助我们检查每一个方块组的子方块的位置（就是组成方块组的方块的位置，，哎呀妈呀，真绕，但愿能理解）：


```csharp
bool isValidGridPos() {        
    foreach (Transform child in transform) {
        Vector2 v = Grid.roundVec2(child.position);

// Not inside Border?
        if (!Grid.insideBorder(v))
            return false;

// Block in grid cell (and not part of same group)?
        if (Grid.grid[(int)v.x, (int)v.y] != null &&
            Grid.grid[(int)v.x, (int)v.y].parent != transform)
            return false;
    }
    return true;
}
```

这个函数事实上很容易理解的，对吧。首先使用foreach循环来遍历每一个方块组的子方块，然后用一个变量（在函数中该变量就是v）存储子方块的位置。然后检查它的位置是否超出了边界。最后检查在同一个网格位置中是否存在两个或多个方块。

好吧，让我们创建最后一个函数。如果一个方块组改变了它的位置，那么方块组的方块也应跟着改变。然后原来在gird二维数组中所对应的方块的位置也应该发生改变：


```csharp
void updateGrid() {
    // Remove old children from grid
    for (int y = 0; y < Grid.h; ++y)
        for (int x = 0; x < Grid.w; ++x)
            if (Grid.grid[x, y] != null)
                if (Grid.grid[x, y].parent == transform)
                    Grid.grid[x, y] = null;

// Add new children to grid
    foreach (Transform child in transform) {
        Vector2 v = Grid.roundVec2(child.position);
        Grid.grid[(int)v.x, (int)v.y] = child;
    }        
}
```

我们再一次循环grid数组，然后通过使用parent属性（也就是Grid.grid[x,y].parent）检查某一方块是否是该方块组里的一部分。如果这个方块是当前方块组的一部分，那么它就是该方块组的子方块。然后我们通过循环该方块组的所有子方块再一次添加到gird数组中。

移动和下落

好了，现在我们可以添加一些代码，不久便能玩了。

我们将修改我们的Updata()函数，添加一点输入检测，我们先从输入左键开始吧：


```csharp
void Update() {
    // Move Left
    if (Input.GetKeyDown(KeyCode.LeftArrow)) {
        // Modify position
        transform.position += new Vector3(-1, 0, 0);

// See if valid
        if (isValidGridPos())
            // Its valid. Update grid.
            updateGrid();
        else
            // Its not valid. revert.
            transform.position += new Vector3(1, 0, 0);
    }
}
```

在这里，我们之前的函数都利用到了。我们要为向左移动做的是：

检测向左键的按下

让方块组向左移动

检查该方块组的所有子方块的位置是否有效（是否越界）

           1. 如果有效，更新所有子方块在grid数组的位置

           2. 如果无效，向右移动

现在来写向右移动的代码：


```csharp
// Move Right
else if (Input.GetKeyDown(KeyCode.RightArrow)) {
    // Modify position
    transform.position += new Vector3(1, 0, 0);

// See if valid
    if (isValidGridPos())
        // It's valid. Update grid.
        updateGrid();
    else
        // It's not valid. revert.
        transform.position += new Vector3(-1, 0, 0);
}
```

现在来写旋转的代码（也就是按向上键）


```csharp
// Rotate
else if (Input.GetKeyDown(KeyCode.UpArrow)) {
    transform.Rotate(0, 0, -90);

// See if valid
    if (isValidGridPos())
        // It's valid. Update grid.
        updateGrid();
    else
        // It's not valid. revert.
        transform.Rotate(0, 0, 90);
}
```

提示，它们的工作流程总是相同的。

现在来写向下移动的代码：


```csharp
// Fall
else if (Input.GetKeyDown(KeyCode.DownArrow)) {
    // Modify position
    transform.position += new Vector3(0, -1, 0);

// See if valid
    if (isValidGridPos()) {
        // It's valid. Update grid.
        updateGrid();
    } else {
        // It's not valid. revert.
        transform.position += new Vector3(0, 1, 0);

// Clear filled horizontal lines
        Grid.deleteFullRows();

// Spawn next Group
        FindObjectOfType<Spawner>().spawnNext();

// Disable script
        enabled = false;
    }
}
```

此时会有一些事情发生。当我们移动一个方块组到底部时，此时子方块的新位置都没有效了。然后我们需要禁用该方块组的运动，删除所有填满的方块的行，然后生成下一个方块组。

方块组应该自动下落，这个我可以实现的。我们可以通过以个变量，用于记录上一个方块组下降的时间。

```csharp
// Time since last gravity tick
float lastFall = 0;
```

然后修改控制下落的代码。当玩家按下向下箭头按钮时触发：


```csharp
// Move Downwards and Fall
else if (Input.GetKeyDown(KeyCode.DownArrow) ||
         Time.time - lastFall >= 1) {
    // Modify position
    transform.position += new Vector3(0, -1, 0);

// See if valid
    if (isValidGridPos()) {
        // It's valid. Update grid.
        updateGrid();
    } else {
        // It's not valid. revert.
        transform.position += new Vector3(0, 1, 0);

// Clear filled horizontal lines
        Grid.deleteFullRows();

// Spawn next Group
        FindObjectOfType<Spawner>().spawnNext();

// Disable script
        enabled = false;
    }

lastFall = Time.time;
}
```

下面是我们Updata()函数的最终形态：


```csharp
void Update() {
    // Move Left
    if (Input.GetKeyDown(KeyCode.LeftArrow)) {
        // Modify position
        transform.position += new Vector3(-1, 0, 0);

// See if valid
        if (isValidGridPos())
            // It's valid. Update grid.
            updateGrid();
        else
            // It's not valid. revert.
            transform.position += new Vector3(1, 0, 0);
    }

// Move Right
    else if (Input.GetKeyDown(KeyCode.RightArrow)) {
        // Modify position
        transform.position += new Vector3(1, 0, 0);

// See if valid
        if (isValidGridPos())
            // It's valid. Update grid.
            updateGrid();
        else
            // It's not valid. revert.
            transform.position += new Vector3(-1, 0, 0);
    }

// Rotate
    else if (Input.GetKeyDown(KeyCode.UpArrow)) {
        transform.Rotate(0, 0, -90);

// See if valid
        if (isValidGridPos())
            // It's valid. Update grid.
            updateGrid();
        else
            // It's not valid. revert.
            transform.Rotate(0, 0, 90);
    }

// Move Downwards and Fall
    else if (Input.GetKeyDown(KeyCode.DownArrow) ||
             Time.time - lastFall >= 1) {
        // Modify position
        transform.position += new Vector3(0, -1, 0);

// See if valid
        if (isValidGridPos()) {
            // It's valid. Update grid.
            updateGrid();
        } else {
            // It's not valid. revert.
            transform.position += new Vector3(0, 1, 0);

// Clear filled horizontal lines
            Grid.deleteFullRows();

// Spawn next Group
            FindObjectOfType<Spawner>().spawnNext();

// Disable script
            enabled = false;
        }

lastFall = Time.time;
    }
}
```

这真的很简单！！

还有一件事情，我们必须小心。一个新的方块组是否被构造出来，是否与其它物体发生碰撞，如果发生碰撞则结束游戏：

```csharp
void Start() {
    // Default position not valid? Then it's game over
    if (!isValidGridPos()) {
        Debug.Log("GAME OVER");
        Destroy(gameObject);
    }
}
```

完成得差不多了。现在让我们为Asset文件夹下的所有方块组添加Group脚本吧（选择所有方块组，在Inspector中找到Add Conponent按钮，然后找到Scripts选项，最后添加Group）：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEhObXBDTlJ1ajJtV244UlFnQVZDOTVTT0pHbUZiU0pub1VjMWJUTGJ3aWNlS0lrRlhpYVRudWljaWFjem9pYmZhTm1yTVF0amVBQmpiNDVQZy8w?x-oss-process=image/format,png)
现在让我们点击Play按钮，去享受一下游戏吧：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEhObXBDTlJ1ajJtV244UlFnQVZDOTVKREtoWmlhNWFPQnhqaWFxaWFtOGdpY3FxVGJQMnY1RHUweDF3SURjN25uRTA1NTVxeE9Mam8yZWpnLzA?x-oss-process=image/format,png)
好了，总算是完成了。不过这一篇有些内容看上去比较绕。

