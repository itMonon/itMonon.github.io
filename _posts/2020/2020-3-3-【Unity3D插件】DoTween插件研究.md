---
layout: post
category: Unity3D-Plugin
title: 【Unity3D插件】DoTween插件研究
tagline: by 恬静的小魔龙
tag: Unity3D
---

@[toc]
## 一、前言
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RvdHdlZW4uZGVtaWdpYW50LmNvbS9faW1ncy9sb2dvcy9kb3R3ZWVucHJvX2hvdHdlZW52Mi5wbmc?x-oss-process=image/format,png)
**DOTween**是一个用于Unity的快速、高效、完全类型安全的面向对象动画引擎，为c#用户进行了优化，是免费和开源的，具有大量高级特性

**DOTween**兼容Unity 2019至4.6版本。
适用于:
**Win, Mac, Linux, Unity WebPlayer, WebGL, iOS, Android，
Windows Phone, Windows Store, PS Vita (PSM)， PS3/PS4, Xbox 360/One，任天堂Switch + more**(没有测试额外的平台，但除了Flash导出，它应该可以在任何地方工作)

**DOTweenPro**
使用新的脚本快捷键、可视化动画编辑器、可视路径编辑器以及额外的特性扩展DOTween Pro。2D工具包和TextMesh Pro.
DoTweenPro是收费的

## 二、下载
官方下载地址：
http://dotween.demigiant.com/downloads/DOTween_1_2_280.zip
下载页面：
http://dotween.demigiant.com/download.php
开放源代码：
https://github.com/Demigiant/dotween

## 三、安装步骤
### 步骤一：导入设置
首先下载完成后，将压缩包解压到任何地方，将解压后的文件放到你的项目文件目录Assets 下面（只是不要放到Editor、Plugins或Resources目录中）
**设置**
在导入新的DoTween之后，你必须为了基于您的Unity版本的导入/重新导入附加库设置DoTween，并激活/停用模块。
要设置DoTween，需要打开Unity中的DoTween面板，
从Tools/Demigiant 选择Setup DOTween
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190927103909724.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

### 步骤二：导入命名空间
导入DOTween命名空间在每个要使用它的类/脚本中：

```csharp
using DG.Tweening;
```
### 步骤三(可选)：全局设置
初始化DOTWIN若要设置一些全局选项，请执行以下操作：

```csharp
DOTween.Init(autoKillMode, useSafeMode, logBehaviour);
```
如果您不这样做(或者在创建第一个DOTween 之后这样做)，那么DOTween将使用默认设置自动初始化。

## 四、全局和特定设置
你可以全局设置它将应用于所有新创建的tweens，或强迫性设置。特定设置将应用于你所创建的特定的tweent

### 全局设置
全局设置允许您设置默认的autoPlay 和autoKill 行为、ease Type、全局时间缩放等等。
### 特定设置
特定设置，通过链接操作，可以设置ease Type类型，回调函数，循环次数，因此智能感知可以帮助你找到它们。

在这里，举几个链接操作的例子：

```csharp
// 创建一个transform补间，并设置它的ease、loops和OnComplete回调
﻿﻿﻿﻿﻿transform.DOMove(new Vector3(2,2,2), 2).SetEase(Ease.OutQuint).SetLoops(4).OnComplete(myFunction);

﻿﻿﻿﻿﻿// 和上面一样，但是使用换行符使它更易于阅读
﻿﻿﻿﻿﻿transform.DOMove(new Vector3(2,2,2), 2)
﻿﻿﻿﻿﻿  .SetEase(Ease.OutQuint)
﻿﻿﻿﻿﻿  .SetLoops(4)
﻿﻿﻿﻿﻿  .OnComplete(myFunction);

﻿﻿﻿﻿﻿// 和上面一样，但是存储补间和应用设置没有链接
﻿﻿﻿﻿﻿Tween myTween = transform.DOMove(new Vector3(2,2,2), 2);
﻿﻿﻿﻿﻿myTween.SetEase(Ease.OutQuint);
﻿﻿﻿﻿﻿myTween.SetLoops(4);
﻿﻿﻿﻿﻿myTween.OnComplete(myFunction);
```
另外，一些tween 类型特别附加选项，比如立即进行链接操作，可以通过SetOptions()函数，记住SetOptions()是特别的，主要作用就是在transform进行补间动画的时候，立即链接操作。看一下例子：

```csharp
﻿﻿﻿﻿﻿transform.DOMove(new Vector3(2,2,2), 2)
﻿﻿﻿﻿﻿  .SetOptions(true)
﻿﻿﻿﻿﻿  .SetEase(Ease.OutQuint)
﻿﻿﻿﻿﻿  .SetLoops(4)
﻿﻿﻿﻿﻿  .OnComplete(myFunction);
```
另外，可以使用SetAs，将当前所有设置，从一个中间复制到另一个，而不用再复制好多的代码：

```csharp
// 使用一些设置创建tween并将其存储为tween
﻿﻿﻿﻿﻿Tween myTween = transform.DOMove(new Vector3(2,2,2), 2)
﻿﻿﻿﻿﻿  .SetEase(Ease.OutQuint)
﻿﻿﻿﻿﻿  .SetLoops(4)
﻿﻿﻿﻿﻿  .OnComplete(myFunction);
﻿﻿﻿﻿﻿
﻿﻿﻿﻿﻿// 创建另一个tween，并应用与前一个相同的设置
﻿﻿﻿﻿﻿material.DOColor(Color.red, 2).SetAs(myTween);
```
## 五、缓存和最大Tween

为了避免不必要地使用更多的资源，它将自己设置为最多200个Tweener和50个同时运行的序列。如果您需要更多，DOTween将自动增加其大小，但您也可以直接设置当自动调整大小：

```csharp
// 设置最大渐变为3000和最大序列为200
﻿﻿﻿﻿﻿DOTween.SetTweensCapacity(3000, 200);
```
## 六、开始使用Tween
DOTween 可用于完全通用方式，就像这样：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RvdHdlZW4uZGVtaWdpYW50LmNvbS9faW1ncy9zcGxhc2hfbGFtYmRhLnBuZw?x-oss-process=image/format,png)
或者你可以利用它的快捷键，就像这样：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RvdHdlZW4uZGVtaWdpYW50LmNvbS9faW1ncy9zcGxhc2hfc2hvcnRjdXRzLnBuZw?x-oss-process=image/format,png)
## 七、创建DOTween
到目前为止，DOTween 可以在这些类型的值之间转换：
float, double, int, uint, long, ulong, Vector2/3/4, Quaternion, Rect, RectOffset, Color, string
(其中一些值可以在特殊方式) 

此外，您还可以创建自定义DOTween插件在自定义值类型之间切换：

创建DOTween的方法有三种：一般方式、快捷键、其他通用方式

### A.一般方式
这是最灵活的创建方式，可以让你在所有的值之间转换。公共或私人、静态或动态：
例子：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190927111712566.png)
getter：一个委托，它将属性值返回为tween。可以像这样写成一个样子：()=> myValue， myValue属性：的名称。
setter：将属性值设置为渐变的委托。可以写成这样的形式：x=> myValue = x 
to：要达到的最终价值。
duration：过渡的持续时间。

**实例：**

```csharp
// 在1秒内将一个叫做myVector的向量补间到(3,4,8)
﻿﻿﻿﻿﻿﻿﻿DOTween.To(()=> myVector, x=> myVector = x, new Vector3(3,4,8), 1);
﻿﻿﻿﻿﻿﻿﻿// 在1秒内将一个名为myFloat的浮点数补间到52
﻿﻿﻿﻿﻿﻿﻿DOTween.To(()=> myFloat, x=> myFloat = x, 52, 1);
```
### B、快捷键
DOTween 可以控制已知的Unity对象，比如Transform、刚体、material等。可以直接在这些对象的引用时使用快捷键进行操作，例如：

```csharp
transform.DOMove(new Vector3(2,3,4), 1);
﻿﻿﻿﻿﻿rigidbody.DOMove(new Vector3(2,3,4), 1);
﻿﻿﻿﻿﻿material.DOColor(Color.green, 1);
```
#### 基本快捷键
**AudioMixer (Unity 5)**
- DOSetFloat(string floatName, float to, float duration)

**AudioSource**
- DOFade(float to, float duration)
- DOPitch(float to, float duration)
**Camera**
- DOAspect(float to, float duration)
- DOColor(Color to, float duration)
- DOFarClipPlane(float to, float duration)
- DOFieldOfView(float to, float duration)
- DONearClipPlane(float to, float duration)
- DOOrthoSize(float to, float duration)
- DOPixelRect(Rect to, float duration)
- DORect(Rect to, float duration)
- DOShakePosition(float duration, float/Vector3 strength, int vibrato, float randomness, bool fadeOut)
- DOShakeRotation(float duration, float/Vector3 strength, int vibrato, float randomness, bool fadeOut)
**Light**
- DOColor(Color to, float duration)
- DOIntensity(float to, float duration)
- DOShadowStrength(float to, float duration)
- Blendable tweens
- DOBlendableColor(Color to, float duration)
**LineRenderer**
- DOColor(Color2 startValue, Color2 endValue, float duration)
**Material**
- DOColor(Color to, float duration)
- DOColor(Color to, string property, float duration)
- DOColor(Color to, int propertyID, float duration)
- DOFade(float to, float duration)
- DOFade(float to, string property, float duration)
- DOFade(float to, int propertyID, float duration)
- DOFloat(float to, string property, float duration)
- DOFloat(float to, int propertyID, float duration)
- DOGradientColor(Gradient to, float duration)
- DOGradientColor(Gradient to, string property, float duration)
- DOGradientColor(Gradient to, int propertyID, float duration)
- DOOffset(Vector2 to, float duration)
- DOOffset(Vector2 to, string property, float duration)
- DOOffset(Vector2 to, int propertyID, float duration)
- DOTiling(Vector2 to, float duration)
- DOTiling(Vector2 to, string property, float duration)
- DOTiling(Vector2 to, int propertyID, float duration)
- DOVector(Vector4 to, string property, float duration)
- DOVector(Vector4 to, int propertyID, float duration)
**Blendable tweens**
- DOBlendableColor(Color to, float duration)
- DOBlendableColor(Color to, string property, float duration)
- DOBlendableColor(Color to, int propertyID, float duration)
**Rigidbody**
这些快捷键在背景中使用刚体的移动定位/移动旋转方法，以正确地动画与物理物体相关的事物。
**Move**
- DOMove(Vector3 to, float duration, bool snapping)
- DOMoveX/DOMoveY/DOMoveZ(float to, float duration, bool snapping)
- DOJump(Vector3 endValue, float jumpPower, int numJumps, float duration, bool snapping)
**endValue:终点值
jumpPower:跳跃的力度
numJumps:跳跃的高度
duration：持续时间
snapping:如果为真，补间将顺利地将所有值捕捉到整数。**
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2RvdHdlZW4uZGVtaWdpYW50LmNvbS9faW1ncy9jb250ZW50L2RvdHdlZW5fZG9qdW1wLmdpZg)
**Rotate**
- DORotate(Vector3 to, float duration, RotateMode mode)
- DOLookAt(Vector3 towards, float duration, AxisConstraint axisConstraint = AxisConstraint.None, Vector3 up = Vector3.up)
- DOSpiral(float duration, Vector3 axis = null, SpiralMode mode = SpiralMode.Expand, float speed = 1, float frequency = 10, float depth = 0, bool snapping = false)

**Rigidbody2D**
这些快捷方式在后台使用rigidbody2D的MovePosition/MoveRotation方法来正确地动画与物理对象相关的东西。
**Move**
- DOMove(Vector2 to, float duration, bool snapping)
- DOMoveX/DOMoveY(float to, float duration, bool snapping)
- DOJump(Vector2 endValue, float jumpPower, int numJumps, float duration, bool snapping)
**Rotate**
- DORotate(float toAngle, float duration)
**SpriteRenderer**
- DOColor(Color to, float duration)
- DOFade(float to, float duration)
- DOGradientColor(Gradient to, float duration)
**Blendable tweens**
- DOBlendableColor(Color to, float duration)
**TrailRenderer**
- DOResize(float toStartWidth, float toEndWidth, float duration)
- DOTime(float to, float duration)
**Transform**
**Move**
- DOMove(Vector3 to, float duration, bool snapping)
- DOMoveX/DOMoveY/DOMoveZ(float to, float duration, bool snapping)
- DOLocalMove(Vector3 to, float duration, bool snapping)
- DOLocalMoveX/DOLocalMoveY/DOLocalMoveZ(float to, float duration, bool snapping)
- DOJump(Vector3 endValue, float jumpPower, int numJumps, float duration, bool snapping)
- DOLocalJump(Vector3 endValue, float jumpPower, int numJumps, float duration, bool snapping)
**Rotate**
- DORotate(Vector3 to, float duration, RotateMode mode)
- DORotateQuaternion(Quaternion to, float duration)
- DOLocalRotate(Vector3 to, float duration, RotateMode mode)
- DOLocalRotateQuaternion(Quaternion to, float duration)
- DOLookAt(Vector3 towards, float duration, AxisConstraint axisConstraint = AxisConstraint.None, Vector3 up = Vector3.up)
**Scale**
- DOScale(float/Vector3 to, float duration)
- DOScaleX/DOScaleY/DOScaleZ(float to, float duration)
- DOPunchPosition(Vector3 punch, float duration, int vibrato, float elasticity, bool snapping)
- DOPunchRotation(Vector3 punch, float duration, int vibrato, float elasticity)
- DOPunchScale(Vector3 punch, float duration, int vibrato, float elasticity)
**Shake**
- DOShakePosition(float duration, float/Vector3 strength, int vibrato, float randomness, bool snapping, bool fadeOut)
- DOShakeRotation(float duration, float/Vector3 strength, int vibrato, float randomness, bool fadeOut)
- DOShakeScale(float duration, float/Vector3 strength, int vibrato, float randomness, bool fadeOut)
**Path**
- DOPath(Vector3[] waypoints, float duration, PathType pathType = Linear, PathMode pathMode = Full3D, int resolution = 10, Color gizmoColor = null)
- DOLocalPath(Vector3[] waypoints, float duration, PathType pathType = Linear, PathMode pathMode = Full3D, int resolution = 10, Color gizmoColor = null)
**Blendable tweens**
- DOBlendableMoveBy(Vector3 by, float duration, bool snapping)
- DOBlendableLocalMoveBy(Vector3 by, float duration, bool snapping)
- DOBlendableRotateBy(Vector3 by, float duration, RotateMode mode)
- DOBlendableLocalRotateBy(Vector3 by, float duration, RotateMode mode)
- DOBlendableScaleBy(Vector3 by, float duration)
- DOSpiral(float duration, Vector3 axis = null, SpiralMode mode = SpiralMode.Expand, float speed = 1, float frequency = 10, float depth = 0, bool snapping = false)
- DOTimeScale(float toTimeScale, float duration)

**Unity UI 4.6 快捷键**

**CanvasGroup (Unity UI 4.6)**
- DOFade(float to, float duration)
**Graphic (Unity UI 4.6)**
- DOColor(Color to, float duration)
- DOFade(float to, float duration)
**Blendable tweens**
- DOBlendableColor(Color to, float duration)
**Image (Unity UI 4.6)**
- DOColor(Color to, float duration)
- DOFade(float to, float duration)
- DOFillAmount(float to, float duration)
- DOGradientColor(Gradient to, float duration)
Blendable tweens
- DOBlendableColor(Color to, float duration)
**LayoutElement (Unity UI 4.6)**
- DOFlexibleSize(Vector2 to, float duration, bool snapping)
- DOMinSize(Vector2 to, float duration, bool snapping)
- DOPreferredSize(Vector2 to, float duration, bool snapping)
**Outline (Unity UI 4.6)**
- DOColor(Color to, float duration)
- DOFade(float to, float duration)
**RectTransform (Unity UI 4.6)**
- DOAnchorMax(Vector2 to, float duration, bool snapping)
- DOAnchorMin(Vector2 to, float duration, bool snapping)
- DOAnchorPos(Vector2 to, float duration, bool snapping)
- DOAnchorPosX/DOAnchorPosY(float to, float duration, bool snapping)
- DOAnchorPos3D(Vector3 to, float duration, bool snapping)
- DOAnchorPos3DX/DOAnchorPos3DY/DOAnchorPos3DZ(float to, float duration, bool snapping)
- DOJumpAnchorPos(Vector2 endValue, float jumpPower, int numJumps, float duration, bool snapping)
- DOPivot(Vector2 to, float duration)
- DOPivotX/DOPivotY(float to, float duration)
- DOPunchAnchorPos(Vector2 punch, float duration, int vibrato, float elasticity, bool snapping)
- DOShakeAnchorPos(float duration, float/Vector3 strength, int vibrato, float randomness, bool snapping, bool fadeOut)
- DOSizeDelta(Vector2 to, float duration, bool snapping)
**ScrollRect (Unity UI 4.6)**
- DONormalizedPos(Vector2 to, float duration, bool snapping)
- DOHorizontalNormalizedPos(float to, float duration, bool snapping)
- DOVerticalPos(float to, float duration, bool snapping)
**Slider (Unity UI 4.6)**
- DOValue(float to, float duration, bool snapping = false)
**Text (Unity UI 4.6)**
- DOColor(Color to, float duration)
- DOFade(float to, float duration)
- DOText(string to, float duration, bool richTextEnabled = true, ScrambleMode scrambleMode = ScrambleMode.None, string scrambleChars = null)
Blendable tweens
- DOBlendableColor(Color to, float duration)
**PRO ONLY ➨ 2D Toolkit 快捷键**
- DOScale(Vector3 to, float duration)
- DOScaleX/Y/Z(float to, float duration)
- DOColor(Color to, float duration)
- DOFade(float to, float duration)
**2dSlicedSprite**
- DOScale(Vector2 to, float duration)
- DOScaleX/Y(float to, float duration)
**2dTextMesh**
- DOColor(Color to, float duration)
- DOFade(float to, float duration)
- DOText(string to, float duration, bool richTextEnabled = true, ScrambleMode scrambleMode = ScrambleMode.None, string scrambleChars = null)
- 
**PRO ONLY ➨ TextMesh Pro 快捷键**
**TextMeshPro + TextMeshProUGUI**
- DOScale(float to, float duration)
- DOColor(Color to, float duration)
- DOFaceColor(Color to, float duration)
- DOFaceFade(float to, float duration)
- DOFade(float to, float duration)
- DOFontSize(float to, float duration)
- DOGlowColor(Color to, float duration)
- DOMaxVisibleCharacters(int to, float duration)
- DOOutlineColor(Color to, float duration)
- DOText(string to, float duration, bool richTextEnabled = true, ScrambleMode scrambleMode = ScrambleMode.None, string scrambleChars = null)


### C.其他通用方式
这些是允许以特定方式在值之间切换的额外的泛型方法。
- static DOTween.Punch(getter, setter, Vector3 direction, float duration, int vibrato, float elasticity)
- 指向给定的方向，然后回到起始方向，就像它通过弹性连接到起始位置一样。
- static DOTween.Shake(getter, setter, float duration, float/Vector3 strength, int vibrato, float randomness, bool ignoreZAxis)
- 摇动Vector3沿着它的X，Y轴与给定的值。
- static DOTween.ToAlpha(getter, setter, float to, float duration)
- 对Color的alpha 从它的当前值到给定的值。
- static DOTween.ToArray(getter, setter, float to, float duration)
- 将向量3转换为给定的结束值。易用性适用于每一段之间，而不是作为一个整体。
- static DOTween.ToAxis(getter, setter, float to, float duration, AxisConstraint axis)
Virtual Tween
单一轴Vector3从它的当前值到给定的值。
- static DOTween.To(setter, float startValue, float endValue, float duration)
将虚拟属性从给定的起始值改为给定的结束值，并实现一个setter，它允许使用外部方法或lambda来使用该值。


## 八、序列帧动画
### 1.创建序列

```csharp
//返回一个可用的序列，您可以将其存储并添加渐变。
Sequence mySequence = DOTween.Sequence();
```

### 2. 在你序列中添加动画、间隔和回调
注意所有这些方法需要在序列开始之前应用。意思就是序列在程序开始前就要设置好，不能动态的添加序列。
还请注意任何嵌套的tweener/序列都需要在将其添加到序列之前完全创建。因为在那之后它会被锁上。
延迟和循环(当不是无限的时候)即使在嵌套的tweens中也能工作。

**API：**
- Append(Tween tween)

```csharp
//将给定的tween添加到序列的末尾
mySequence.Append(transform.DOMoveX(45, 1));
```

- AppendCallback(TweenCallback callback)

```csharp
//将给定的回调函数添加到序列的末尾。
mySequence.AppendCallback(MyCallback);
```

- AppendInterval(float interval)

```csharp
//将给定的区间添加到序列的末尾。
mySequence.AppendInterval(interval);
```

- Insert(float atPosition, Tween tween)

```csharp
//在给定的时间位置插入给定的tween，从而允许您重叠tween，而不是将它们一个接一个地播放。
mySequence.Insert(1, transform.DOMoveX(45, 1));
```

- InsertCallback(float atPosition, TweenCallback callback)

```csharp
//在给定的时间位置插入给定的回调函数。
mySequence.InsertCallback(1, MyCallback);
```

- Join(Tween tween)

```csharp
//在添加到序列的最后一个补间或回调的同一时间位置插入给定的补间。
// 旋转动画将与移动动画一起播放
﻿﻿﻿﻿﻿﻿﻿mySequence.Append(transform.DOMoveX(45, 1));
﻿﻿﻿﻿﻿﻿﻿mySequence.Join(transform.DORotate(new Vector3(0,180,0), 1));
```

- Prepend(Tween tween)

```csharp
//将给定的补间添加到序列的开头，按时间推进其余内容
mySequence.Prepend(transform.DOMoveX(45, 1));
```

- PrependCallback(TweenCallback callback)

```csharp
//将给定的回调函数添加到序列的开头。
mySequence.PrependCallback(MyCallback);
```

- PrependInterval(float interval)

```csharp
//将给定的区间添加到序列的开头，按时间推进其余内容。
mySequence.PrependInterval(interval);
```

提示：您可以创建仅由回调组成的序列，并将它们用作计时器或类似的东西。


实例
创建序列

```csharp
// 获取一个可用的自由序列
﻿﻿﻿﻿﻿Sequence mySequence = DOTween.Sequence();
﻿﻿﻿﻿﻿// 在开始时添加一个移动补间
﻿﻿﻿﻿﻿mySequence.Append(transform.DOMoveX(45, 1));
﻿﻿﻿﻿﻿// 在前一个补间完成后立即添加一个旋转补间
﻿﻿﻿﻿﻿mySequence.Append(transform.DORotate(new Vector3(0,180,0), 1));
﻿﻿﻿﻿﻿// 将整个序列延迟1秒
﻿﻿﻿﻿﻿mySequence.PrependInterval(1);
﻿﻿﻿﻿﻿// 在序列的整个过程中插入一个缩放补间
﻿﻿﻿﻿﻿mySequence.Insert(0, transform.DOScale(new Vector3(3,3,3), mySequence.Duration()));
```

与前面的示例相同，但使用链接(加号换行以使事情更清晰)：

```csharp
Sequence mySequence = DOTween.Sequence();
﻿﻿﻿﻿﻿mySequence.Append(transform.DOMoveX(45, 1))
﻿﻿﻿﻿﻿  .Append(transform.DORotate(new Vector3(0,180,0), 1))
﻿﻿﻿﻿﻿  .PrependInterval(1)
﻿﻿﻿﻿﻿  .Insert(0, transform.DOScale(new Vector3(3,3,3), mySequence.Duration()));
```
## 九、设置、选项和回调
当涉及到将设置应用到DOTween 时，DOTween使用了一种链式方法。或者您可以更改将应用于所有新创建的tween的全局默认选项。

### 1.全局设置
**一般设置**
- static LogBehaviour DOTween.logBehaviour
根据所选择的模式，DOTween将只记录错误、错误和警告，或者所有内容和附加信息。
LogBehaviour.ErrorsOnly: Logs errors and nothing else. 
LogBehaviour.Default: Logs errors and warnings. 
LogBehaviour.Verbose: Logs errors, warnings and additional information.
- static bool DOTween.maxSmoothUnscaledTime
- static bool DOTween.nestedTweenFailureBehaviour
- static bool DOTween.onWillLog<LogType,object>
- static bool DOTween.showUnityEditorReport
- static float DOTween.timeScale
- static bool DOTween.useSafeMode
- static bool DOTween.useSmoothDeltaTime
- static DOTween.SetTweensCapacity(int maxTweeners, int maxSequences)
**应用于所有新创建的Tweens的设置**
- static bool DOTween.defaultAutoKill
- static AutoPlay DOTween.defaultAutoPlay
- static float DOTween.defaultEaseOvershootOrAmplitude
- static float DOTween.defaultEasePeriod
- static Ease DOTween.defaultEaseType
- static LoopType DOTween.defaultLoopType
- static bool DOTween.defaultRecyclable
- static bool DOTween.defaultTimeScaleIndependent
- static UpdateType DOTween.defaultUpdateType
### 2.Tweener和序列设置
**实例属性**
float timeScale;
提示：这个值可以设置从这个动画到另一个动画的平滑效果
使用：

```csharp
myTween.timeScale = 0.5f;
```

**链式设置**
这些设置可以设置到所有类型的动画
也可以在运行的时候设置
- SetAs(Tween tween \ TweenParams tweenParams)
作用：将已经设置好的动画参数复制给另一个动画
- SetAutoKill(bool autoKillOnCompletion = true)
作用：当autoKillOnCompletion为true的时候，动画完成后会立即销毁
注：默认情况下，Tweens在完成的时候会自动终止，所以这个方法只有在设置不自动终止的时候生效
- SetEase(Ease easeType \ AnimationCurve animCurve \ EaseFunction customEase)
作用：设置渐变的easeType类别。
注意：Ease.Flash/InFlash/OntFlas/InOutFlash，将适用于属性闪烁的效果
例子：iamge.DoColor(Color color,float time).SetEase(Ease easeType,int overshoot,int period)
overshoot:指示要应用的闪烁总数。偶数将在起始值上结束吐温，而奇数将结束于结束值。
period:指示放松时间内的功率，并且必须介于-1和1之间。
0是平衡的，1完全削弱了时间的舒适度，-1开始的轻松完全削弱，并给予它的力量，直到最后
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191009150044950.gif)
- SetId(object id)
作用：设置动画的ID(然后可以使用DOTween的静态方法作为过滤器使用)。它可以是一个int，一个字符串，一个对象或其他什么。
注：使用int或stringID使过滤操作更快(其中int也比string).
例：transform.DOMoveX(4, 1).SetId("supertween");
- SetLink(GameObject target, LinkBehaviour linkBehaviour = LinkBehaviour.KillOnDestroy)
作用：将此链接到GameObject，并根据其活动状态分配行为。这也会导致在GameObject被摧毁时，会自动销毁动画。
注：如果将动画添加到序列中，则无效。
例：transform.DOMoveX(4, 1).SetLink(aGameObject, LinkBehaviour.PauseOnDisableRestartOnEnable);
- SetLoops(int loops, LoopType loopType = LoopType.Restart)
作用：设置动画的循环选项，选项有三种：Restart, Yoyo, Incremental
注：如果动画已经开始的话就没有效果了。此外，如果动画在序列中，无限循环也不会被应用。
设置loops到-1会使吐温循环无限。
LoopType.Restart: 当循环结束时，它将从一开始就重新启动。
LoopType.Yoyo: 当一个循环结束时，它将向后播放，直到它完成另一个循环，然后再向前，然后再向后，等等。
LoopType.Incremental: 每次循环结束时，其endValue和startValue之间的差异将被添加到endValue中，从而创建随每个循环而增加其值的Tweens。此循环类型仅适用于Tweeners.
- SetRecyclable(bool recyclable)
作用：设置是否可以被回收，设置为true，可以在被销毁后再循环，否则就会被销毁
- SetRelative(bool isRelative = true)
作用：设置相对值endValue，将变成计算statValue+endValue而不是直接使用endValue
- SetUpdate(UpdateType updateType, bool isIndependentUpdate = false)
作用：设置更新的类型(Normal, Late或Fixed)
UpdateType.Normal:在更新调用期间更新每个帧。
UpdateType.Late:在LateUpdate调用期间更新每个帧。
UpdateType.Fixed:使用FixedUpdate调用进行更新。
UpdateType.Manual:通过手动通知DOTween.ManualUpdate更新。

**链式回调**

- OnComplete(TweenCallback callback)
//设置一个回调，该回调将在动画完成时触发，所有循环都包括在内
transform.DOMoveX(4, 1).OnComplete(MyCallback);
- OnKill(TweenCallback callback)
//设置一个回调，该回调将在动画被销毁的时触发
transform.DOMoveX(4, 1).OnKill(MyCallback);
- OnPlay(TweenCallback callback)
//设置一个回调，该回调将在开始播放的动画的时候触发，也会在每次动画从暂停到恢复播放的时候调用
transform.DOMoveX(4, 1).OnPlay(MyCallback);
- OnPause(TweenCallback callback)
//设置一个回调，该回调将在暂停播放的时候触发
transform.DOMoveX(4, 1).OnPause(MyCallback);
- OnRewind(TweenCallback callback)
//设置一个回调，该回调将在动画倒回来的时候触发
transform.DOMoveX(4, 1).OnRewind(MyCallback);
- OnStart(TweenCallback callback)
//设置一个回调，该回调将在动画启动的时候触发
transform.DOMoveX(4, 1).OnStart(MyCallback);
- OnStepComplete(TweenCallback callback)
//设置一个回调，该回调将在每次动画完成一个循环时都会触发该回调
transform.DOMoveX(4, 1).OnStepComplete(MyCallback);
- OnUpdate(TweenCallback callback)
//设置一个回调，每次动画有更新的时候触发这个回调
transform.DOMoveX(4, 1).OnUpdate(MyCallback);
- OnWaypointChange(TweenCallback<int> callback)
//设置一个回调，该回调将在当前路径之间的路径更改时触发。这是一个特殊的回调，与其他回调相反，需要接受一个类型的参数。int(这将是新更改的路点索引)。
void Start() {
﻿﻿﻿﻿﻿﻿﻿   	transform.DOPath(waypoints, 1).OnWaypointChange(MyCallback);
﻿﻿﻿﻿﻿﻿﻿}
﻿﻿﻿﻿﻿﻿﻿void MyCallback(int waypointIndex)﻿﻿﻿﻿﻿﻿﻿ {
﻿﻿﻿﻿﻿﻿﻿ 	  Debug.Log("Waypoint index changed to " + waypointIndex);
﻿﻿﻿﻿﻿﻿﻿}

注意：如果想使用嵌套回调，是仍然有效的，但是要按照正确的顺序。

**如果想使用带参数的回调，可以使用Lambdas表达式：**
```csharp
// 回调函数没有参数
﻿﻿﻿﻿﻿﻿transform.DOMoveX(4, 1).OnComplete(MyCallback);
﻿﻿﻿﻿﻿﻿// 回调函数带参数
﻿﻿﻿﻿﻿﻿transform.DOMoveX(4, 1).OnComplete(()=>MyCallback(someParam, someOtherParam));
```
## 十、从Tweens获取数据
### 10.1 静态方法
- static List<Tween> PausedTweens()
返回处于暂停状态的所有活动动画的列表，如果没有活动暂停的动画，则返回NULL。
小心：调用此方法将创建垃圾分配，因为每次调用都会生成一个新列表。
- static List<Tween> PlayingTweens()
返回处于播放状态的所有活动动画的列表，如果没有活动的播放消息，则返回NULL。
注意：调用此方法将创建垃圾分配，因为每次调用都会生成一个新列表。
- static List<Tween> TweensById(object id, bool playingOnly = false)
返回一个具有给定id的所有活动Tweens的列表，如果没有带有给定id的活动Tweens，则返回NULL。
小心：调用此方法将创建垃圾分配，因为每次调用都会生成一个新列表。
playingOnly：如果true只返回当前正在播放的给定ID的Tweens，否则全部返回。
- static List<Tween> TweensByTarget(object target, bool playingOnly = false)
返回具有给定目标的所有活动Tweens的列表，如果给定目标没有活动Tweens，则返回NULL。
注：在使用快捷方式时，会自动设置Tweens的目标，而不是使用一般的方式。
注：DOTween动画视觉编辑器将其游戏对象指定为目标(而不是转换、材料或其他快捷方式的实际目标)，因此，如果您想要获取视觉创建的Tweens，请使用它。
小心：调用此方法将创建垃圾分配，因为每次调用都会生成一个新列表。
playingOnly：如果true只返回当前正在播放的给定ID的Tweens，否则全部返回。
- static bool IsTweening(object idOrTarget, bool alsoCheckIfPlaying = false)
如果具有给定ID或目标的动画是活动的，则返回true。您还可以使用它来了解目标上是否存在快捷键。
alsoCheckIfPlaying：如果false(默认)返回true，则只要给定目标/ID的动画是活动的，否则也要求它正在播放。
- static int TotalPlayingTweens()
返回活动和播放的动画总数。
### 10.2 实例方法
- float fullPosition
获取并设置渐变的时间位置(包括循环，不包括延迟)。
- int CompletedLoops()
返回Tweens完成的循环总数
- float Delay()
返回Tweens的最终延时时间
- float Duration(bool includeLoops = true)
返回Tweens的持续时间(不包括延迟，如果包含循环，则为includeLoops是真的)。
includeLoops为true时，返回完整持续时间循环时间，不然返回单个循环周期的持续时间
- float Elapsed(bool includeLoops = true)
返回Tweens的当前运行时间（不包括延迟，如果包含循环，则设置includeLoops为true）
- float ElapsedDirectionalPercentage()
基于单个循环返回该补间的运行百分比(0到1)(延迟除外)，并将最终的反向Yoyo循环计算为1到0，而不是0到1。
- float ElapsedPercentage(bool includeLoops = true)
返回此之间的运行百分比(0到1)(不包括延迟，如果包含循环，则为includeLoops是真的)。
- bool IsActive()
返回Tweens是否被启动
- bool IsBackwards()
返回Tweens是否正在倒带
- bool IsComplete()
返回Tweens是否完成，true是完成，如果Tweens被销毁在返回flase
- bool IsInitialized()
如果已经初始化Tweens，则返回true
- bool IsPlaying()
如果Tweens正在播放，则返回true
- int Loops()
返回分配给Tweens的循环总数
### 10.3 实例方法 --- Path tweens
- Vector3 PathGetPoint(float pathPercentage)
根据给定的路径百分比返回路径上的一个点
路径在Tweens启动后被初始化，或者如果使用路径编辑器(DOTweenPro功能)创建Tweens，则立即初始化路径。
您可以通过调用Init来初始化路径
pathPercentage：到达点的路径的百分比(0到1)。
示例：Vector3 myPathMidPoint = myTween.PathGetPoint(0.5f);
- Vector3[] PathGetDrawPoints(int subdivisionsXSegment = 10)
返回可用于绘制路径的点数组(返回NULL如果这不是一个路径之间，如果Tweens是无效的，或者如果路径尚未初始化)。
注：此方法生成分配，因为它创建了一个新数组。
示例：Vector3[] myPathDrawPoints = myTween.PathGetDrawPoints();
- float PathLength()
返回路径的长度(如果这不是路径之间的路径，则返回-1，如果路径无效，或者路径尚未初始化)。
示例：float myPathLength = myTween.PathLength();
## 十一、协同方法
Tweens提供了一套很有用的协同程序，可以将其设置在协同器中，这样您就可以等待一些事情发生
这些方法都有一个可选的bool参数，该参数是设置是否允许返回：

- WaitForCompletion
创建一个协同指令，当Tweens完成的时候才执行后面的代码
IEnumerator SomeCoroutine()
﻿﻿﻿﻿﻿﻿﻿{
﻿﻿﻿﻿﻿﻿﻿  Tween myTween = transform.DOMoveX(45, 1);
﻿﻿﻿﻿﻿﻿﻿  yield return myTween.WaitForCompletion();
﻿﻿﻿﻿﻿﻿﻿  Debug.Log("Tween completed!");
﻿﻿﻿﻿﻿﻿﻿}
﻿﻿﻿﻿﻿﻿
- WaitForElapseLoops
创建一个协同指令，当Tweens被销毁或者循环一定的次数的时候才执行后面的代码
IEnumerator SomeCoroutine()
﻿﻿﻿﻿﻿﻿﻿{
﻿﻿﻿﻿﻿﻿﻿  Tween myTween = transform.DOMoveX(45, 1).SetLoops(4);
﻿﻿﻿﻿﻿﻿﻿  yield return myTween.WaitForElapsedLoops(2);
﻿﻿﻿﻿﻿﻿﻿  Debug.Log("Tween has looped twice!");
﻿﻿﻿﻿﻿﻿﻿}
- WaitForKill
创建一个协同指令，当Tweens被销毁的时候才执行后面的代码
IEnumerator SomeCoroutine()
﻿﻿﻿﻿﻿﻿﻿{
﻿﻿﻿﻿﻿﻿﻿  Tween myTween = transform.DOMoveX(45, 1);
﻿﻿﻿﻿﻿﻿﻿  yield return myTween.WaitForKill();
﻿﻿﻿﻿﻿﻿﻿  Debug.Log("Tween killed!");
﻿﻿﻿﻿﻿﻿﻿}
- WaitForPosition
创建一个协同指令，当Tweens被销毁的时候或者到达指定位置才执行后面的代码
IEnumerator SomeCoroutine()
﻿﻿﻿﻿﻿﻿﻿{
﻿﻿﻿﻿﻿﻿﻿  Tween myTween = transform.DOMoveX(45, 1);
﻿﻿﻿﻿﻿﻿﻿  yield return myTween.WaitForPosition(0.3f);
﻿﻿﻿﻿﻿﻿﻿  // This log will happen after the tween has played for 0.3 seconds
﻿﻿﻿﻿﻿﻿﻿  Debug.Log("Tween has played for 0.3 seconds!");
﻿﻿﻿﻿﻿﻿﻿}
- WaitForRewind
创建一个协同指令，当Tweens被销毁的时候或者倒带的时候才执行后面的代码
IEnumerator SomeCoroutine()
﻿﻿﻿﻿﻿﻿﻿{
﻿﻿﻿﻿﻿﻿﻿  Tween myTween = transform.DOMoveX(45, 1).SetAutoKill(false).OnComplete(myTween.Rewind);
﻿﻿﻿﻿﻿﻿﻿  yield return myTween.WaitForRewind();
﻿﻿﻿﻿﻿﻿﻿  Debug.Log("Tween rewinded!");
﻿﻿﻿﻿﻿﻿﻿}
- WaitForStart
创建一个协同指令，当Tweens被销毁的时候或者启动的时候才执行后面的代码
IEnumerator SomeCoroutine()
﻿﻿﻿﻿﻿﻿﻿{
﻿﻿﻿﻿﻿﻿﻿  Tween myTween = transform.DOMoveX(45, 1);
﻿﻿﻿﻿﻿﻿﻿  yield return myTween.WaitForStart();
﻿﻿﻿﻿﻿﻿﻿  Debug.Log("Tween started!");
﻿﻿﻿﻿﻿﻿﻿}


## 十二、总结
基本将Tweens的API总结了一次，包括如何设置全局参数，使用Tweens，创建序列，设置回调等内容，以后有时间，再将API的内容作用说明以及效果展示一下。好啦，结束
