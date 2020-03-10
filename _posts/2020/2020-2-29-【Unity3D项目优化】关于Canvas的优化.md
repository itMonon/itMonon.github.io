---
layout: post
category: Unity3D-Opti
title: 【Unity3D项目优化】关于Canvas的优化
tagline: by 恬静的小魔龙
tag: Unity3D
---

Canvas Setting
目标给你一个关于怎么去设置Canvas（画布）工作的建议
 

## Unity提供Canvas（画布）来创建UI。Canvas有以下三种渲染模式：

1. Screen Space Camera

2. Screen Space Overlay

3. World Space

 

现在，我们做一个简单的例子，让我们对这些选项一个一个的测试，以便我们能更好去了解它。

 

## 1.Screen Space Camera
 

**首先，我们创建一个Unity工程，并且进一步的理解它：**

**C#脚本: MoveCamera.cs**

1. 在你的Scene视图下创建一个Canvas

2. 设置它的Renderer mode（渲染模式）选项为：Screen Space - Camera

3. 拖拽Main Camera到该选项条上去

![9445LCT-6kKaVIejbYYWdQ==.png](http://mmbiz.qpic.cn/mmbiz/eP7zEV1evXic8K5z5s30N9sP090cYNSe97j4tian8HHSUrhNic43Dvo5JEFZnelM1eMwtbAaKfoTsbMpWdThGBGMw/0)

4. 在Canvas下创建一个Panel，且随便拿一张图片或纹理作为背景

5. 在Canvas下创建尽可能多的Text、Panel、Image

![hOZx22xcuEe0FxY_5ZRReg==.png](http://mmbiz.qpic.cn/mmbiz/eP7zEV1evXic8K5z5s30N9sP090cYNSe9S4CLOH8aIwYGcqicbrvia3YtqPGyf97aDGZ3RUoIWbyJaoak3fzTmCDA/0)

6. 为Main Camera添加如下脚本

 

  

```csharp
  using UnityEngine;

  using System.Collections;

  public class MoveCamera : MonoBehaviour

  {

      private float velocity = 0.0f;

      private float smoothTime = 0.3f;

      private bool moveCamera = false;

      public Vector3 initialPosition;

      public Vector3 targetPosition;

      public float lerpSpeed;

      public float initialZ;

      public float targetZ;

      public Camera cam;

      void Update ()

      {

          if (Input.GetMouseButtonDown (0)) {

              initialPosition = transform.position;

              targetPosition = new Vector3 (transform.position.x + Random.Range (-5, 5), transform.position.y +                 Random.Range (-5, 5), transform.position.z);

              initialZ = transform.eulerAngles.z;

              targetZ = initialZ + Random.Range (-50, 50);

              moveCamera = true;

              lerpSpeed = 0;

          }

          if (moveCamera) {

              CameraMovementMethod ();

          }

      }

        private void CameraMovementMethod ()

      {

          lerpSpeed = Mathf.SmoothDamp (lerpSpeed, 1.0f, ref velocity, smoothTime);

          cam.transform.position = Vector3.Lerp (initialPosition, targetPosition, lerpSpeed);

          cam.transform.eulerAngles = new Vector3 (0, 0, Mathf.LerpAngle (initialZ, targetZ, lerpSpeed));

      }

  }

```

 

7. 为脚本中的Cam参数赋值（把Main Camera拖拽上去）

 

![X8AoOmvrp0a_RuPrOJINtg==.png](http://mmbiz.qpic.cn/mmbiz/eP7zEV1evXic8K5z5s30N9sP090cYNSe9avk5v4T8MKgiaicBfe8aichOYPGZaUZ5Z8uichd098No4PqmicJLGLzss0g/0)
8. 使用安卓手机连接Profiler进行测试（如果不会的朋友，请观看手机连接Profiler教程篇；如果真的很懒的话，那就直接播放，然后这在拖动Transfrom上的Position或者Rotation改变它的值即可）

![在这里插入图片描述](http://mmbiz.qpic.cn/mmbiz/LJ2FKOSh48HwSd8AtmDmfrBhLQ9YCJAZ5ZpmMeGYESoBqlkThZAKQPsnq0icEGCpvTRicoBjBa6Xc5gtyTOGF5yQ/0?wx_fmt=png)
9. 在安卓手机上运行该项目，点击运行（摇晃手机，相机将会在任意一个方向或角度上旋转或移动），并且注意看Prefiler里的性能分析

10. 在Profiler里的Search中输入“Canvas”，你将会在Profiler中看到一些Canvas数据。

![DhmN7Xcxi0CPZOuDtydcGA==.png](http://mmbiz.qpic.cn/mmbiz/eP7zEV1evXic8K5z5s30N9sP090cYNSe9uF91nPo6Qia7ZbbicDCqZxyUd0rZR23TpejGCUicoUM5OChsvwDDADViaw/0)
 

你可以在上图中看到，有许多与Canvas的函数都被调用了，特别是CanvasRender.OnTransformChanged调用的次数最多

 

每当相机移动或者旋转，与Canvas相关的函数就会被调用将近50次。

 

NoteCalls的数值取决于在Canvas里使用UI元素的数量
 

**我们可以更好的理解下面这张GIF图：**



![zo6VMdggJEuHj-Bi8g6b0g==.gif](http://mmbiz.qpic.cn/mmbiz/eP7zEV1evXic8K5z5s30N9sP090cYNSe9QrrKMWYNEpDS1ibe3X3m9zqc9LWx3enh8xqo1KTJ6g1LeBfgDtluVUw/0)
如上图所示，在游戏场景中Canvas跟随相机一起移动。因此，这便导致在Canvas里的UI元素在Unity引擎中必须重新定位，所以UI元素越多，需要消耗的性能就越高。

 


那么解决方案是什么?
## 2. Screen Space Overlay
 

![95b2AyFFwkGBglxtdQPwiw==.png](http://mmbiz.qpic.cn/mmbiz/eP7zEV1evXic8K5z5s30N9sP090cYNSe9O3g15puLn2bTZ15OGqVXqbcvJe0Ku6wUibK48icp7CTQ6WQMVcpntARA/0)
现在，改变Canvas的渲染模式为 Screen Space Overlay，并且重复上面的步骤，然后在Profiler的Search中输入Canvas，注意数据的变化

![jeHVTp2RukGfACoAN3ITGA==.gif](http://mmbiz.qpic.cn/mmbiz/eP7zEV1evXic8K5z5s30N9sP090cYNSe9LodNoLEjlWsAyw1sSxFLAsfg3aUXLIUTepp6BCM4Sh3GodicLQAFjtw/0)
 

注意Calls数据的变化，是否已经减少了？现在Calls显示的数据加起来差不多等于前面Screen Space Camera模式所显示的Calls数据的个位数。

 

哈哈，这么一来我们优化了大约90%的性能，是不是很神奇啊？


   如上图所示，Canvas在Unity空间的位置保持不变，相机的移动不会影响Canvas及Canvas里的所有UI元素。（它能会静静的在那里装逼，动都不动了）

   因此，就不再需要为Canvas里的UI元素重新定位，这便减少了Calls的次数，优化了性能

   这样我们的优化任务算是完成了 

 

## 3.World Space
 

  在World Space渲染模式下，Canvas渲染对于每一个视图必须手动的管理，这造成的Calls和Screen Space Camera模式所造成Calls数量差不多，因此不再详述。

 

总结：

  Screen Space Camera模式 和 World Space模式都会造成大量的Calls，所以还是建议大家使用Screen Space Overlay模式，这样有利于性能的优化，提高了游戏的可玩度。

