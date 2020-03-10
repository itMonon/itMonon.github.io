---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity的预制体
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
这篇短文解释了什么是Unity的预制体和它们的用处

## 二、用途
让我们想象一下，我们想在Unity中做一个塔防游戏。首先，我们创建一个Tower GameObject，它在层次结构中如下所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS9wcmVmYWJzL2hpZXJhcmNoeS5wbmc)
当玩家开始游戏时，通常场景中还没有塔。我们需要的是一种方法，将我们的塔游戏对象保存到某个地方，然后当玩家想要建造一个塔时，将它们再次加载到层次结构中。

这就是预制体的用途。

## 三、创建预制体
让我们创建一个预制件。非常简单，我们所要做的就是将GameObject从层次结构中拖到Project区域中，如下所示：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS9wcmVmYWJzL2NyZWF0ZV9wcmVmYWIucG5n)
之后，我们可以在我们的项目区看到预制件：

.![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS9wcmVmYWJzL3ByZWZhYi5wbmc)
现在我们可以从层次结构中删除GameObject，这样它就不在游戏世界中了。但由于它在项目区域，我们不会完全失去它。

## 四、加载预制体
如果我们想装载预制件，我们有两个选择。我们可以手动地将它从ProjectArea拖到层次结构区域，或者使用调用实例化.

下面是一个示例脚本，游戏一开始就加载Prefab：

```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {
    public GameObject prefab = null;
                
    void Start () {
        // instantiate the prefab
        // -> transform.position means that it will be
        //    instantiated at the position of *this*
        //    gameobject
        // -> quaternion.identity means that it will
        //    have the default rotation, hence its not
        //    rotated in any weird way
        Instantiate(prefab,
                    transform.position,
                    Quaternion.identity);
    }
}
```
现在我们要做的就是将测试脚本添加到场景中的GameObject中，查看Inspector，然后将public prefab变量设置为来自我们项目区域的任何prefab，如下所示:

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS9wcmVmYWJzL3NldF9wdWJsaWNfcHJlZmFiLnBuZw)
按下Play后，它立即将我们的塔预置体加载到游戏场景。

就这么简单！

