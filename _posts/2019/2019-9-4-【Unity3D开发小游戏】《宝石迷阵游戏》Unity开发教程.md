---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《宝石迷阵游戏》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
让我们在Unity做一个宝石风格的游戏。对于那些从未听说过的人来说，宝石迷阵是一个三回合制的游戏，在2001年发布，后来复制为流行的糖果粉碎游戏。(这也使得这是一个Unity糖果粉碎教程).

我们将专注于游戏的核心机制。我们将实现一个8x8网格的宝石，可以互换。目标是始终匹配至少三个相同类型的宝石，无论是水平的还是垂直的。

游戏将用少于140行代码实现。和往常一样，每件事都会尽可能简单地解释，这样每个人都能理解它。

## 二、资源下载
图片资源：
源文件：

## 三、正文
**注意：Unity版本 5.0.0f4**


### 1、摄像机调整

如果我们选择主照相机在层次性然后我们可以设置背景色若要黑色，请调整大小而位置如下图所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904173154560.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

### 2、宝石
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1tYXRjaC0zLWdhbWUvYmx1ZS5wbmc?x-oss-process=image/format,png)
导入到我们项目中，Sprites文件夹：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904173335914.png)
然后修改设置：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904173427983.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注：Pixels Per Unit：16这意味着16x16像素将适合在游戏世界的一个单位。我们将把这个价值用于我们所有的宝石。其他的值如Filter Mode主要是为了平衡压缩和质量。

**创建游戏对象**
将宝石拖入到场景中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904173530960.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
注：我们项目区的宝石图像只是一个图像，仅此而已。但是一旦我们把它拖到场景中，它就变成了一个游戏对象，它有一个位置，一个旋转，一个比例尺，一个渲染器和其他各种我们可以添加到其中的组件。

**创建Gem.cs脚本**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904173610456.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
将脚本拖入到Scripts文件夹中（要养成规范分类的习惯）：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904173706697.png)
编辑Gem.cs脚本：

```csharp
using UnityEngine;
using System.Collections;

public class Gem : MonoBehaviour {

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

public class Gem : MonoBehaviour {
    
    // Update is called once per frame
    void Update () {
    
    }
}
```
让我们添加一个函数，让我们知道两个GEM是否是同一类型的。如果我们能把两个蓝宝石和==，因为它们是不同的游戏对象，所以它们永远不会是平等的。我们只想知道它们是否属于同一种宝石类型，或者换句话说，它们是否使用相同的Sprites：

```csharp
public bool sameType(Gem other) {
    return GetComponent<SpriteRenderer>().sprite == other.GetComponent<SpriteRenderer>().sprite;
}
```
注意：如果这还不清楚，想象两颗宝石在一排。如果我们能把它们和==那他们就是不平等因为它们是不同的游戏对象，在不同的位置等等。然而，如果我们将它们与我们的同类型功能，那么它们将是平等如果它们都是蓝色的宝石，或者都是红色的宝石。他们会不平等如果其中一个是蓝色另一个是红色..当一条线上的三颗宝石相等时，它们就是匹配的，可以从游戏中移除。

**创建宝石的预制体**

将宝石拖入到我们的Project面板中的Prefabs文件夹中：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904173924103.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
然后，我们就可以在场景中删除blue对象了，因为我们可以在脚本中实例化对象

**红宝石**
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1tYXRjaC0zLWdhbWUvcmVkLnBuZw?x-oss-process=image/format,png)
设置属性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904174021961.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
然后也把它拖入到场景中，添加Gem.cs脚本，并且设置成预制体：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904174105376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904174109714.png)
**其他宝石**
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1tYXRjaC0zLWdhbWUvYnJvd24ucG5n?x-oss-process=image/format,png)![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1tYXRjaC0zLWdhbWUvZ3JlZW4ucG5n?x-oss-process=image/format,png)![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1tYXRjaC0zLWdhbWUvcGluay5wbmc?x-oss-process=image/format,png)![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1tYXRjaC0zLWdhbWUvd2hpdGUucG5n?x-oss-process=image/format,png)![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1tYXRjaC0zLWdhbWUveWVsbG93LnBuZw?x-oss-process=image/format,png)
我们将其他的宝石也重复这个流程做一遍：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019090417425180.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 3、网格
让我们添加一个新脚本叫做Grid.cs，挂载到Main Camera物体上
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904174355700.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
将Grid.cs脚本拖入到Scripts文件夹中（规范分类啊喂！）：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019090417450244.png)
让我们编辑Grid.cs脚本：

```csharp
using UnityEngine;
using System.Collections;

public class Grid : MonoBehaviour {

    // Use this for initialization
    void Start () {
    
    }
    
    // Update is called once per frame
    void Update () {
    
    }
}
```
我们可以删除启动函数，因为我们不需要它：

```csharp
using UnityEngine;
using System.Collections;

public class Grid : MonoBehaviour {
    
    // Update is called once per frame
    void Update () {
    
    }
}
```
**网格尺寸**
让我们添加一个宽度和高度变量以控制水平和垂直宝石的数量：

```csharp
using UnityEngine;
using System.Collections;

public class Grid : MonoBehaviour {
    // Grid size
    public static int w = 8;
    public static int h = 8;
    
    // Update is called once per frame
    void Update () {
    
    }
}
```

注：w平均宽度和h意味着身高。我们会用8对于这两种情况，最后我们得到了一个8×8的网格。我们做了两个变量公共静电这样就可以通过以下方式从任何其他脚本中访问它们Grid.w或Grid.h


未完待续。。。
