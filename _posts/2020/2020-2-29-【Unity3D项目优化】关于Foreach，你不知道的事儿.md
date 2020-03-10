---
layout: post
category: Unity3D-Opti
title: 【Unity3D项目优化】关于Foreach，你不知道的事儿
tagline: by 恬静的小魔龙
tag: Unity3D
---

**优化foreach循环**

## 最终目标：

- 这篇文章的主要目的是说明为什么要在Unity中避免使用foreach循环

- 你注意到游戏中出现的一些问题了吗？

- 是否是在循环遍历迭代中出现的？

- 你时常需要遍历许多GameObject列表吗？

- 如果你有很多像这样的问题，那么你就来对地方了！

 

“通常问题/由于在每一帧中GC（垃圾回收器）的高度回收所导致的，所以在解决这个问题之前，我们先来了解一下什么是GC（垃圾回收器）”

 

**什么是GC（垃圾回收器）？**

   1.GC（垃圾回收器）是任何一个计算机设备的内存管理系统中重要的一部分。其主要目的是尝试去回收或释放系统中程序不再使用的资源。

    

   2.这是一个自动化的系统，它确保了空闲的对象不再占用内存空间，这便充分优化了内存资源，提高了性能。尽管它是一个自动化的系统，但是还是可以在程序中对它进行控制。

    

   3.通常的，GC在进行回收处理时，要确保该对象在程序中不再使用，方才对该对象进行回收。

 

**那么我们该如何在Unity中使用foreach呢？**

让我们来列举一个例子：

**Step 1 在Unity中创建一个场景，如下图所示：**

![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48GhRD2EHp6cicoMhoePmlBtZpan7BR3VWbypwmThju2ZyYQvdKbibCtib5fyFoHibjVUeJYKYTMOqqicfQ/0?wx_fmt=png)
1.创建一个Canvas和一个Text如上图所示。

2.创建一个空的游戏物体（Empty Gameobject）并命名为GameObjectList

3.创建一些空物体（大约10-30个就可以），并绑定为GameObjectList的子物体

 

**Step 2 创建一个脚本，名字随你意愿来（可以得话就跟着教程来吧）：**

我给它命名为：ForEachLoopTest.cs

我偏好使用C#，如果你想使用Javascript也是可以的

```csharp
   public class ForEachLoopTest : MonoBehaviour

  {

      #region PUBLIC_DECLARATIONS

      public List<GameObject> emptyGameObjects;

      public Text indicatorText;

      #endregion

      #region UNITY_CALLBACKS

      void Update()

      {

          if (Input.GetKeyDown(KeyCode.Space))

          {

              indicatorText.text = "EXECUTION ON";

          }

          if (Input.GetKeyUp(KeyCode.Space))

          {

                indicatorText.text = "HOLD SPACE TO TURN ON EXECUTION";

          }

          if (Input.GetKey(KeyCode.Space))

          {

              UpdateTextValue();

          }

      }

      #endregion

      #region PUBLIC_METHODS

      public void UpdateTextValue()

      {

        

          foreach (var item in emptyGameObjects)

          {

              // PROCESS ITEMS IN LIST

          }

//        for (int i = 0; i < emptyGameObjects .Count; i++)

//        {

         // PROCESS ITEMS IN LIST

//        }

     }

     #endregion

}
```

**Note：**

这里我注释类for循环的代码，只留下了foreach循环的代码

**Step 3  指定引用和测试代码**
指定引用和测试代码请您跟着如下的步骤来执行：

   1.为GameObjectList添加ForEachLoopTest.cs‍脚本，并未emptyGameObjects指定GameObjectList中所有的子物体对象。

    

   2.现在执行Play游戏

   3.打开Profiler Window



你注意到Profile的变化了吗?

![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48GhRD2EHp6cicoMhoePmlBtZsVUfOm8cUKApkf4twNjiaeR7DF0zWFnXMr8ibev8As9ibxaB5Rtyso1Jw/0?wx_fmt=png)

**GC Alloction的值没有改变？觉得不可思议？
这到底是怎么回事呢？**

这个时候你会大喊着...“嘿，伙计，你在浪费我们的时间吗？我没有看到任何改变，除了一个数字（GC Alloction）之外”

在这种情况下，你是对的。但是现在你想像一下有这样一种情况，你必须要运行1000次不同的循环迭代，每一次循环都消耗一些GC！

这必定会在手机设备上照成CPU变慢，处理内存分配管理需要消耗大量处理能力，特别是GC（垃圾回收）。

现在，如果你不小心，继续在每一帧中进行循环遍历的话，程序必定卡死导致程序结束，这便说明你的游戏非常差，用户体验差。

所以应该尽量避免使用foreach循环，这将是一个明智的选择。

好吧，也许你还是对的，但是GC背后的原因是什么呢？

你一定很想知道，它只是一个循环！这个垃圾（Object）是从哪里回收的？

 

**首先让我们看看它的语法：**

```csharp
foreach (SomeType s in someList)

  s.DoSomething();
```
**现在编译，编译器将预处理代码如下：**

```csharp
 using (SomeType.Enumerator enumerator = this.someList.GetEnumerator())

{

  while (enumerator.MoveNext())

  {

  SomeType s = (SomeType)enumerator.Current;

  s.DoSomething();

  }

}
```

哇哦，太棒了，总而言之，foreach循环将会在每一次迭代中创建一个enumerator对象，并且迭代完成后便销毁那些对象。如果我使用字典或其他任何这样的集合。

 

这个时候GC便对这些销毁的对象进行回收，这便消耗了一定的CPU性能，照成了游戏变得迟钝，导致玩家心情变差。

 

Note：

GC的数量将取决于不同集合的类型的遍历。在我们的例子中，我们集合在GC Alloction中的显示为40B，但如果我使用Dicitionary（字典）或其它任何这样的集合，那么它的显示也是不同的。

 

Oh，我现在明白了！

我希望这是在你阅读完这篇文章之后的感叹！如果你还是不明白，于是乎我们得到了一个很简单的结论：那就是尽可能的在你的游戏中使用foreach循环。

最后我想对大家说的是：每个小的优化都有助于我们游戏的发展。

