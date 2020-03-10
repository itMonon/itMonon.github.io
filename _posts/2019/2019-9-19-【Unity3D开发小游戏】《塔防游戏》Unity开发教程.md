---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《塔防游戏》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

@[TOC]
## 一、前言
在本教程中，我们将创建一个小的三维塔防御游戏与一个完全独特的图形风格。该游戏将非常容易制作，将不需要任何三维建模技能，没有动画，没有什么是复杂的。

## 二、版本
==Unity5.3.1f1==
较新的版本也应该正常工作，旧版本可能工作也可能不起作用。

我们不需要任何高级的功能，所以免费版的Unity就可以。

## 三、开始

### 1、游戏规则
游戏规则基本如下：
- 怪物每隔几秒钟就会生成一次。
- 怪物们穿越世界，奔向他们想要摧毁的城堡
- 玩家可以建造攻击怪物的塔

此外，我们将使用建筑地点，也就是地图上预定义的位置，玩家可以在这个位置建立塔。


### 2、艺术风格
我们有两个选择：要么我们追求现实的东西，这将需要数百个最高质量的3D模型，纹理和阴影，或者我们追求一些简单但独特的东西。

我们的座右铭是越简单越好！因此，我们显然不会深入到CAD工具世界中去创建真实的3D模型。相反，我们将使用一种完全简约的风格，为我们节省大量的工作，但看起来很有趣。我们将利用Unity的自带的模型(立方体，球体，圆柱体，.)再加上几种颜色(主要是绿色、黄色和灰色).

这种艺术风格最好的地方是大家可以在不到一个小时的工作，没有任何三维建模或动画知识。

### 3、摄像机设置
我们在Hierarchy选择主摄像机，然后将背景色设成黑色，并且调整位置如下图所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919160257774.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注：这将使相机在旋转45°角后向下看游戏世界。

### 4、光线设置
让我们为我们的游戏添加一些光线，这样世界以后就不会太黑暗了。我们可以通过选择**GameObject->Light->Directional Light**在上面的菜单里。然后，我们将使用以下设置，以确保光线以完美的角度照射到我们的场景中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919160514514.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注意：我们基本上可以使用任何我们想要的设置，但上面的设置看起来真的很好。
### 5、地面设置
好吧，所以我们需要一些地方让怪物继续前进。让我们选择一个平面
**GameObject->3D Object->Plane**，改名字叫做Ground，并且设置一下位置和缩放比例：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919160640157.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注：这将是适合所有建筑和城堡的完美尺寸。

让我们打开我们选择的绘图工具，然后用一些基本的绿色色调创建一个小的40x40px纹理。自由地创造你自己的纹理，用一些基本的颜色填充它，然后用稍微明亮或更深绿色的色调画几条随意的线条。这是我们想出的
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS90b3dlci1kZWZlbnNlLWdhbWUvdGV4X2dyb3VuZC5wbmc?x-oss-process=image/format,png)
*注意：右击图像，选择另存为，导入到项目中的，保存到Texture文件夹中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919160755770.png)
然后我们将纹理拖入到场景中的Ground物体上面：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919160839230.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
如果我们现在再仔细看一下地面，我们就能看到它的纹理是如何的光滑：


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919160904802.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
这对于大多数游戏来说都很好，但是对于我们的塔防御教程，我们希望纹理看起来像素，以实现一个更独特的风格。让我们在项目区选择图片，选择导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919160932969.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
它现在看起来完全像像素一样：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091916101828.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注：我们将使用这种风格的所有纹理在我们的游戏。
### 6、建筑物设置
**建筑物立方体**
好吧，我们需要一个玩家可以在那里建一个塔的地方(而不是能够在世界上任何地方建造它，这会更加复杂)..我们将从创建一个建筑物开始，然后复制它几次，这样我们就可以使用建筑物为怪物设计某种迷宫。

让我们GameObject->3D Object->Cube创建一个立方体，然后位置设置一下，命名为Buildplace:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919161344144.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注：x和z坐标应该总是四舍五入-14(而不是-13.99)。这个y位置0.5所以立方体正好站在地面平面的顶端，而不是在地面上的一半。

下面是场景中的场景：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919161404142.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**建筑物纹理**
让我们也画一个非常简单的8x8px纹理，我们可以放在上面：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS90b3dlci1kZWZlbnNlLWdhbWUvdGV4X2J1aWxkcGxhY2UucG5n?x-oss-process=image/format,png)
*注意：右击图像，选择另存为，导入到项目中的，保存到Texture文件夹中：

导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919161503666.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
然后我们将材质附给建筑物，让它看起来更有质感：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919161540402.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**Buildplace.cs脚本**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919161603566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
将脚本附给Buildplace物体：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919161633794.png)
将脚本移动到Scripts文件夹，这样看起来更加整洁：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919161702109.png)
编辑脚本Buildplace .cs：

```csharp
using UnityEngine;
using System.Collections;

public class Buildplace : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
首先，我们不需要启动或者更新方法，因此，让我们删除它们：

```csharp
using UnityEngine;
using System.Collections;

public class Buildplace : MonoBehaviour {

}
```
然而，我们将需要某种公共变量来指定应在以后建造的塔：

```csharp
using UnityEngine;
using System.Collections;

public class Buildplace : MonoBehaviour {
    // The Tower that should be built
    public GameObject towerPrefab;

}
```
下一步是使用实例化函数可在单击Buildplace之后构建塔。像往常一样，团结使我们的生活在这里非常容易，因为它已经提供了一个OnMouseUpAsButton我们可以利用的函数：

```csharp
using UnityEngine;
using System.Collections;

public class Buildplace : MonoBehaviour {
    // The Tower that should be built
    public GameObject towerPrefab;

    void OnMouseUpAsButton() {
        // TODO build stuff...
    }
}
```
现在我们要做的就是把塔建在建筑上面：

```csharp
void OnMouseUpAsButton() {
    // Build Tower above Buildplace
    GameObject g = (GameObject)Instantiate(towerPrefab);
    g.transform.position = transform.position + Vector3.up;
}
```
*注意：首先，我们使用实例化..之后，我们将位置修改为当前Buildplace的位置+一个单元向上。

我们保存脚本可以看到，我们可以把塔预制件拖到Tower Prefab的位置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919161928515.png)
**创建更多的建筑物**
现在，一个建筑物已经完成，我们可以在场景中选择建筑物，然后复制：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919162014170.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
之后我们会把它定位在(-13, 0.5, -14)所以它正好在前一个的旁边：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919162025284.png)
我们将重复这个过程很多次，以建立某种迷宫，让怪物走进来。我们还将在城堡顶部留下一个空旷的区域：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919162038109.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 7、城堡设置
**城堡模型**
好吧，让我们创造一个城堡让怪物们有东西要摧毁。
让我们选择GameObject->3D Object->Cube，给它命令为Castle（城堡），把它放在迷宫中的空旷区域，再把它放大一点：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919162151700.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
城堡结构
我们还将再次创建一个非常简单的16 x 16 px纹理：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS90b3dlci1kZWZlbnNlLWdhbWUvdGV4X2Nhc3RsZS5wbmc?x-oss-process=image/format,png)
*注意：右击图像，选择另存为，导入到项目中的，保存到Texture文件夹中：

导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919162238959.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
然后让我们给我们的城堡附一个非常漂亮的材质吧。。。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919162311833.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

**城堡血条设置**
好吧，让我们给城堡加一个血条，要做到这一点有很多不同的方法，我们将选择最简单的方法。我们将在城堡上方添加一个3D文本，包括“1格血条”、“2格血条”、“3格血条”等文本。

我们可以先创建一个空物体：
在Hierarchy选择Create Empty，然后我们将新的游戏对象重命名为HealthBar,并且添加组件Add Component->Mesh->Text Mesh：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919162655784.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们将把它放在城堡上方的一个单元上，然后修改TextMesh组件以获得完美的结果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919162711326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注：文本属性已经包含7个‘-’，等于7个生命值。大多数其他属性都更改为修改字体大小和清晰度。同样重要的是Arial(或任何其他字体)进入Font文本。我们可以通过单击Font属性右侧的小圆圈来做到这一点。之后，联合会显示一个当前可用字体的列表，我们可以在其中选择一个字体。

下面是场景中的场景：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919162801984.png)
就这么简单。现在我们只需要一个能做三件事的血条脚本：
- 通过计数“-”返回当前的血量状态。
- 通过删除一个‘-’降低当前的血量
- 让TextMesh时刻面对镜头

我们希望TextMesh总是面对镜头，以避免像这样奇怪的角度：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919162854579.png)
让我们添加一个新脚本Health.cs:

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
首先，我们需要访问TextMesh组件：

```csharp
using UnityEngine;
using System.Collections;

public class Health : MonoBehaviour {
    // The TextMesh Component
    TextMesh tm;

    // Use this for initialization
    void Start () {
        tm = GetComponent<TextMesh>();
    }

    // Update is called once per frame
    void Update () {

    }
}
```

现在，我们可以通过使用更新职能：

```csharp
using UnityEngine;
using System.Collections;

public class Health : MonoBehaviour {
    // The TextMesh Component
    TextMesh tm;

    // Use this for initialization
    void Start () {
        tm = GetComponent<TextMesh>();
    }

    // Update is called once per frame
    void Update () {
        // Face the Camera
        transform.forward = Camera.main.transform.forward;
    }
}
```

*注：这里没有复杂的数学。我们所做的就是让我们的游戏对象看想相机所看到的的完全相同的方向。

现在我们可以添加current()和decrease()函数：

```csharp
// Return the current Health by counting the '-'
public int current() {
    return tm.text.Length;
}

// Decrease the current Health by removing one '-'
public void decrease() {
    if (current() > 1)
        tm.text = tm.text.Remove(tm.text.Length - 1);
    else
        Destroy(transform.parent.gameObject);
}
```
*注意：这是唯一看起来有点奇怪的东西。通常，我们会有某种整数变量来表示我们当前的血量状况。相反，我们利用了TextMesh的文本组件，由这些“-”字符串组成。我们还会自动销毁HealthBar的父对象(即城堡)。这些功能我们使用Public访问权限，以便其他脚本可以使用它们。

这就是我们游戏中最简单的HealthBar。好的是我们也能把它用在怪物身上。

### 8、怪物设置
让我们在游戏中加入一个怪物。通常，我们要创建一个怪物，需要很多建模工具，比如CAD工具，Blender，Maya或3 DS MAX。但是，由于我们希望保持简单，我们将使用Unity自带模型来创建某种看起来有点像怪物的立方体。

我们将从从上面的菜单选择GameObject->3D Object->Cube。我们会把它重命名为Monster：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919163407865.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
让我们也画一个8x8px纹理，给它一些颜色：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS90b3dlci1kZWZlbnNlLWdhbWUvdGV4X21vbnN0ZXIucG5n?x-oss-process=image/format,png)
*注意：右击图像，选择另存为，导入到项目中的，保存到Texture文件夹中：

导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919163647505.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
然后就给我们的“怪物”附上一个美美的皮肤吧。。。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919163721404.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**怪物的眼睛**
让我们给怪物添加一个眼睛，把它放置到我们立方体的左边中心，使它表小，看起来更像一只眼睛：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919163830967.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091916384118.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
眼睛也画一个白色纹理：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS90b3dlci1kZWZlbnNlLWdhbWUvdGV4X2V5ZS5wbmc?x-oss-process=image/format,png)
*注意：右击图像，选择另存为，导入到项目中的，保存到Texture文件夹中：

导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919163944721.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
然后把它拖到我们的眼睛游戏对象上。

我们将为另一只眼睛重复这一过程，直到如下所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919163948888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
请随意添加更多的细节给怪物，随你的意愿。我们只需再增加两耳朵：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919164015303.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**怪物物理**
我们的怪物已经有碰撞器了，这使得它已经成为物理世界的一部分。不过，这里还有一件事要补充。任何应该在物理世界中移动的东西都应该有一个刚体附在上面。刚体负责物体的重力、速度和其他使物体运动的力。

添加刚体：

Add Component->Physics->Rigidbody
我们将使用以下设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919164140239.png)
*注意：我们在这里几乎使用了默认设置。

**寻路**
我们希望怪物能够穿越迷宫进入城堡。要做到这一点，我们必须做两件事：
- 烘焙导航网格(要告诉统一号哪个区域是可步行的)
- 在怪物中添加一个NavMeshAgent组件
因此，让我们首先告诉Unity，我们的世界哪些部分是可行走的。好消息是，Unity实际上是自己解决了这个问题。我们真正要做的就是告诉Unity我们的世界是哪一部分静态 (如：永不移动).

让我们选择地面所有的建筑场然后启用静态属性:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919164501124.png)
现在Unity知道那些东西永远不会移动，因此它将使用它们来计算可行走的区域。

现在我们可以选择Window->Navigation在顶部菜单中使用以下属性：


![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919164532975.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
之后，我们单击烘焙按钮在底部。几秒钟后，我们可以看到Unity是如何计算场景中的可步行区域的：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919165037925.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
*注意：重要的部分是穿过迷宫的路径。团结也认为建筑本身是可步行的，但由于没有楼梯让怪物爬上去，这并不重要。

好了，现在我们可以选择怪物再次单击添加组件->通航->NAV网格剂..这个组件是所有想要沿着我们刚刚烘焙的Navesh走的东西所需要的。我们将为我们的代理使用以下设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091916504887.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注意：我们只是修改了移动速度和大小，以适应我们的怪物。如果你对这些设置不满意的话，可以自由地玩这些设置。

既然我们的NavMesh已经烤好了，现在我们的怪物有了一个NavMeshAgent，我们所要做的就是让它移动到这样的地方：

```csharp
GetComponent<NavMeshAgent>.destination = new Vector3(1, 2, 3);

```
注意：我们将在稍后创建Monster脚本时这样做。

**怪物血条**

让我们也添加一个血条到我们的怪物，以便塔可以攻击它以后。我们将使用与城堡的HealthBar完全相同的工作流，只是使用一些稍微不同的设置，使其更小、更红：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919165138872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注意：我们还更改了文本属性为4x‘-’，因此怪物只能抵抗4座塔的攻击。

下面是怪物血条的场景：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919165152536.png)
添加Monster.cs脚本：

```csharp
using UnityEngine;
using System.Collections;

public class Monster : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```

让我们移除更新方法，因为我们不需要它：

```csharp
using UnityEngine;
using System.Collections;

public class Monster : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }
}
```

我们将使用启动函数在场景中找到城堡游戏对象，然后使用怪物的NavMeshAgent向它移动：

```csharp
// Use this for initialization
void Start () {
    // Navigate to Castle
    GameObject castle = GameObject.Find("Castle");
    if (castle)
        GetComponent<NavMeshAgent>().destination = castle.transform.position;
}
```
就这么简单。

现在我们要减少城堡的血量一旦怪物走进它。我们的怪兽碰撞器触发已启用，这意味着Unity会自动通知我们OnTriggerEnter功能。我们所要做的就是找出我们与城堡相撞的东西，然后降低它的血量：

```csharp
void OnTriggerEnter(Collider co) {
    // If castle then deal Damage
    if (co.name == "Castle") {
        co.GetComponentInChildren<Health>().decrease();
    }
}
```
在怪物对城堡造成破坏后，我们不再需要它了，所以让我们在这里也摧毁它吧：

```csharp
void OnTriggerEnter(Collider co) {
    // If castle then deal Damage, destroy self
    if (co.name == "Castle") {
        co.GetComponentInChildren<Health>().decrease();
        Destroy(gameObject);
    }
}
```
这就是我们的怪物剧本，和往常一样，团结让我们在这里过得很轻松。
*注意：不要忘记保存脚本。

**怪物预制件**
现在我们不希望我们的怪物从一开始就出现在游戏中。相反，我们希望将其保存为预制件这样我们就可以随时把它装进游戏里了。

我们所要做的就是把我们的怪物变成一个新的预制体:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919165425509.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在，我们可以在任何时候将其加载到场景中，只需将其从ProjectArea拖到其中，或者使用实例化函数在脚本中的作用(我们很快就会这么做).

因为我们现在有一个预置文件，所以我们可以在场景中删除怪物.

### 9、生成怪物
好的，让我们创造一个生成怪物，进入游戏世界每几秒钟加载新的怪物。我们将从选择GameObject>Create Empty从上面的菜单。我们会把它重命名为Spawn把它放在迷宫的开头：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919165607328.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
让我们来看一下Gizmo：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919165615659.png)
注意：Gizmo是一个视觉指示器，它可以用于空的游戏对象，这样就可以更容易地看到它们。小游戏只显示在场景中，而不是在最后的游戏中。

现在可以更容易地看到生成位置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919165635196.png)
让我们添加新的脚本Spawn.cs:

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
我们不需要更新方法，因此，让我们删除它：

```csharp
using UnityEngine;
using System.Collections;

public class Spawn : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }
}
```
现在我们可以增加一个公众MonsterPrefab变量，以便我们可以指定应该生成的怪物：

```csharp
using UnityEngine;
using System.Collections;

public class Spawn : MonoBehaviour {
    // The Monster that should be spawned
    public GameObject monsterPrefab;

    // Use this for initialization
    void Start () {

    }
}
```
我们将为间隔，这是在生怪物之间的延迟：

```csharp
using UnityEngine;
using System.Collections;

public class Spawn : MonoBehaviour {
    // The Monster that should be spawned
    public GameObject monsterPrefab;

    // Spawn Delay in seconds
    public float interval = 3;

    // Use this for initialization
    void Start () {

    }
}
```
现在我们所要做的就是创建一个生成新怪物的函数，然后每隔几秒钟调用该函数一次。InvokeRepeting:

```csharp
using UnityEngine;
using System.Collections;

public class Spawn : MonoBehaviour {
    // The Monster that should be spawned
    public GameObject monsterPrefab;

    // Spawn Delay in seconds
    public float interval = 3;

    // Use this for initialization
    void Start() {
        InvokeRepeating("SpawnNext", interval, interval);
    }

    void SpawnNext() {
        Instantiate(monsterPrefab, transform.position, Quaternion.identity);
    }
}
```
真的那么简单。我们可以保存脚本并查看检验员，在那里我们可以拖着我们的怪物预制件项目区在剧本里MonsterPrefab插槽：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919165801689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
如果我们按下Play然后，我们现在可以看到一个新的怪物是如何产生每3秒。怪物们甚至向城堡移动：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919165820189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 10、子弹设置
让我们创建一个简单的子弹预制，这样我们就可以创建一个塔之后。我们将从选择GameObject->3D Object->Cube从上面的菜单。我们会把它重命名为Bullet（子弹），将其缩小一点，并启用IsTrigger：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919165915107.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注意：我们启用了IsTrigger，这样子弹就能穿越墙壁，在任何情况下都能击中怪物。

让我们画一个简单的8x8px纹理，我们可以用于子弹：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS90b3dlci1kZWZlbnNlLWdhbWUvdGV4X2J1bGxldC5wbmc?x-oss-process=image/format,png)*注意：右击图像，选择另存为，导入到项目中的，保存到Texture文件夹中：

导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919170003923.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
之后，我们可以在场景中看到我们的子弹：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919170021796.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
好吧，子弹应该在游戏世界里飞到怪物身上。正如我们所知，物理世界中任何想要移动的东西都需要一个刚体。让我们选择Add Component->Physics->Rigidbody并指定下列属性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019091917004453.png)
新建Bullet.cs脚本：

```csharp
using UnityEngine;
using System.Collections;

public class Bullet : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
我们不需要启动或者更新功能。相反，我们将使用FixedUpdate这有点像更新，只是它是在物理计算的相同时间内被调用的。既然子弹是物理世界的一部分，我们将用FixedUpdate:

```csharp
using UnityEngine;
using System.Collections;

public class Bullet : MonoBehaviour {

    void FixedUpdate() {

    }
}
```

子弹需要一个目标飞到，速度变量：

```csharp
using UnityEngine;
using System.Collections;

public class Bullet : MonoBehaviour {
    // Speed
    public float speed = 10;

    // Target (set by Tower)
    public Transform target;

    void FixedUpdate() {

    }
}
```

注意：目标将是它想要攻击的怪物。目标将由发射子弹的塔设定。
让我们修改一下FixedUpdate使子弹飞向目标。我们将简单地使用刚体的速度特性。速度被定义为运动方向乘以速度，所以让我们这样做：

```csharp
void FixedUpdate() {
    // Fly towards the target
    Vector3 dir = target.position - transform.position;
    GetComponent<Rigidbody>().velocity = dir.normalized * speed;
}
```

*注：我们计算方向，从怪物的位置减去当前位置，这是基本的矢量数学。然后我们把这个方向乘以速度。我们还使用了归一化方向，以确保它的长度为1。

就这么简单。现在可能会发生这样的情况：一只怪物跑进城堡，然后死去，而一颗子弹仍然试图到达城堡。让我们为此做好准备：

```csharp
void FixedUpdate() {
    // Still has a Target?
    if (target) {
        // Fly towards the target
        Vector3 dir = target.position - transform.position;
        GetComponent<Rigidbody>().velocity = dir.normalized * speed;
    } else {
        // Otherwise destroy self
        Destroy(gameObject);
    }
}
```

太好了，这里还有一件事要做。我们希望子弹一到达怪物就对它造成伤害。然后子弹会在造成伤害后自我毁灭。我们将简单地利用统一的OnTriggerEnter函数可以知道它何时到达怪物：

```csharp
void OnTriggerEnter(Collider co) {
    Health health = co.GetComponentInChildren<Health>();
    if (health) {
        health.decrease();
        Destroy(gameObject);
    }
}
```

那是我们的子弹。然而，我们实际上并不希望它从一开始就出现在场景中，所以让我们给它做成预制体：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919170231187.png)
### 11、防御塔设置
我们已经建立了许多建筑的地方，我们可以把塔上，现在是时候创建一个塔。

我们将从选择 GameObject->3D Object->Cube把它重命名为Tower（塔），缩小一点，给它一些旋转，只是为了更好的外观：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919171210799.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们将使用以下16x16px纹理作为我们的塔：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS90b3dlci1kZWZlbnNlLWdhbWUvdGV4X3Rvd2VyLnBuZw?x-oss-process=image/format,png)
*注意：右击图像，选择另存为，导入到项目中的，保存到Texture文件夹中：

导入设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919171306572.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
将材质赋值给防御塔：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919171325287.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们的塔模型完成了。现在我们还要加上逻辑，找到一个靠近塔的怪物，然后朝它开一颗子弹。做这件事有很多不同的方法。最明显的方法是编写一个脚本，找到所有的怪物，然后向最近的一颗发射一颗子弹。虽然这样做很好，但在计算上也是非常昂贵的。

在塔周围加上一个大的球体触发器，并使用Unity的OnTriggerEnter功能。这样，每当怪物走进塔的“觉知区”时，Unity就会自动通知我们。

让我们先移除塔的BoxCollider：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919171343677.png)
之后我们会选择Add Component->Physics->Sphere Collider并指定下列属性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919171416676.png)
如果我们看看场景然后我们可以看到球体。这是塔看到并攻击怪物的地方：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919171427859.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
我们需要一个脚本来实施塔攻击。添加一个Tower.cs脚本：

```csharp
using UnityEngine;
using System.Collections;

public class Tower : MonoBehaviour {

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

public class Tower : MonoBehaviour {

    // Update is called once per frame
    void Update () {

    }
}
```

让我们增加一个公众BulletPrefab变量，该变量指定要使用哪一Bullet预制体：

```csharp
using UnityEngine;
using System.Collections;

public class Tower : MonoBehaviour {
    // The Bullet
    public GameObject bulletPrefab;

    // Update is called once per frame
    void Update () {

    }
}
```

现在我们可以使用OnTriggerEnter函数并找出进入触发器的东西是否是怪物，在这种情况下，我们向它发射一颗子弹：

```csharp
void OnTriggerEnter(Collider co) {
    // Was it a Monster? Then Shoot it
    if (co.GetComponent<Monster>()) {
        GameObject g = (GameObject)Instantiate(bulletPrefab, transform.position, Quaternion.identity);
        g.GetComponent<Bullet>().target = co.transform;
    }
}
```

*注：我们使用GetComponent找出不管是什么东西进入扳机怪物剧本附在上面。如果是这样，那么我们使用实例化将子弹加载到游戏世界中，在塔的当前位置和默认的旋转(Quaternion.Identity)。之后，我们访问“子弹”组件并设置目标向怪物。
让我们也用我们的更新函数为我们的塔增加一个非常简单的旋转，这样它看起来更好：

```csharp
using UnityEngine;
using System.Collections;

public class Tower : MonoBehaviour {
    // The Bullet
    public GameObject bulletPrefab;

    // Rotation Speed
    public float rotationSpeed = 35;

    void Update() {
        transform.Rotate(Vector3.up * Time.deltaTime * rotationSpeed, Space.World);
    }

    void OnTriggerEnter(Collider co) {
        // Was it a Monster? Then Shoot it
        if (co.GetComponent<Monster>()) {
            GameObject g = (GameObject)Instantiate(bulletPrefab, transform.position, Quaternion.identity);
            g.GetComponent<Bullet>().target = co.transform;
        }
    }
}
```

下面是我们的塔旋转看起来是多么的讨人喜欢：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS90b3dlci1kZWZlbnNlLWdhbWUvdG93ZXJfcm90YXRpbmcuZ2lm)
别忘了把子弹从项目区拖入到塔的子弹预制件插槽：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919171700420.png)

这里还有一个小改动要做。塔周围有一个相当大的球对撞机。此对撞机将与Buildplace的对撞机重叠，从而防止OnMouseUpAsButton函数不被调用。我们所要做的就是让团结在检查鼠标时忽略塔的对撞机。我们可以通过选择忽略Raycast图层：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919171716942.png)
现在，我们可以进入我们Prefabs文件夹：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919171741780.png)
然后把我们的塔拖到Tower Prefab插槽：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919171753857.png)
如果我们按下Play然后，我们现在可以通过单击建筑物来构建塔。塔甚至会攻击怪物：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919171836571.png)
这也意味着我们的Unity塔防游戏现在已经完成：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190919171856775.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 12、内容拓展
这是我们的Unity塔防教程。和往常一样，现在该由读者来让游戏变得有趣了。有各种各样的东西可以添加：
- 得分
- 金币
- 不同怪物
- 不同塔
- 不同子弹
- 不同等级
- 背景音乐
- 更好的三维动画模型
- 菜单
- 保存游戏
- 还有更多
