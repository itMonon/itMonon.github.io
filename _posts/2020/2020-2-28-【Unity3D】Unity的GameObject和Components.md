---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity的GameObject和Components
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
Unity是一个基于组件的游戏引擎。本文将解释这是什么以及如何有效地使用它。

## 二、GameObject
在Unity中， GameObject几乎就是一个空对象。这是其他一切的基础。它只是一个有组件空间的原始对象。除非我们向GameObject添加组件，否则GameObject本身什么也不做。

## 三、Components
我们想在游戏中创造一个怪物。在“Unity”中，这样做的方式如下：

- 创建一个GameObject
- 添加三维模型Conponents
- 增加生命值Components
- 增加技能Components
- 增加一个移动Components
- 增加AI人工智能)Components
- 加几个音乐Components
- 添加用来显示名字GUIComponents

因此，基本上每次我们想要给我们的统一世界添加一些东西时，我们创建一个GameObject，然后向这个GameObject添加组件。在Unity中，组件可以是脚本、声音、网格(3D模型)、刚体、碰撞器等等。

在上面的例子中，生命、技能、运动、AI和GUI可能是脚本。这就提出了一个问题：为什么我们不创建一个Monster脚本并将其全部放入其中，而不是将所有内容分解为组件呢？

嗯，可以这样做。如果你喜欢，就做吧。但是想象一下：我们想在我们的游戏世界中创造另一个东西，这一次是玩家。我们将采取以下行动：

- 创建一个GameObject
- 添加三维模型Conponents
- 增加生命值Components
- 增加技能Components
- 增加一个移动Components
- 加几个音乐Components
- 添加用来显示名字GUI Components
- 增加一个经验Components
- 加一把剑的Components
- 加一把盾牌的Components

现在，基于组件的系统的美妙之处来了：我们可以重用我们用于怪物的组件！所以，我们要创建一个Player所要做的就是使用我们之前为怪物使用的大部分组件(比如Health)，将它们添加到播Player中，添加一些新的组件(比如剑和盾)，然后我们就完成了。与创建单个Player脚本相比，这将为我们节省大量的工作，在这里，我们将不得不重新编程所有的内容。

就这样，就这么简单。这只是一个聪明的方法来避免我们做太多的工作。

## 四、基于组件的开发：提示和技巧

**访问同一游戏对象上的组件**

因此，我们的玩家有两个脚本(也称为组件)：
- 生命脚本
- 移动脚本

具有当前最大生命值和当前生命值得组件可能如下所示：

```csharp
using UnityEngine;
using System.Collections;

public class Health : MonoBehaviour {
    public int current = 50;
    public int maximum = 100;
}
```
一个移动组件可以移动到玩家点击的地方，如下图所示:

```csharp
using UnityEngine;
using System.Collections;

public class Movement : MonoBehaviour {
    void Update() {
        if (clickedSomewhere()) {
            moveToDestination();
        }
    }
}
```
然后，我们将两个脚本拖到Player的GameObject上，这样看起来如下所示：
![在这里插入图片描述](https://noobtuts.com/content/unity/gameobjects-and-components/player_health_and_movement.png)
现在显然死了的人不能移动，那么当玩家死了的时候，我们如何阻止移动组件完成它的工作呢？我们需要一种让组件彼此沟通的方法。显然，他们是在同一个游戏对象(Player)，所以必须有一些方式。

好消息：有一个很简单的方法叫做GetComponent..下面是如何获取游戏对象：

```csharp
GetComponent<Health>()
```

注：这意味着“获取Health类型的组件”。
既然我们知道了这一点，我们就可以轻松地修改我们的移动脚本，这样它只能在玩家还没死的时候移动：

```csharp
using UnityEngine;
using System.Collections;

public class Movement : MonoBehaviour {
    void Update() {
        if (GetComponent<Health>().current > 0) {
            if (clickedSomewhere()) {
                moveToDestination();
            }
        }
    }
}
```
就这样。我们需要记住的是同GameObject可以通过GetComponent功能。

## 五、访问另一个游戏对象上的组件
好的，那么问题是，组件如何与其他游戏对象。例如，我们如何从玩家的脚本中获取怪物的生命值？

我们的GetComponent函数仅适用于同游戏对象，所以我们需要想出别的办法。

别担心，这也很容易。假设我们有一个Monster GameObject，它附带了上面的Health脚本：
![在这里插入图片描述](https://noobtuts.com/content/unity/gameobjects-and-components/monster.png)
我们有一个Test脚本：

```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {
    void Update() {
        // ToDo: is monster alive?
        // then do something
    }
}
```
它将被附加到玩家的游戏对象：
![在这里插入图片描述](https://noobtuts.com/content/unity/gameobjects-and-components/player_health_and_movement_and_test.png)
所以问题是，我们如何从玩家的Test脚本中获取怪物的生命值？

这也很容易。我们只需要使用一个公共类型的生命值变量这可以由任何类型的Health组成部分(在我们的例子中，它将是怪物的生命值)。这时候我们修改的Test.cs脚本：

```csharp
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour {
    public Health monsterHealth = null;

    void Update() {
        if (monsterHealth.current > 0) {
            // do something
        }
    }
}
```

如果我们再看一秒钟，我们很快就会意识到这根本是没有意义的。这个怪物健康组件是*NULL*，所以访问它是个坏主意。
但是等等，因为它是公众的，我们现在可以在“Unity的Inspector面板”中看到它的一个字段：
![在这里插入图片描述](https://noobtuts.com/content/unity/gameobjects-and-components/player_monsterhealth.png)
这意味着我们可以把一些东西拖到里面，这样它就不再是空的了。因此，让我们通过拖动怪物GameObject从Hierarchy 面板拖到我们的Test脚本的怪物生命值插槽：
![在这里插入图片描述](https://noobtuts.com/content/unity/gameobjects-and-components/monster_drag.png)
这个插槽看起来是这样的：
![在这里插入图片描述](https://noobtuts.com/content/unity/gameobjects-and-components/player_monsterhealth_filled.png)
这意味着我们公众生命值变量，指向怪物的生命值。因此我们Test剧本行得通。

这就是我们如何访问其他游戏对象上的组件！
