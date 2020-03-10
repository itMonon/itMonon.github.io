---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《太空射击游戏》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

@[TOC](《太空射击游戏》游戏教程)
## 一、前言
我们的2D太空射手将受到古老的街机游戏的启发，每个人在成长过程中都很喜欢这些游戏。

我们将利用物理等各种不同的统一特征(包括刚体和对撞机)、动画(Mecanim)，脚本(C#)，预制板，阴影和Sprite Editor。游戏艺术将非常容易创建与我们的绘画工具的选择。
## 二、版本
Unity 5.0.1f1

## 三、正文
### 1.相机调整
首先，我们将选择主照相机在层次性然后去找探长。我们会改变背景技术颜色改为黑色，并将大小调整为10:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911174234439.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 2.创建背景
**空间纹理**
我们的背景应该包括一个恒星的图像，滚动向下滚动，所以它似乎是在太空飞行的球员。我们首先画一个256 x 512PX空间背景在我们的绘图工具中的选择：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911174254302.png)
注意：右击图像，选择但作为.。，导航到项目的资产文件夹并将其保存到新的Sprits文件夹。

一旦保存到我们的项目中，我们就可以在项目区:
![Background in Project Area](https://img-blog.csdnimg.cn/2019091117430656.png)

之后，我们可以修改导入设置在检验员:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911174319158.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注意：通常我们会设置纹理类型到Sprits一个2D游戏。但是用UV映射只为纹理，不是为了精灵..精灵将是像素完美的，但是滚动它们需要一个着色机对于本教程来说，这太复杂了。

**将其添加到Quad中**
好吧，让我们把背景添加到我们的游戏中。我们会选择GameObject->3D Object->Quad从顶部菜单为了给我们的游戏添加一个四边形：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911175608795.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注意：我们使用Quad从三维物体菜单在2D游戏中，因为纹理(而不是Sprite)可以添加到它。

现在我们来看一下检验员和比例尺它的高宽比和我们的背景纹理是一样的：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911181921217.png)

我们也会将它重命名为背景技术:
![Quad Renamed](https://img-blog.csdnimg.cn/20190911181926858.png)

并移除Mesh Collider因为我们不需要它(这是3D游戏):
![Remove Mesh Collider](https://img-blog.csdnimg.cn/20190911181933653.png)

之后，我们可以从项目区在背景技术:
![Quad with Default Material](https://img-blog.csdnimg.cn/20190911181945784.png)

如果我们按下Play然后我们可以看到我们的空间背景，它仍然非常黑暗：
![Dark Background](https://img-blog.csdnimg.cn/20190911181953746.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

**未点亮的着色器**
背景是黑暗的，因为它当前使用标准只有当场景中有灯光时，着色器才会使事情变得明亮。我们不会使用任何类型的灯光或阴影，所以让我们选择Unlit>Texture着色器：
![Background with Unlit Texture Shader](https://img-blog.csdnimg.cn/20190911182010634.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

如果我们按下Play再一次，我们可以在不太暗的情况下看到背景恒星：
![Unlit Background ingame](https://img-blog.csdnimg.cn/20190911182039549.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

**UV映射**
如果我们仔细观察一下阴影的特性，我们就可以看到我们的紫外线了。Tiling和Offset在此：
![Background UV Properties](https://img-blog.csdnimg.cn/2019091118204911.png)

随意使用这些属性，看看它们如何改变背景。Tiling会重蹈覆辙偏移量会改变纹理的位置。如果我们设置Y偏移量对于像这样的价值观0.1, 0.2, 0.3等等，我们已经可以看到滚动发生了。

当然，为了达到滚动效果，我们不希望每秒手动更改偏移量60次。我们会写一个Script处理好了。让我们点击添加组件按钮，然后选择新脚本，说出来UVScroll并选择CSHARP关于语言：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190911182148294.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
团结创造了新的C#脚本在我们项目区并将其添加到四角..让我们创建一个新的剧本我们的文件夹项目区并将脚本移动到其中：
![UVScroll Script in Project Area](https://img-blog.csdnimg.cn/20190911182154598.png)

现在，我们可以双击脚本以便打开它：

```csharp
using UnityEngine;
using System.Collections;

public class UVScroll : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```

我们不需要启动或者更新函数，让我们移除它们。修改UV映射在最新进展函数在完成所有其他计算之后。我们还将添加一个公共变量，让我们稍后修改检查器中的滚动速度：

```csharp
using UnityEngine;
using System.Collections;

public class UVScroll : MonoBehaviour {
    public Vector2 speed;

    void LateUpdate() {
        GetComponent<Renderer>().material.mainTextureOffset = speed * Time.time;
    }
}
```

注意：虽然看起来很简单，但是这里有很多事情发生在幕后。首先，我们使用了矢量2为了我们速度变量，以确保我们可以修改x(横向)和y(垂直)速度。我们使用GetComponent<Renderer>()访问呈现器组件。使用GetComponent<Renderer>().Matter确保UV偏移量不会被永久修改，就像手工修改它时的情况一样。相反，通过使用GetComponent<Renderer>().Matter统一创建材料的运行时副本，并在游戏停止后删除它。此外，我们使用主要纺织品属性修改主纹理的UV偏移量。然后我们用时间，时间若要实现滚动效果，请执行以下操作：时间，时间是游戏开始后的时间。既然这段时间总是平稳地增长，我们不妨把它用于滚动。而且我们把时间乘以速度让它慢下来或者让它变快。因为我们速度变量是矢量2，在将它与之相乘后，它仍将是一个时间，时间..所以最后我们得到了一个新的矢量2将用于紫外偏移.
让我们保存脚本，然后查看检验员再来一次。现在我们可以修改速度了。我们将设置y速度(用于垂直滚动)以我们所希望的速度离开x速度(水平滚动)在…0因此，它根本不水平滚动：
![UV Scroll in Inspector with Speed](https://img-blog.csdnimg.cn/20190911182227670.png)

**滚动空间纹理**
如果我们按下Play然后我们可以看到我们的背景向下滚动，就像我们在太空中飞行一样：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1zcGFjZS1zaG9vdGVyLWdhbWUvYmFja2dyb3VuZF9zY3JvbGxpbmcuZ2lm)

这个效果很好，但是我们会更进一步实施。视差滚动给它增加更多的深度。

. . .未完待续
