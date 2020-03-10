---
layout: post
category: Unity3D-Opti
title: 【Unity3D】Unity性能改进
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 前言
如果一个游戏卡死了，它就没有乐趣。本文介绍了一些非常简单的性能改进，为了让玩家满意，每个Unity 开发者都应该知道这些改进。没有人期望你制作一个看起来像AAA+标题的游戏，但是它应该每秒有大量的帧。

注意：当我们谈论在FPS改进环境中，我们总是意味着计算起来很费时间(是什么使我们的CPU变得疯狂)。

## 算法和数据结构
当涉及到游戏的性能时，最主要的部分是开发人员对高效算法和数据结构的了解。这是一个太大的主题，不能在这里讨论。如果您是编程新手，请尝试查看以下主题，以了解什么时候最好使用这些主题：
- 列表
- 链接列表
- HashMaps
- 树
- 排队
- 堆叠
- 排序(快速排序，Bubblesort，.)
## 数学
为了保持理智，我们将在本文中不讨论数学。然而，数学对性能很重要。想象一下我们的场景中有个怪物。玩家现在射杀了怪物-我们怎样才能发现他是否击中了它？

有无数的方法可以做到这一点，它们都包括数学。你的工作是了解他们，并决定哪一种是最好的方法。幸运的是，联合已经为我们提供了大量的功能。不过，如果找不到合适的方法，数学就会出现。

## 网格
让我们来谈谈一些可以不用付出很大努力就可以学习和应用的东西：优化网格。在制作游戏时，我们通常在场景中有很多3D模型。每个模型都由一个所谓的网格组成。网格就是一大串三角形。三角形越多，我们得到的FPS就越少，所以保持它们尽可能低是很重要的。

例如，一个怪物模型可以看起来真的很好，只有4000三角形。没有必要使用其中的一百万个。大多数三维建模程序已经具有网格优化功能，这取决于您使用它们。

如果没有方法绕过有很多三角形的网格，那么还有另一个选择：LOD(细节级别)。LOD的概念很简单：当网格离摄像机很远时，它会被修改，所以它有较少的三角形。玩家看不出有什么不同，因为离摄像机很远。统一支持这个特性，所以总有一天会尝试一下。

## 缓存GetComponent
通常使用GetComponent函数来访问GameObject的组件。就计算而言，这个函数是昂贵的。

让我们看一个关于如何不使用它的示例：

```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {

    // Use this for initialization
    void Start () {
        
    }
        
    // Update is called once per frame
    void Update () {
        Health h = GetComponent<Health>();
        h.increaseBy(100);
    }
}
```

这个例子很糟糕，因为每次更新都会调用GetComponent一次(大约每秒60次)。这一点可以很容易地改进如下：


```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {
    // Cache
    Health h;

    // Use this for initialization
    void Start () {
        h = GetComponent<Health>();
    }
        
    // Update is called once per frame
    void Update () {
        h.increaseBy(100);
    }
}
```

这个例子要好得多。它提供了相同的功能，但是计算较少，因为GetComponent函数在开始时只调用一次，而不再每秒调用60次。

## 单位捷径性质
为了让我们的生活变得更简单，Unitity提供了一种不使用GetComponent函数访问一些标准组件的方法，例如：
- 变换
- 游戏对象

它们是这样使用的：

```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {

    // Use this for initialization
    void Start () {
    
    }
        
    // Update is called once per frame
    void Update () {
        Vector3 pos = transform.position;
    }
}
```
要知道这段代码需要大量的计算，需要了解一些关于Unity的知识。原因是它真正做的就是这样：

```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {

    // Use this for initialization
    void Start () {
    
    }
        
    // Update is called once per frame
    void Update () {
        Vector3 pos = GetComponent<Transform>.position;
    }
}

```

尽管它看起来不像它，但它仍然每次都使用GetComponent，正如上面提到的，计算起来很费时间。

正确的方法是这样做：

```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {
    // Cache
    Transform tr;

    // Use this for initialization
    void Start () {
        tr = transform;
    }
        
    // Update is called once per frame
    void Update () {
        Vector3 pos = tr.position;
    }
}
```


## GUIS
在使用guis时，最简单的方法通常是使用GUILayout类而不是GUI类，等级。遗憾的是，GUILayout类要比GUI类慢得多，所以请慎重考虑它是否值得。

## Shaders
Shaders可以使图形卡变得完全疯狂。除非你是一个Shaders专家，这是一个好主意，只使用那些Unity提供的Shaders。除此之外，还有带有“Mobile”后缀的Unity Shaders。这些着色器特别适用于手机，但它们也能在普通电脑上带来性能改进。

注意：Unity着色器制作精良，但计算起来仍然很昂贵。少用！


## 烘焙
我们做点饼干.。
统一有几个烘焙特点。例如，当我们想在游戏中有阴影时，首先想到的是这样的方法：
在每次Draw Call中：
- 1.灯的位置
- 2.画场景
- 3.画阴影
- 
这意味着在每次Draw Call中，阴影都会被一次又一次地计算出来。现在，烘焙的功能类似于我们上面使用的缓存。它只计算了一次阴影，并且已经在我们的纹理上绘制了它们，所以它们不需要一次又一次的计算。这个巨量性能优势。

尝试一下，这个特性可以在Window-Lightmap下面找到.有更多的烘焙方法，除了照明以外的东西，只要与他们一起玩，看看他们是否有益于你的游戏的表现。

## 动态灯
烘焙灯的缺点是，如果其中一个物体移动，它的阴影不会随着它移动，因为它很久以前就被烤到了纹理上。

烘焙灯的对立面叫做动态灯..物体移动时，阴影也会移动。

虽然这听起来很酷，但也有一个巨大的缺点：根据经验，每增加一盏灯，场景就必须再画一次。因此，如果我们在现场使用2个灯，我们可能会得到一半的FPS(以此类推)。

虽然这是一个经验法则和统一仍然是相当快，当涉及到绘制灯，这通常是一个好主意，使用尽可能少的灯。

## 内存分配
让我们看看下面的代码：


```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {
        
    // Update is called once per frame
    void Update () {
        SomeClass x = new SomeClass("Some", 123, "Parameters");
    }
}
```

在每个更新调用中，SomeClassx使用关键字创建(“已分配”)。新的..当使用新的关键字，United从我们的计算机中分配一些内存。当对象不再使用时，它将再次释放这个内存。如果经常这样做，这可能是一个非常昂贵的计算。

整个概念背后有更多的东西，但对我们来说，重要的是要记住，我们应该避免新的只要我们能。

另一种选择可能是这样的：


```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {
    // Cache
    SomeClass v = new SomeClass();
        
    // Update is called once per frame
    void Update () {
        v.setName("Some");
        v.setLevel(123);
        v.setSomethingElse("Parameters");
    }
}
```

## 摘要
性能是计算机科学研究的主要问题之一。我们讨论了增加游戏FPS的基本方法，而不使用任何可怕的数学。然而，一个很大的(如果不是最大的)部分在于程序员对数学、算法和数据结构的知识。这些话题可能是压倒性的，但是在你的旅途中，尽可能多地了解它们是很重要的。

顺便提一句：我们的其他文章通常不是为了保持它们尽可能容易理解而对性能进行优化。这取决于您应用本文中所学到的内容。
