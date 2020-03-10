---
layout: post
category: Unity3D-Daily
title: 【Unity3D】UGUI原理分析
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
这篇文章是分析UGUI的各种原理，包括层级渲染，事件触发，布局等，教程也比较详细，转过来有空研究一下.

## 二、原文
原文地址：https://mp.weixin.qq.com/s/ZaYrH9QzxaS5nMg3Kn3Eig
原文作者：克森 
原文出处：微信公众号克森空间

## 三、正文


----------


## Unity UGUI 原理篇(一)：Canvas 渲染模式

### 目标
了解各种不同UI Render Mode

### 使用环境与版本
Window 7
Unity 5.2.5

### Render Mode

UI渲染的方式，有以下三种

- Screen Space – Overlay：萤幕空间 – 覆盖

- Screen Space – Camera：萤幕空间 – 摄影机

- World Space：世界座标空间

<font color="red">Screen Space - Overlay</font>
![这里写图片描述](https://img-blog.csdn.net/20180704115314916?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
在此模式下不会参照到Camera，UI直接显示在任何图形之上

- 1.Pixel Perfect：可以使图像更清晰，但是有额外的性能开销，如果在有大量UI动画时，动画可能会不平顺

- 2.Sort Order：深度值，该值越高显示越前面

<font color="red">Screen Space - Camera</font>
![这里写图片描述](https://img-blog.csdn.net/20180704115351818?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
使用一个Camera作为参照，将UI平面放置在Camera前的一定距离，因为是参照Camera，如果萤幕大小、分辨率、Camera视锥改变时UI平面会自动调整大小。如果Scene中的物件(GameObject)比UI平面更靠近摄影机，就会遮挡到UI平面。

- 1.Render Camera：用于渲染的摄影机

- 2.Plane Distance：与Camera的距离

- 3.Sorting Layer：Canvas属于的排序层，在 Edit->Project Setting->Tags and Layers->Sorting Layers 进行新增，越下方的层显示越前面

- 4.Order in Layer：Canvas属于的排序层下的顺序，该值越高显示越前面

<font color="red">World Space</font>
![这里写图片描述](https://img-blog.csdn.net/20180704115434487?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
把物体当作世界座标中的平面(GameObject)，也就是当作3D物件，显示3D UI

- 1.Event Camera：处理UI事件(Click、Drag)的Camera，所设定的Camera才能触发事件

<font color="red">参考资料</font>
Unity – Manual: Canvas

http://docs.unity3d.com/Manual/class-Canvas.html


----------
## Unity UGUI 原理篇(二)：Canvas Scaler 缩放核心

### 目标
- 1.了解各种不同 UI Scale Mode
- 2.Pixels Per Unit 每单位像素
- 3.Canvas Scale Factor 缩放因子
- 4.Reference Resolution（预设屏幕大小） 
- 5.Screen Size丶Canvas Size 之间的关系与算法

### 使用环境 与 版本
Window 7

Unity 5.2.4

### Canvas Scaler
Canvas Scaler是Unity UI系统中，控制UI元素的总体大小和像素密度的Compoent，Canvas Scaler的缩放比例影响著Canvas下的元素，包含字体大小和图像边界。

### Size
- Reference Resolution：预设萤幕大小

- Screen Size：目前萤幕大小
![这里写图片描述](https://img-blog.csdn.net/20180704115652359?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Canvas Size：Canvas Rect Transform 宽高
![这里写图片描述](https://img-blog.csdn.net/20180704115700864?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Scale Factor

http://docs.unity3d.com/ScriptReference/Canvas-scaleFactor.html

 用于缩放整个Canvas，而且调整Canvas Size与Screen Size一样



### 先来看一段官方代码

## CanvasScaler.cs  

```
protected void SetScaleFactor(float scaleFactor)
{
    if (scaleFactor == m_PrevScaleFactor)
        return;
 
    m_Canvas.scaleFactor = scaleFactor;
    m_PrevScaleFactor = scaleFactor;
}
```
程式码可以看出，Canvas Scaler 透过设定Canvas下的Scale Factor，缩放所有在此Canvas下的元素



当Scale Factor为1时，Screen Size (800*600)、Canvas Size(800*600)，图片大小1倍

![这里写图片描述](https://img-blog.csdn.net/2018070411583999?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704115845190?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
当Scale Factor为2时，Screen Size (800*600)、Canvas Size(400*300)，图片大小2倍
![这里写图片描述](https://img-blog.csdn.net/20180704115852841?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704115859800?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
在当Scale Factor为2时，Scale Factor 会调整整个Canvas 的大小，并让他的大小跟Screen Size一样，运算后Canvas Size放大2倍，刚好等于Screen Size，而底下的图片会放大2倍

### UI Scale Mode
<font color="red">Constant Pixel Size</font>
Canvas Size 始终等于 Screen Size，透过Scale Factor直接缩放所有UI元素
![这里写图片描述](https://img-blog.csdn.net/20180704115943985?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

- 1. Scale Factor：透过此Factor缩放所有在此Canvas下的元素

-  2. Reference Pixels Per Unit：

先介绍图片档设定中的Pixels Per Unit，意思是在这张Sprite中，世界座标中的一单位由几个Pixel组成
![这里写图片描述](https://img-blog.csdn.net/20180704142741357?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这边使用的测试图片为原始大小100*100 的图档，这边统称测试图
![这里写图片描述](https://img-blog.csdn.net/20180704142752902?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
举例来说，场景中有一个1*1 Cube ，与一个Sprite图片指定为测试图，两者的Transform Scale 都为 1
当 Pixels Per Unit=100，每单位由 100 Pixel组成，Sprite 是100*100 Pixels，那 Sprite 在世界座标中大小就会变成 100/100 * 100/100 = 1*1 Unit
![这里写图片描述](https://img-blog.csdn.net/20180704142825460?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
(左：Cube ，右：Sprite)

当 Pixels Per Unit=10，每单位由 10 Pixel组成，Sprite 是100*100 Pixels，那 Sprite 在世界座标中大小就会变成 100/10 * 100/10 = 10*10 Unit
![这里写图片描述](https://img-blog.csdn.net/20180704142857261?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
(左：Cube，右：Sprite)
<font color="red">结论：</font>
- Unity中一单位等于 100 Pixels
- 由此可以推导出公式：
> Sprite 在世界座标中大小 = 原图大小(Pixels) / Pixels Per Unit

让我们回到 Reference Pixels Per Unit，官方解释是，如果图片档有设定Pixels Per Unit，则会将Sprite 的 1 pixel 转换成 UI 中的 1 pixel

#### Image.cs

```
public float pixelsPerUnit
{
    get
    {
        float spritePixelsPerUnit = 100;
        if (sprite)
            spritePixelsPerUnit = sprite.pixelsPerUnit;
 
        float referencePixelsPerUnit = 100;
        if (canvas)
            referencePixelsPerUnit = canvas.referencePixelsPerUnit;
 
        return spritePixelsPerUnit / referencePixelsPerUnit;
    }
}
```
上面官方程式码，可以看出 Image 透过 spritePixelsPerUnit / referencePixelsPerUnit 方式算出新的 pixelsPerUnit

#### Image.cs

```
public override void SetNativeSize()
{
    if (overrideSprite != null)
    {
        float w = overrideSprite.rect.width / pixelsPerUnit;
        float h = overrideSprite.rect.height / pixelsPerUnit;
        rectTransform.anchorMax = rectTransform.anchorMin;
        rectTransform.sizeDelta = new Vector2(w, h);
        SetAllDirty();
    }
}
```
在设定 Image 图片大小时，是把 宽高 / pixelsPerUnit

实作一下，建立一个Canvas参数如下

![这里写图片描述](https://img-blog.csdn.net/20180704143043642?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Canvas底下建立一个Image，Sprite设定为测试图，参数如下
![这里写图片描述](https://img-blog.csdn.net/20180704143057948?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这边做4种不同的测试：测试方式是修改 Reference Pixels Per Unit 与 Pixels Per Unit 后，点下 Image Compoent 的 Set Native Size来设定图片原始大小，藉此看到图片变化.
![这里写图片描述](https://img-blog.csdn.net/20180704143116952?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
■ 上表可以看出当数值改变时，图片预设大小也会改变

■ 由此可以推导出公式
> UI大小 = 原图大小(Pixels)  /  (Pixels Per Unit / Reference Pixels Per Unit) 

<font color="red">Scale With Screen Size：</font>
透过设定的Reference Resolution(预设萤幕大小)来缩放
1. Reference Resolution：预设萤幕大小

2. Screen Match Mode：缩放模式


先来看官方的算法

#### CanvasScaler.cs

```
Vector2 screenSize = new Vector2(Screen.width, Screen.height);
 
float scaleFactor = 0;
switch (m_ScreenMatchMode)
{
    case ScreenMatchMode.MatchWidthOrHeight:
    {
        // We take the log of the relative width and height before taking the average.
        // Then we transform it back in the original space.
        // the reason to transform in and out of logarithmic space is to have better behavior.
        // If one axis has twice resolution and the other has half, it should even out if widthOrHeight value is at 0.5.
        // In normal space the average would be (0.5 + 2) / 2 = 1.25
        // In logarithmic space the average is (-1 + 1) / 2 = 0
        float logWidth = Mathf.Log(screenSize.x / m_ReferenceResolution.x, kLogBase);
        float logHeight = Mathf.Log(screenSize.y / m_ReferenceResolution.y, kLogBase);
        float logWeightedAverage = Mathf.Lerp(logWidth, logHeight, m_MatchWidthOrHeight);
        scaleFactor = Mathf.Pow(kLogBase, logWeightedAverage);
        break;
    }
    case ScreenMatchMode.Expand:
    {
        scaleFactor = Mathf.Min(screenSize.x / m_ReferenceResolution.x, screenSize.y / m_ReferenceResolution.y);
        break;
    }
    case ScreenMatchMode.Shrink:
    {
        scaleFactor = Mathf.Max(screenSize.x / m_ReferenceResolution.x, screenSize.y / m_ReferenceResolution.y);
        break;
    }
}
```
a. Expand(扩大)：将Canvas Size进行宽或高扩大，让他高于Reference Resolution，计算如下

>scaleFactor = Mathf.Min(screenSize.x / m_ReferenceResolution.x, screenSize.y / m_ReferenceResolution.y);


意思是分别算出长宽 ，”Screen Size” 佔了 “Reference Resolution” 的比例，在求小的

举例来说，Reference Resolution为1280*720，Screen Size为800*600

ScaleFactor Width： 800/1280=0.625

ScaleFactor Height：600/720=0.83333


套用ScaleFactor公式：Canvas Size = Screen Size / Scale Factor

Canvas Width：800 / 0.625 = 1280

Canvas Height：600 / 0.625 = 960

Canvas Size 为 1280*960，高度从720变成了960，最大程度的放大(显示所有元素)

![这里写图片描述](https://img-blog.csdn.net/20180704143325145?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
b. Shrink(收缩)：将Canvas Size进行宽或高收缩，让他低于Reference Resolution，计算如下

>scaleFactor = Mathf.Max(screenSize.x / m_ReferenceResolution.x, screenSize.y / m_ReferenceResolution.y);

意思是分别算出长宽 ，”Screen Size” 佔了 “Reference Resolution” 的比例，在求大的

举例来说，Reference Resolution为1280*720，Screen Size为800*600

ScaleFactor Width： 800/1280=0.625

ScaleFactor Height：600/720=0.83333


套用ScaleFactor公式：Canvas Size = Screen Size / Scale Factor

Canvas Width：800 / 0.83333 = 960

Canvas Height：600 / 0.83333 = 720

Canvas Size 为 960*720，宽度从1280变成了960，最大程度的缩小
![这里写图片描述](https://img-blog.csdn.net/20180704143351341?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

c. Match Width or Height：根据Width或Height进行混合缩放，计算如下

```
float logWidth = Mathf.Log(screenSize.x / m_ReferenceResolution.x, kLogBase);
float logHeight = Mathf.Log(screenSize.y / m_ReferenceResolution.y, kLogBase);
float logWeightedAverage = Mathf.Lerp(logWidth, logHeight, m_MatchWidthOrHeight);
scaleFactor = Mathf.Pow(kLogBase, logWeightedAverage);
```
分别对ScaleFactor Width、Height取对数后，再进行平均混合，那为什麽不直接使用March对Width、Height进行混合呢??，让我们来比较一下

假设Reference Resolution为400*300，Screen Size为200*600 大小关系是

Reference Resolution Width 是 Screen Size Width的2倍

Reference Resolution Height 是 Screen Size 的0.5倍

看起来会像下图
![这里写图片描述](https://img-blog.csdn.net/20180704143418965?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
当March为0.5时，ScaleFactor应该要是 1 (拉平)

ScaleFactor Width： 200/400=0.5

ScaleFactor Height：600/300=2

一般混合：

ScaleFactor = March * ScaleFactor Width + March * ScaleFactorHeight

ScaleFactor = 0.5 * 0.5 + 0.5 * 2 = 1.25

对数混合：

logWidth：log2(0.5) = -1

logHeight：log2(2) = 1

logWeightedAverage：0

ScaleFactor：20 = 1

scaleFactor一般混合为1.25，对数混合为1，结果很明显，使用对数混合能更完美的修正大小


<font color="red">Constant Physical Size</font>

透过硬体设备的Dpi(Dots Per Inch 每英吋点数)，进行缩放
![这里写图片描述](https://img-blog.csdn.net/20180704143452587?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
1. Physical Unit：使用的单位种类

![这里写图片描述](https://img-blog.csdn.net/20180704143515770?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70) 

2.Fallback Screen DPI：备用Dpi，当找不到设备Dpi时，使用此值

3.Default Sprite DPI：预设的图片Dpi

```
float currentDpi = Screen.dpi;
float dpi = (currentDpi == 0 ? m_FallbackScreenDPI : currentDpi);
float targetDPI = 1;
switch (m_PhysicalUnit)
{
    case Unit.Centimeters: targetDPI = 2.54f; break;
    case Unit.Millimeters: targetDPI = 25.4f; break;
    case Unit.Inches:      targetDPI =     1; break;
    case Unit.Points:      targetDPI =    72; break;
    case Unit.Picas:       targetDPI =     6; break;
}
 
SetScaleFactor(dpi / targetDPI);
SetReferencePixelsPerUnit(m_ReferencePixelsPerUnit * targetDPI / m_DefaultSpriteDPI);
```
<font color="red">结论</font>

■ ScaleFactor 为 “目前硬体dpi” 佔了 “目标单位” 的比例

■ ReferencePixelsPerUnit 要与目前的Dpi在运算求出新的值，再传入Canvas中求出大小，公式如下：

新的 Reference Pixels Per Unit = Reference Pixels Per Unit * Physical Unit / Default Sprite DPI

UI大小 = 原图大小(Pixels)  /  (Pixels Per Unit / 新的 Reference Pixels Per Unit)
<font color="red">参考资料</font>
■ Unity – Manual: Canvas

http://docs.unity3d.com/Manual/class-Canvas.html


----------
### Unity UGUI 原理篇(三)：RectTransform

#### 目标
1.理解 RectTransform Component
2.Anchor（锚点）
3.Pivot（支点）
4.Blue Print Mode 与 Raw Edit Mode
#### 使用环境 与 版本
Window 7

Unity 5.2.4
#### RectTransform
![这里写图片描述](https://img-blog.csdn.net/20180704143704807?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
RectTransform 是 Transform 的 2D 对应 Component，Transform 表示单个点，RectTransform 表示一个2D矩形(UI空间)，如果父子物体都有RectTransform，那麽子物体可以指定在父物体矩形中的位置与大小，简单来说RectTransform 就是定义UI元素的位置、旋转、大小
#### Anchor (锚点)
![这里写图片描述](https://img-blog.csdn.net/20180704143728932?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
物体的锚点(对齐点)，如果父子都有 RectTransform 情况下，子物体可以依据 Anchor 对齐到父物体，又分为 Min 与 Max 位置，如下图物体四周有4个三角形
![这里写图片描述](https://img-blog.csdn.net/2018070414373644?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/2018070414375420?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

<font color="red">Anchor位置座标与关系</font>
当我们使用滑鼠点选4个三角形调整Anchor时，会贴心的出现比例讯息，此比例是子物体在父物体中的缩放比例
![这里写图片描述](https://img-blog.csdn.net/20180704143842878?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/201807041438529?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
当Canvas 下有1张图 Anchor Min 与 Anchor Max 皆为 (0.5 , 0.5)，如下左图部分

如果将Anchor Min调整为(0.3, 0.5) ，Anchor Max调整为 (0.5, 0.7)，如下右图部分

注意看 左图 Pos X、Pos Y、Width、Height ，会改变为 右图 Left、Top、Right、Buttom
![这里写图片描述](https://img-blog.csdn.net/20180704143900205?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
因为当 Anchor 在同一点时，显示的是物体的座标与大小，当 Anchor 不在同一点时(此时会形成矩形)，显示的会是 Anchor 矩形填充空间，如下图，(P.S.在我们移动物体时会贴心的显示目前与 Anchor 距离关系)
![这里写图片描述](https://img-blog.csdn.net/20180704143934187?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
<font color="red">上面看完一定还是不了解怎麽运作，让我们来透过实例了解一下</font>
Canvas 下有5张图，Anchor Min 与 Anchor Max 皆为 (0.5 , 0.5)，物体的位置会对齐到父物体的中心，当父物体大小改变时，情形如下
![这里写图片描述](https://img-blog.csdn.net/20180704144011418?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Canvas 下有1张图，Anchor Min 与 Anchor Max 皆为 (0.0, 1.0)，物体的位置会对齐到父物体的左上角，当父物体大小改变时，情形如下，物体会固定在左上角
![这里写图片描述](https://img-blog.csdn.net/20180704144027556?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Canvas 下有1张图，Anchor Min 为 (0.0, 0.0)， Anchor Max 为 (1.0, 0.0)，物体的位置会对齐到父物体的左下角与右下角，当父物体大小改变时，情形如下，物体宽度会随著父物体改变
![这里写图片描述](https://img-blog.csdn.net/20180704144050369?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
由上面的几个实例可以知道，子物体会依据所设定 Anchor 对齐到父物体，当父物体大小改变时，透过 Anchor 更新子物体，上面有提到当我们点选4个三角形调整Anchor时，画面会贴心的出现比例讯息，相信有经验的人一定知道该比例的用意，此比例就是子物体在父物体中的缩放比例，以下举例
<font color="red">原来数值</font>
Parent Size (400, 350)

Image Size (120, 105)

Anchor Min 为 (0.2, 0.5)， Anchor Max 为 (0.5, 0.8)
![这里写图片描述](https://img-blog.csdn.net/20180704144127613?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
<font color="red">Parent Size 一半时数值</font>
Parent Size (200, 175)

Image Size (60, 52.5)
Image Size Width  =<font color="red">400 * 50% * 30% = 60</font>
Image Size Height =<font color="red">350 * 50% * 30% = 52.5</font>
Anchor Min 为 (0.2, 0.5)， Anchor Max 为 (0.5, 0.8)

经由上面可以得知父物体在缩小2倍后，父物体透过子物体的 Anchor 比例更新子物体，透过这种方式，我们可以达到不同萤幕解析度自动改变UI大小与位置
<font color="red">Anchor Presets</font>
![这里写图片描述](https://img-blog.csdn.net/20180704144251224?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
点选 RectTransform 左上角，可以开启Anchor Presets 工具，这边列出了常用的 Anchor ，可以快速套用，按住Shift 可以连同 Pivot 一起改变，按住 Alt 可以连同位置一起改变
<font color="red">Pivot (支点)</font>
物体自身的支点，影响物体的旋转、缩放、位置，改变 UI Pivot 必须先开启控制面板的 Pivot 按钮，如下图
![这里写图片描述](https://img-blog.csdn.net/20180704144320728?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

<font color="red">当 Pivot (0.5, 0.5)</font>
![这里写图片描述](https://img-blog.csdn.net/20180704144355198?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
<font color="red">当 Pivot (0, 1)</font>
![这里写图片描述](https://img-blog.csdn.net/20180704144415172?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

<font color="red">Blue Print Mode(蓝图模式) 、 Raw Edit Mode(原始编辑模式)</font>
![这里写图片描述](https://img-blog.csdn.net/20180704144432228?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



<font color="red">Blue Print Mode (蓝图模式)</font>
忽略了物体的 Local Rotation 和 Local Scale，方便以原来的旋转与大小调整物体
![这里写图片描述](https://img-blog.csdn.net/20180704144512252?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704144518903?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
<font color="red">Raw Edit Mode (原始编辑模式)</font>
在 Inspector 中调整 Pivot 和 Anchor 时，物体会维持目前的位置与大小(Inspector 中数值部分)，调整情形如下，请注意数值部分

Inspector 中调整  Pivot
![这里写图片描述](https://img-blog.csdn.net/20180704144542918?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Inspector 中调整  Anchor
![这里写图片描述](https://img-blog.csdn.net/20180704144551571?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
<font color="red">参考资料</font>
■ Unity – Manual: Basic Layout

http://docs.unity3d.com/Manual/UIBasicLayout.html

■ UnityのuGUIのレイアウト调整机能について解说してみる（RectTransform入门）

http://tsubakit1.hateblo.jp/entry/2014/12/19/033946


----------
## Unity UGUI 原理篇（四）: Event System Manager 事件与触发
### 目标
1.Event System 事件系统
2.Input Module 输入控制
3.Graphic Raycaster
4.Physics Raycaster 与 Physics 2D Raycaster
### 使用环境 与 版本
Window 7

Unity 5.2.4
### Event System
在建立出UI时，Unity会自动帮我们建立Event System物件，此物件是基于滑鼠、触摸、键盘的输入方式，传送 Event 到 Object 上，物件下有3个组件，分别为<font color="red">.Event System Manager、Standalone Input Module、Touch Input Module</font>
![这里写图片描述](https://img-blog.csdn.net/20180704144954248?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
<font color="red">1.Event System Manager</font>
![这里写图片描述](https://img-blog.csdn.net/20180704145026380?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
控管所有Event，负责将滑鼠、触摸、键盘输入方式(Input Module) 与 被选中的 Object 互相协调，每个 “Update” Event System 都会接收所有呼叫，并判断出这一瞬间要使用哪种Input Modules
<font color="red">Event System Info</font>
当按下Play后，点选Event System物件，会在inspector显示 选中物件、位置、接收事件的Camera等等资讯
.![这里写图片描述](https://img-blog.csdn.net/20180704145059478?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
First Selected

执行时第一次要选择的Object，例如：选择为 InputField (输入框) 后 ，按下Play后就会将游标 Force 在 InputField 上


Send Navigation Events

是否开启UI导航功能，导航功能是可以用键盘的 “上”、”下”、”左”、”右”、”Cancel(Esc)”、”Sumit(Enter)” 控制选择的UI

举例：如果画面上有多个选单按钮，我们可以设定按钮上的 Navigation Options 这裡使用Explicit方式，来指定按下键盘 “上”、”下”、”左”、”右” 要选取哪一个物件

Select On Up ：当键盘按下 “上” 键后要选择的物件，Down、Left、Right 不多做赘述
![这里写图片描述](https://img-blog.csdn.net/20180704145120443?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Visualize Buttin： 按下Visualize可以看到物件指向的黄线
![这里写图片描述](https://img-blog.csdn.net/2018070414513016?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Drag Threshold

Drag Event灵敏度，越低越灵敏


2.Standalone Input Module
![这里写图片描述](https://img-blog.csdn.net/20180704145155688?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
电脑输入控制模组，主要影响著滑鼠与键盘的输入，使用 Scene 中的 Raycasters 计算哪个元素被点中，并传递 Event

Horizontal Axis

代表 Input Module 中的 Horizontal Axis，可以被设定为 Input Manager 中的值，Vertical Axis、Submit Button、Cancel Button 不多做赘述


Input Actions Per Second

每秒能输入的最大按钮与滑鼠次数



Repeat Delay

重複输入的延迟



事件执行完整流程

键盘输入

1.Move Event：透过 input manager 验证输入 axis、left、right、up、down 按钮，传递给 selected object

2.Submit 、Cancel Button：物件已经 Preesed (按下)时，透过 input manager 验证输入  submit 、cancel 按钮，传递给 selected object

滑鼠输入

1.如果是新的按下

a.传送 PointerEnter event

b.传送 PointerPress event

c.将 drag 相关暂存

e.传送 BeginDrag event

f.设定Event system中的 selected object 为按下的Object

2.如果是持续按下(Drag)

a.处理移动相关

b.传送 Drag event

c.处理 Drag 时跨到其他物体的 PointerEnter event、PointerExit event

3.如果是释放(滑鼠放开)

a.传送 PointerUp event

b.如果滑鼠放开与按下时的物件一样，传送 PointerClick event

c.如果有 drag 相关暂存，传送 Drop event

d.传送EndDrag event

4.滑鼠中键滚轮传送scroll event



3.Touch Input Module




![这里写图片描述](https://img-blog.csdn.net/20180704145205591?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
触摸输入模组，主要用于移动设备上，可以透过Touch、Drag的方式响应，使用 Scene 中的 Raycasters 计算哪个元素被点中，并传递 Event

事件执行完整流程

与Standalone Input Module 的滑鼠输入一样，滑鼠点下想成触摸即可


4.Event System 触发流程

1.使用者输入(滑鼠、触摸、键盘)

2.透过 Event System Manager 决定使用 Standalone 还是 Touch Input Module

3.决定使用的 Input Module 后，透过 Scene 中的 Raycasters 计算哪个元素被点中

4.传送Event


<font color="red">Graphic Raycaster (图形 射线检测员)</font>

组件位置：Unity Menu Item → Component → Event → Graphic Raycaster
![这里写图片描述](https://img-blog.csdn.net/20180704145252215?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
建立 Canvas 物件时下的其中一个 Component，Raycaster 会观察 Canvas下所有图形，并检测是否被击中，射线检测其实就是指定位置与方向后，投射一条隐形线并判断是否有碰撞体在线上，射线检测这点官方已经有详细说明，这裡用于判断是否点选到UI图形

<font color="red">Ignore Reversed Graphics：</font>

背对著画面的图形，射线检测是否要忽略此图形

举例：当图形Y轴进行旋转180后，此时是背对著画面，这时是如果有打勾，就会忽略不检测此图形



Blocked Objects 、 Blocking Mask：

主要用于当Canvas Component Render Mode 使用 World Space 或是 Camera Space 时，UI 前有 3D 或是 2D Object 时，将会阻碍射线传递到 UI 图形

Blocked Objects 阻碍射线的 Object 类型

Blocking Mask 勾选的 Layer 将会阻碍射线

举例：如果画面上有一个 Button 与 Cube 位置故意重叠，现在点击重叠之处会发现 Button 还是会被触发
![这里写图片描述](https://img-blog.csdn.net/20180704145329616?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
如果将Cube 的 Layer 改为 Test01 ，Blocked Objects 设定为 Three D，Blocking Mask 只勾选 Test01，再次点选重叠区域，会发现 Cube 会阻碍射线检测，此时按钮会接收不到射线，当然也不会有反应
![这里写图片描述](https://img-blog.csdn.net/20180704145348281?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

<font color="red">Physics Raycaster (物理物件 射线检测员)</font>
组件位置：Unity Menu Item → Component → Event → Physics Raycaster

透过 Camera 检测 Scene 中的 3D GameObject(必须有 Collider Component)，有实现 Event Interfaces 接口的物件将会接收到 Message 通知，例如能让 3D GameObject 能接收 点下Event 或是 拖拉Event 等等…..，看更多 Event 请点我

接下来让我们透过实例理解

1.建立 EventSystem，进行 Event 处理

物件位置：Unity Menu Item → GameObject → UI → EventSystem

2.Camera下增加 Physics Raycaster Component，用来观察射线
![这里写图片描述](https://img-blog.csdn.net/20180704145418288?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
3.实现 Event Interfaces 接口，这裡有两种方式，一种是建立 Script 直接实作 Interfaces ，一种是使用Event Trigger Component

<font color="red">第一种 建立 Script 直接实作 Interfaces</font>

a.建立一个 Script，实作 Event Interfaces


### EventTest.cs

```csharp
using UnityEngine;
using UnityEngine.EventSystems;
 
public class EventTest : MonoBehaviour, IPointerDownHandler
{
    public void OnPointerDown(PointerEventData eventData)
    {
        print(gameObject.name);
    }
}
```
Line. 2：using  UnityEngine.EventSystems 汇入命名空间

Line. 4：继承 Event Interfaces，这裡是IPointerDownHandler(点下事件)，看更多 Event 请点我

Line. 6~8：实作方法，传入 PointerEventData 为事件资料

b.建立一个3D物件(此称为Cube)，并增加 BoxCollider Component

![这里写图片描述](https://img-blog.csdn.net/20180704145531819?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
c.将 Script 放至 Cube 下，Inspector 中会出现 Intercepted Events 资讯，显示出正在监听的 Event
![这里写图片描述](https://img-blog.csdn.net/20180704145538912?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
d.此时点击 Cube 就会通知 OnPointerDown 方法并传入事件资讯

<font color="red">第二种 使用Event Trigger Component 实作 Interfaces </font>

a.建立一个 Script，实作方法，用于接收 Event Trigger 通知


#### EventTriggerTest.cs

```csharp
using UnityEngine;
using UnityEngine.EventSystems;
 
public class EventTriggerTest : MonoBehaviour
{
    //BaseEventData 動態傳入事件資訊
    public void OnPointerDown(BaseEventData eventData)
    {
        print("OnPointerDown--BaseEventData");
    }
 
    //純呼叫
    public void OnPointerDown()
    {
        print("OnPointerDown--non");
    }
 
    //傳入int
    public void OnPointerDown(int i)
    {
        print("OnPointerDown--int");
    }
}
```
Line. 2：using  UnityEngine.EventSystems 汇入命名空间

Line. 6~8：实作方法，这边实作3种

b.建立一个3D物件(此称为Cube)，并增加 BoxCollider Component
![这里写图片描述](https://img-blog.csdn.net/20180704145630401?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
c.将 Script 放至 Cube 下

d.Cube 下加入 Event Trigger Component，主要接收来至 Event System 的 Event ，并呼叫有实作的 Event

组件位置：Unity Menu Item → Component → Event → Event Trigger

e.点选 Add New Event Type 选择要实作的 Event 类型 ，这裡使用PointerDown(点下事件)举例
![这里写图片描述](https://img-blog.csdn.net/20180704145639125?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
f.此时会新增一个UnityEvents，是一种透过编辑器设定的方式，设定 Event 触发时要通知的方法与属性，详细可以参考以下，这边简单说明

<font color="red">胡乱说‧随便写 – Unity：使用 UnityEngine.Events 让程式更灵活、稳定</font>

Unity – Manual: UnityEvents

点下 “+” 按钮后，拖入要通知的Scene GameObject，Unity Event 就会寻找此 GameObject 上所有 Public 的方法与属性 ，就可以新增 Event 触发时 “通知的方法” 与 “预修改属性”

g.GameObject 拖入 Cube，通知方法设定 Script 中的3个方法
![这里写图片描述](https://img-blog.csdn.net/20180704145720983?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
h.此时点击 Cube 就会触发 PointerDown ，通知 Script 中的3个方法


<font color="red">4.实作注意点：</font>

■ Scene 必需有 EventSystem GameObject

■ Camera 必需有 Physics Raycaster Component

■ 3D GameObject 必须有 Collider Component

■ 实作 Event Interfaces 的方式，一种是建立 Script 直接实作 Interfaces ，一种是使用 Event Trigger Component，由上面实作可以知道，使用 Event Trigger 的方式可以使用编辑器设定，设定触发时的 “通知方法” 与 “修改属性”，且更为弹性


<font color="red">Physics 2D Raycaster</font>
组件位置：Unity Menu Item → Component → Event → Physics 2D Raycaster

跟 Physics Raycaster 只差在于，Physics 2D Raycaster 是检测 Scene 中的 2D GameObject，当然 GameObject 上必须有 Collider2D Component，这边不再赘述

<font color="red">后记</font>
我们透过输入的方式不同与 Raycaster 的关系，理解了整个 Event System 触发流程，而且也知道怎麽实作 Event 与应用 Event，不管是3D、2D、UI物件都可以方便的套用，大大提升开发速度、简化语法，可说是非常方便的功能


<font color="red">参考资料</font>
■ Unity – Manual: Event System

http://docs.unity3d.com/Manual/EventSystem.html

■ Unity – Manual: UnityEvents

http://docs.unity3d.com/Manual/UnityEvents.html

■ Unity – Raycasting

http://unity3d.com/cn/learn/tutorials/modules/beginner/physics/raycasting

■ 胡乱说‧随便写 – Unity：使用 UnityEngine.Events 让程式更灵活、稳定

http://godstamps.blogspot.tw/2015/10/unity-unityengineevents.html


----------
## Unity UGUI 原理篇（五）：Auto Layout 自动布局
### 目标
1.Auto Layout System 架构
2.Layout Element 元素大小
3.Horizontal丶Vertical丶Grid Layout Group 元素排列
4.Content Size 与 Aspect Ratio Fitter 大小控制
### 使用环境 与 版本
Window 7

Unity 5.2.4
### Auto Layout System
Auto Layout System 是基于 Rect Transform Layout System 之上的系统，自动调整一个或多个的元素大小、位置、间格，又分为 Layout Controllers(父物件) 与 Layout Elements(子物件) 两部分，一个简单的 Auto Layout 架构如下 (此处介绍理论，实作留到后面)
![这里写图片描述](https://img-blog.csdn.net/2018070414590676?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
<font color="red">Layout Element (子物件)</font>
![这里写图片描述](https://img-blog.csdn.net/20180704145941224?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704145947859?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
点选UI后，可以在 Inspector 最下方切换为 Layout Properties 看到资讯
![这里写图片描述](https://img-blog.csdn.net/20180704145955430?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704150001939?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Layout Controllers 透过不同的佈局方式，取得 Layout Element size 分配子物件，基本原则如下

首先分配 Minimum Size

如果还有足够空间，分配 Preferred Size

如果还有额外空间，分配 Flexible Size


从以下图片可以看出图片宽度的增长方式 (此处介绍理论，实作留到后面)
![这里写图片描述](https://img-blog.csdn.net/20180704150020925?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/2018070415003044?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
首先分配 Minimum Size (300，红色部分)

如果还有足够空间，分配 Preferred Size (300~500，绿色部分)

如果还有额外空间，分配 Flexible Size：1 (500~700，蓝色部分)



比较特别的是 Flexible，他是代表著整个大小的比例，如果 Layout 下有2个物体，分别给 Flexible 设定为 0.3 与 0.7，那比例就会变成下图 (3:7)
![这里写图片描述](https://img-blog.csdn.net/20180704150040436?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
另外要注意的是，Text、Image Component 会根据内容大小自动分配 Preferred Size

<font color="red">Layout Controllers (父物件)
Layout Group</font>

不会控制 Layout Controllers (父物件)自身大小，而是控制子物件大小与位置，在大多数情况下，根据每个元素的 minimum、preferred、flexible 大小分配适当的空间，layout group 之间也可以嵌套，又分为 Horizontal(水平)、Vertical(垂直)、Grid(格状) 3种

<font color="red">Horizontal Layout Group</font>
![这里写图片描述](https://img-blog.csdn.net/20180704150052175?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
水平方向(Width) 排列子物件

组件位置：Unity Menu Item → Component → Layout → Horizontal Layout Group


Padding：填充内部空间

Spacing：每个元素间格

Child Alignment：当没有填满全部空间时，子物件对齐位置

Child Force Expand：强制控制子物件填满空间



透过实例理解各参数：

A.开新 Scene
Unity Menu Item → File → New Scene

B.新增一个 Canvas
Unity Menu Item → GameObject → UI → Canvas

C.Canvas 下新增空物件，做为 Layout Controllers (以下简称父物件)

D.父物件增加 Horizontal Layout Group Component
Unity Menu Item → Component → Layout → Horizontal Layout Group

E.父物件下建立5个 Button(子物件)，完成后如下，当大小改变时会自动分配子物件大小
![这里写图片描述](https://img-blog.csdn.net/20180704150151859?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
F.此时在 Button 的 Rect Transform Component 就不能进行调整，因为我们已经透过 Horizontal Layout Group 进行分配空间，在 Rect Transform 会显示目前被哪个 Layout Group 控制
![这里写图片描述](https://img-blog.csdn.net/20180704150200853?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
G.将 Padding 数值调整如图，可以看出填充区域
![这里写图片描述](https://img-blog.csdn.net/20180704150212324?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
H.将 Spacing 数值调整如图，可以看出元素区间
![这里写图片描述](https://img-blog.csdn.net/20180704150226295?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
I.接下来我们将5个 Button 增加 Layout Element Component 覆盖预设大小，用于手动设定每个元素的大小
组件位置：Unity Menu Item → Component → Layout → Layout Element

J.此时将 Horizontal Layout Group 的 Child Force Expand Width 取消勾选，不强制子物件填满额外空间，而是透过 Layout Element 手动设定

K.这裡使用几种不同的设定，来理解 Horizontal Layout Group 是怎麽取得 Layout Element size 分配子物件

■ 复习一下子物件大小分配方式，如果不清楚请回去上面 Layout Elements 部分

首先分配 Minimum Size

如果还有足够空间，分配 Preferred Size

如果还有额外空间，分配 Flexible Size



■ 将5个 Button 的 Layout Element Min Width 分别改为 20、30、40、50、60，此时可以看出每个 Button 宽度分佈，改变父物件大小时子物件大小并不会改变，因为只有分配 Min Width，并不会分配额外有效空间
![这里写图片描述](https://img-blog.csdn.net/20180704150244487?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
此时改变 Horizontal Layout Group 的 Child Alignment，可以看出元素对齐
![这里写图片描述](https://img-blog.csdn.net/20180704150313222?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
父物件 Layout Properties Min Width = 5个按钮宽(20+30+40+50+60=200) + Spacing(40) + Padding Left、Right(20) = 260
![这里写图片描述](https://img-blog.csdn.net/20180704150320715?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
■ 现在将第1个 Button 的 Layout Element 数值调整如图
![这里写图片描述](https://img-blog.csdn.net/20180704150335280?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这边设定 Preferred Width 为 100

1.首先分配 Minimum Size(20)

2.空间足够的情况下，将会分配剩下的 Preferred Size (20~100 空间)，如下所示
![这里写图片描述](https://img-blog.csdn.net/20180704150351852?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
■ 现在将第1个 Button 的 Layout Element 数值调整如图
![这里写图片描述](https://img-blog.csdn.net/20180704150404105?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这边设定 Flexible Width 为 1

1.首先分配 Minimum Size(20)

2.如果还有足够空间，将会分配剩下的 Preferred Size (20~100 空间)

3.如果还有额外空间，分配剩下 Flexible Size，如下所示
![这里写图片描述](https://img-blog.csdn.net/20180704150426458?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
■ 现在将 Horizontal Layout Group 的 Child Force Expand Width 勾选，让子物件强制填满

1.首先分配 Minimum Size(20)

2.如果还有足够空间，将会分配剩下的 Preferred Size (20~100 空间)

3.如果还有额外空间，分配剩下元素 Flexible Size 与 Child Force Expand Width
![这里写图片描述](https://img-blog.csdn.net/20180704150450939?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
<font color="red">结论：</font>
上面我们看到，所有元素会先被分配 Minimum Size，接下来还有足够空间，将会分配剩下的 Preferred Size，最后才是 Flexible Size 与 Child Force Expand Width

至此我们了解到 Horizontal Layout Group 是怎麽取得 Layout Element size 分配子物件


###Vertical Layout Group
![这里写图片描述](https://img-blog.csdn.net/20180704150527249?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
垂直方向(Height) 排列子物件，与 Horizontal Layout Group 只差在水平或是垂直，这边不在赘述

组件位置：Unity Menu Item → Component → Layout → Vertical Layout Group

###Grid Layout Group
![这里写图片描述](https://img-blog.csdn.net/20180704150550241?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
网格方式排列子物件

组件位置：Unity Menu Item → Component → Layout → Grid Layout Group


Padding：填充内部空间

Cell Size：每个元素的宽高
![这里写图片描述](https://img-blog.csdn.net/20180704150601473?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Spacing：每个元素间格

Start Corner：开始排列的角落(位置)，又分为 “左上”、”右上”、”左下”、”右下”，请仔细看元素数字
![这里写图片描述](https://img-blog.csdn.net/20180704150623822?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Start Axis：”水平” 或是 “垂直” 排列，请仔细看元素数字
![这里写图片描述](https://img-blog.csdn.net/201807041506317?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Child Alignment：当没有填满全部空间时，子物件对齐位置

Constraint：排列限制

Flexible：自动根据大小弹性排列

Fixed Column Count：限制排列 “行数(直)”

Fixed Row Count：限制排列 “列数(横)”
### Layout Fitter
控制著 Layout Controllers 自身大小，大小取决于子物件，或是设定的大小比例，又分为 Content Size Fitter 与 Aspect Ratio Fitter

<font color="red">Content Size Fitter</font>

![这里写图片描述](https://img-blog.csdn.net/20180704150657844?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
控制著 Layout Controllers (父物件)自身大小，大小取决于子物件的 Minimum 或是 Preferred 大小，能透过 Pivot 改变缩放方向

组件位置：Unity Menu Item → Component → Layout → Content Size Fitter



Horizontal、Vertical Fit：水平、垂直 适应调整

None 不调整

Min Size 根据子物件的 Minimum 大小进行调整

Preferred Size 根据子物件的 Preferred 大小进行调整



透过实例理解：

如果我们现在有一个需求，必需要让 “父物件大小” 根据 “子物件大小” 进行缩放，完成如下 (方便明显看出父物件大小，增加黑色外框)
![这里写图片描述](https://img-blog.csdn.net/20180704150730783?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
A.开新 Scene
Unity Menu Item → File → New Scene

B.新增一个 Canvas
Unity Menu Item → GameObject → UI → Canvas

C.Canvas 下新增空物件，做为 Layout Controllers (以下简称父物件)

D.父物件增加 Horizontal Layout Group Component
Unity Menu Item → Component → Layout → Horizontal Layout Group

这时如果增加 Button(子物件)，上面有提到，Horizontal Layout Group 会根据子物件的 Layout Element 进行分配子物件大小，而不会修改父物件本身的大小，如下所示 (方便明显看出父物件大小，增加黑色外框)
![这里写图片描述](https://img-blog.csdn.net/20180704150742562?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
E.父物件下增加 Button(子物件)，并增加 Layout Element Component 覆盖预设大小，Minimum Width 调整为 100
组件位置：Unity Menu Item → Component → Layout → Layout Element


F.父物件增加 Content Size Fitter Component，Horizontal Fit 调整为 Min Size，透过子物件 Minimum Width 调整父物件本身大小 (Horizontal 方向其实就是取得子物件 Width)
![这里写图片描述](https://img-blog.csdn.net/20180704150754960?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
G.此时如果 Button 複製增加，父物件本身的大小也会跟著改变，如下所示
![这里写图片描述](https://img-blog.csdn.net/20180704150812962?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
H.调整父物件的 pivot，可以控制缩放方向，如下所示
![这里写图片描述](https://img-blog.csdn.net/20180704150819974?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
I.通过上面实例，我们首先使用 Horizontal Layout Group 排列子物件，并在子物件增加 Layout Element 覆盖预设大小，最后透过 Content Size Fitter 取得子物件 Layout Element 设定父物件大小，至此父物件大小就会根据子物件大小进行缩放

###Aspect Ratio Fitter
![这里写图片描述](https://img-blog.csdn.net/20180704150841556?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
控制著 Layout Controllers 自身大小，按照物件宽高比调整大小，能透过 pivot 改变缩放方向

组件位置：Unity Menu Item → Component → Layout → Aspect Ratio Fitter



Aspect Mode：调整模式

None：不调整

Width Controls Height：

基于 Width 为基准，依据比例改变 Height
![这里写图片描述](https://img-blog.csdn.net/20180704150848186?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
当 Width 改变时，Height 会依比例改变
![这里写图片描述](https://img-blog.csdn.net/20180704151008246?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Height Controls Width：

基于 Height 为基准，依据比例改变 Width


![这里写图片描述](https://img-blog.csdn.net/20180704151014444?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
当 Height 改变时，Width 会依比例改变
![这里写图片描述](https://img-blog.csdn.net/20180704151041640?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Fit In Parent：依据比例将 宽高、位置、anchors自动调整，使此图形大小在父物件中完全贴齐，此模式可能不会包覆所有空间

调整比例 (方便明显看出父物件增加黑底)

![这里写图片描述](https://img-blog.csdn.net/20180704151048629?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
调整父物件大小，物体会依据比例贴齐父物件
![这里写图片描述](https://img-blog.csdn.net/20180704151103337?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Envelope Parent：依据比例将 宽高、位置、anchors自动调整，使此图形大小完全包覆父物件，此模式可能会超出空间

调整比例 (方便明显看出父物件增加黑框)
![这里写图片描述](https://img-blog.csdn.net/20180704151122485?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
调整父物件大小，物体会依据比例包覆父物件
![这里写图片描述](https://img-blog.csdn.net/20180704151129262?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Aspect Ratio：比例，此数值为 宽/高


<font color="red">差别：</font>

Content Size Fitter 是透过子物件自动进行调整大小

Aspect Ratio Fitter 是透过数值(宽高比)进行调整


<font color="red">后记</font>
Auto Layout System 可以快速、方便的排列多个 UI，当大小改变时会自动调整内容，也能应用在多层崁套下，在日后调整与修改上也是非常方便与直觉，是 UI 系统中必学的功能之一 !!


<font color="red">参考资料</font>
■ Unity – Manual- Auto Layout

http://docs.unity3d.com/Manual/UIAutoLayout.html

■ Unity – Manual- Auto Layout_UI Reference

http://docs.unity3d.com/Manual/comp-UIAutoLayout.html


----------
## Unity：UGUI 应对各种萤幕自动调整大小及位置

### UGUI自适应

以前曾经发佈过两篇有关 GUI 自动调整的文章「Unity 自动调整 GUI 缩放比例及位置」以及「Unity：应对各种萤幕比例自动调整画面缩放及位置」，
自从 Unity 于 4.6 版发佈了新的 GUI 系统（UGUI）之后，使用 Unity 製作 UI 变得更为简单方便，同时也比较不需要依赖第三方製作的 UI 工具了，在事件系统上也有了一些革新，UI 事件与 Component 之间变得更加视觉化，彼此间的藕合度大大降低，而使得使用上更为弹性，虽然，UGUI 解决了许多以前 UI 製作上面的问题，但实际开发时，还是有些部分需要使用者做些调整。

在 Unity 4.6 发佈之后，UGUI 相关的游戏物件，不像一般的游戏物件使用 Transform，而是 Rect Transform，其中多了宽、高以及 Anchors、Pivot 等栏位，这些栏位对 UI 製作的视觉化设计上有很大的帮助，而其中的 Anchors 更是让 UI 整体应对于各种不同比例画面有著更加强大及弹性的控制权。

如果在 Unity 的 Game view 使用 Free Asspect 观察画面时，任意的拉动视窗边缘去调整画面比例，可以发现 UGUI 本身是并不会移动位置或缩放大小，如果将 320x480 变为 480x320，就有可能使 UI 被画面边缘切掉，这时候 Anchors 就能发挥很大的用处，使得 UI 图片或按钮等元素都能跟随画面比例的改变而变更其宽高大小，但其美中不足的是文字并不会因此跟随改变大小，当然，如果文字有勾选 Best Fit 的话，也会有自动变更 Size 的效果，只是，按钮、图片等 UI 元素跟随画面比例改变，也改变了本身的宽高比例，但文字却只是改变 Font Size，所以，当画面比例改变了，图片变形了，文字并未跟著变形，那麽文字与其他 UI 之间的距离或是按钮边缘与按钮内的文字的间隙就可能会发生奇怪的变化，这些都是我们所不希望看到的，所以，最好的状况应该是，当画面比例改变了，UI 的图片、文字、按钮等文字都能维持原本的长宽比例自动缩放大小及位置。


![这里写图片描述](https://img-blog.csdn.net/20180704151256682?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
正常的纵向画面
![这里写图片描述](https://img-blog.csdn.net/20180704151303770?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
转为横向时，画面被切掉了
![这里写图片描述](https://img-blog.csdn.net/20180704151310557?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
为 UI 设置好 Anchors
![这里写图片描述](https://img-blog.csdn.net/20180704151317816?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
转为横向后，UI 自动变形调整，但文字不同


当在场景中建立第一个 UGUI 的游戏物件时，一定会先自动建立 Canvas 的游戏物件，在这个物件中，预设配置了 Canvas、Canvas Scaler、Graphic Raycaster 这三个 Component，其中的 Canvas Scaler 正是用来控制 UGUI 整体的缩放，所以只要依照以下步骤，就能达到最基本的依照原比例调整 UI 的目的：

- 1.让所有的 UI 的 Anchors 维持在预设值(0.5)。
- 2.将 Canvas Scaler 的 Ui Scale Mode 栏位设置为 Scale With Screen Size。
- 3.在 Reference Resolution 栏位输入基础解析度的宽(X)、高(Y)。
- 4.Screen Match Mode 栏位选择 Match Width Or Height。
- 5.如果画面为横向，Match 栏位选输入 0，如果画面为纵向，Match 栏位输入 1。


![这里写图片描述](https://img-blog.csdn.net/20180704151434340?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
让 Anchors 维持在预设值 0.5
![这里写图片描述](https://img-blog.csdn.net/20180704151441376?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
设置 Canvas Scaler 的相关栏位

如此，当画面从纵向转为横向，GUI 就会自动维持原比例调整其大小及位置。
![这里写图片描述](https://img-blog.csdn.net/2018070415145995?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
转为横向画面，UI 的比例及位置不变

但是，这是在比例相同直接横向转纵向时才会如此理想，当遇到比例不同的画面，例如原本是 iPhone 4 的 640x960 设计的 UI 佈局，遇上了 iPhone 5 的 640x1136 画面，就会发生左右两边被切掉的情形。
![这里写图片描述](https://img-blog.csdn.net/20180704151522360?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
当比例不同时，边缘被切掉了

这时我们就必须将 Canvas 游戏物件的 Canvas Scaler componet 的 Match 栏位改为 0，使其 UI 依照原本比例缩放调整到画面内。
![这里写图片描述](https://img-blog.csdn.net/20180704151537898?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
调整 Canvas Scaler 的 Match 使被切掉的部分回到画面内。

为了因应可能遇到各种未知的画面比例（特别是 Android 以及桌机平台），我们可以为 Canvas 挂上包含以下这一段程式码的 component 来自动调整 Canvas Scaler component 的 Match 栏位。

```csharp
void Awake(){

    CanvasScaler canvasScaler = GetComponent<CanvasScaler>();

    float screenWidthScale = Screen.width / canvasScaler.referenceResolution.x;
    float screenHeightScale = Screen.height / canvasScaler.referenceResolution.y;
 
    canvasScaler.matchWidthOrHeight = screenWidthScale > screenHeightScale ? 1 : 0;
}
```

完成以上的工作，将来在 UI 製作上就会轻鬆很多，如果没有特殊需求，基本上不需要动到 UI 的 Anchors 值，大部份都只是单纯的调整 UI 大小及位置就行了，面对各种画面比例，也将维持原设计的佈局自动调整，另外，如果需要製作 UI 的动态缩放、位移等等使画面更生动的话，以变更其 Local space 的数值为主，那麽比例缩放的部分都不需要特别去考虑，Canvas Scaler 会帮我们处理。

不过，有一个例外，目前是无法使用此办法解决的，就是当 UI 文字使用了 Rich Text 的 Size 标签去指定文字大小时，文字的大小是不会受到 Canvas Scaler 影响的，它指定了绝对的文字 Size，这一点要特别注意。

附带一提，除了 UI 佈局以及变化之外，有时候也会想在 UI 上做些粒子特效，或是让非 GUI 的游戏物件在画面上与 UI 呈现互动的效果，此时，只要把 Canvas 游戏物件的 Canvas component 的 Render Mode 栏位改为 Screen Space - Camera 并设定好 Render Camera 及 Plane Distance 的内容，那麽放置于 UI 平面与其指定的 Camera 中间的物件就可以显示于 UI 之上与其互动，如果是 Sprite 游戏物件，则可以依据 Sorting Layer 与 GUI 之间调整前后顺序；不过，当遇到自动调整过的 GUI 画面，这些非 GUI 的游戏物件大小及位置可能会与预期的不同，为了避免此情况发生，可以参考之前发佈过的文章内容「<font color="red">Unity：应对各种萤幕比例自动调整画面缩放及位置</font>」（该文章是Unity4.3版本的，所有就没给大伙们搞过来）去调整 Camera。
![这里写图片描述](https://img-blog.csdn.net/20180704151624174?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
改变 Canvas 的 Render Mode 为 Screen Space - Camera

PS：目前使用 Unity 版本为 5.0.1f1。


----------
## Unity：制作 UGUI 的 UI 流程管理机制
### 为什么需要UI流程管理机制
自从 Unity 4.6 发布新 GUI 系统之后，Unity 终于有个比较完整的视觉化编辑 UI 工具可以使用，于是，我们可以很方便、直觉的在画面上添加按钮，使用拖曳、下拉选单等几个动作就能设置好 UI 事件应该执行哪个 GameObject 上的哪个 Component 中的功能，所以透过 UI 去触发我们自己撰写的程式功能也变得非常简便，但是整个游戏内容可能会有相当多的画面，不同的 UI 按钮或行为将转向不同的画面，也需要开启不同 UI 视图，如果没有规划好 UI 画面的动线规则，在複杂的画面转换间，将可能会发生很难维护的情形，甚至在未来多次的变更修改后产生不必要的 Bug 而使得 UI 画面动线变得相当混乱。

不管 UI 做得多漂亮、多华丽，反馈、排版、效果等这些细工做得多好，如果整体架构、动线流程出错了，做了那么多细工的辛苦就会白费掉，因为，会让玩家迷路的 UI 动线，使用起来就是会不舒服；即使使用 UI 的动线设计都没问题，但没有规划一致性的流程管理，在制作及维护上也容易造成一些困扰。

举例来说，大部份的 UI 画面都会有个「返回」按钮可以回到前一个画面，当这个 UI 画面是从 A 画面进来时，应该可以返回到 A 画面，从 B 画面进来时，应该可以返回到 B 画面，就像我们平常使用网页浏览器一样，不管目前的页面是从哪裡进来的，按下「上一页」就是会回到进来目前网页之前的那个网页，UI 上的「返回」按钮也是一样的功能，只是，这个返回是个按钮，它会被我们设置执行某个 Component 的某个功能，透过这个功能来使它开启某个 UI 画面，所以，我们很直觉的就会有点像写死的程式一样，A 画面进入 B 画面，所以点击 B 画面的返回按钮就是开启 A 画面，如果，C 画面也可以进入 B 画面呢？那麽就做个暂存纪录让 B 画面判断是从 A 还是 C 画面进入的，或者进入 B 画面的同时，通知 B 画面说是从哪裡进来的，此时，第一个问题来了，每当多出一个会进入到 B 画面的画面，B 画面的返回按钮功能就要进行修改来达到正确的判断；第二个问题，假设 A、B 画面都可以进入 C 画面，C 画面可以进入 A、D 画面，A、D 画面都可以进入 B 画面... 等等较深入的交叉动线，新增一个 UI 画面或者更动到某个 UI 画面流程，就几乎全部画面的返回键都要进行修改，这种工程实在太浩大了，牵一髮而动全身的维护状况是最容易产生未知 Bug 的。

有于此，设计的 UI 流程管理机制至少要满足两个条件：

- 玩家使用时不会迷路。
- 不管未来变更多少画面，都不需要修改或维护返回按钮。

要满足这两个条件，有一个最直接的做法，也就是前面提到的，网页浏览器的「上一页」功能，这个功能是如何实现的呢？概念其实就是浏览纪录，也就是说我们只要把玩家操作时，所切换过的 UI 画面依序记录下历程，再依序返回，这样子，玩家使用时不会迷路，我们在制作及维护时，也不需要为每个画面的返回按钮客制化。

流程上的概念没问题了，另外就是一些其他要注意的部分：

- 每个 UI 画面间的进场和退场动态，
- 进退场画面进行中时，UI 事件不能有作用。
- 退场 UI 画面即使游戏画面上看不到，也不应该让它持续执行。

前面写了那麽多，最重要的还是如何实作，以下是整个实作过程的影片，影片内并没有太详细说明，所以，在下面的文章内容提供进一步的详细解说：

### UI 画布的基本结构
先决定我们要使用怎么样的画面比例、解析度来编排制作 UI，通常会跟 Game view 设置好要预览的画面相同
![这里写图片描述](https://img-blog.csdn.net/20180704151814205?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
建立 UI 的主画布(Canvas)，并在其 Canvas Scaler 设置好 Ui Scale Mode 以及 Reference Resolution，这主要是为了使将来 UI 面临不同画面比例的装置时，可以有基本的自适应调整

![这里写图片描述](https://img-blog.csdn.net/20180704151842124?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

接下来，每个 UI 画面都应该另外建立 Canvas，并将画面内的其他 UI 元件放置在该 Canvas 之下，如此前面主画布里设置好的 Canvas Scaler 也将直接适用于其下的每个 UI 画面，另外，这麽做的另一个好处是，之后可以直接变更 Canvas 的 Sort Order 来排列 UI 画面的前后顺序。同时，每个画面除了 UI 元件之外，还需要使用 Image 制作一个覆盖全画面大小的「透明遮挡层」，并放置于其它 UI 元件之上，平常处于 GameObject 被关闭的状态，当 UI 画面在进行进场、退场动态时，才将它开启，这样可以保护 UI 的 Button 不会被点击到。
![这里写图片描述](https://img-blog.csdn.net/20180704151849329?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 制作动画
制作 UI 画面的进场、退场动画档分别命名为 Open、Closed，最简单的就是淡入、淡出或者是放大、缩小的方式，当然，也可以设计更丰富的动画效果，最重要的是，进、退场动画档播放期间要记得开启前面製作的「透明遮挡层」。
![这里写图片描述](https://img-blog.csdn.net/20180704151913675?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
动画播放期间开启「透明遮挡层」

### 设置动画控制
当我们为 UI 画面制作好进场、退场的动画档之后，开启 Animator view，可以发现 Unity 已经自动帮我们在 Animator 建立两个与动画档相同名称的动画状态(State)。
![这里写图片描述](https://img-blog.csdn.net/20180704151945755?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
预设，Unity 会自动将 AnimationClip 放到 Animator 做为  state


接着，从 Open 状态建立 Transition 连接到 Closed 状态，并在 Inspector view 设置其中的过渡条件，由于 Unity 预设会认为两个动画状态之间的转换是需要混合过程的（例如，人物角色閒置的动作转换到跑步动作），这个混合过程会牺牲一点动画档本身的播放时间，在 UI 画面转换的动画并不需要这种混合过程，所以在此，我们可以直接将过渡时间 Transition Duration 设置为 0，让它很明确的播放 Closed 动画；由于，Open 状态并不需要播放结束之后马上转换到 Closed 状态，而是等待程式通知 Animator 触发 Out 参数才转换到 Closed 状态，所以，在此也要取消勾选 Has Exit Time，并且在 Conditions 设置一个过渡条件 Out。
![这里写图片描述](https://img-blog.csdn.net/20180704152000380?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
设置 Transition 裡的几个相关栏位

然后，再从 Closed 状态建立 Transition 连接到 Exit 状态，并在 Inspector view 设置过渡时间 Transition Duration 为 0，把 Exit Time 设置为 1，为什麽要这样做呢？理论上，当 Closed 播放完毕之后，UI 画面就会完成退场的动作，在它下次进场之前，都不需要去理会它，但为了不让不在游戏画面中的 UI 画面继续执行，浪费效能资源，我们会希望完成退场之后，可以将这些 UI 画面整个 GameObject 都关闭，所以，我们后续会撰写 Animator 的 State 专用的 StateMachineBehaviour script来做这件事，而 StateMachineBehaviour 的 OnStateExit 是在状态结束「之后」才会被呼叫，所以，如果没有为 Closed 建立 Transition 连接出来的话，StateMachineBehaviour 的 OnStateExit 将不会执行，另外，Exit Time 是以 0 到 1 来代表整个动画时间从开始到结束的时间点，我们希望 Closed 是能够明确的播放到结束时间才真的结束，所以在此的 Exit Time 才直接设置为 1。
![这里写图片描述](https://img-blog.csdn.net/20180704152027618?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
设置好正确的时间值

由于，在 Unity 裡建立动画档时，预设会认为该动画是要重複循环播放的，所以，我们还要另外手动找出 Open 及 Closed 动画档，并在 Inspector view 裡将 Loop Time 取消勾选。
![这里写图片描述](https://img-blog.csdn.net/20180704152046911?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
取消 Loop Time

###动画状态脚本
这部分，最主要的是要让 UI 画面退场之后，也能同时将它的 GameObject 整个关闭掉，以往，我们是在 GameObject 挂上我们自已写的 Script，并在製作动画档时，在 Animation view 加入 Event，自从 Unity 5.0 发佈后，就不再需要这麽麻烦，我们可以直接点选 Animator view 裡的任意状态，并在 Inspector view 中点击 Add Behaviour 按钮，建立 Animator 状态专用的 Script，如此就可以直接透过撰写这个 Script 的内容来对 Animator 裡的状态针对状态开始、进行中、结束.. 等等的时机进行其他操作，因此，我们要让 UI 画面退场之后关闭本身 GameObject，只需要为 Closed 建立 StateMachineBehaviour 并在 OnStateExit 加入简短的一行程式码即可。
![这里写图片描述](https://img-blog.csdn.net/20180704152117461?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
为 Animator 的 State 加入 Behaviour

```csharp
using UnityEngine;

public class UIStateClosed : StateMachineBehaviour {

    override public void OnStateExit(Animator animator, AnimatorStateInfo stateInfo, int layerIndex) {

        animator.gameObject.SetActive(false);
    }
}
```

### UI流程管理脚本
接下来算是重头戏了，做完前面的工作，剩下的就是靠这个 Script 去做主要的管理，而这个 Script 主要做哪些工作呢？我们将其列出如下：

1. 建立一个列表来纪录 UI 画面历程，用来将曾经进入的 UI 画面依序记录下来，以方便依序返回。
2. 先将第一个开启的 UI 画面记录到历程中，当历程只剩下一笔时，就不能再返回。
3. 不可以进入与目前 UI 画面相同的画面。
4. 即将要进入或返回的目标画面必须移到最上层。
5. 往前进入的目标画面必须记录到历程中。
6. 返回时，必须要将目前画面从历程纪录中移除。

另外，之前的文章「Unity：认识 Tag 与 Layer 的差异与应用」曾提到过程式码中应该避免写死的字串值，所以，这个 Script 因为会使用到 Animator 的 Parameters 中设定的名称来通知 Animator 转换到 Closed 状态，所以也需要宣告一个 public 栏位来设置该名称。

有了以上的工作需求，我们就可以撰写出如下的程式码：

```csharp
using UnityEngine;
using System.Collections.Generic;

public class UIManager : MonoBehaviour {

    public GameObject startScreen;
    public string outTrigger;
    private List<GameObject> screenHistory;

    void Awake(){

        this.screenHistory = new List<GameObject>{this.startScreen};
    }

    public void ToScreen(GameObject target){

        GameObject current = this.screenHistory[this.screenHistory.Count - 1];

        if(target == null || target == current) return;

        this.PlayScreen(current , target , false , this.screenHistory.Count);
        this.screenHistory.Add(target);
    }

    public void GoBack(){

        if(this.screenHistory.Count > 1){

            int currentIndex = this.screenHistory.Count - 1;
            this.PlayScreen(this.screenHistory[currentIndex] , this.screenHistory[currentIndex - 1] , true , currentIndex - 2);
            this.screenHistory.RemoveAt(currentIndex);
        }
    }

    private void PlayScreen(GameObject current , GameObject target , bool isBack , int order){

        current.GetComponent<Animator>().SetTrigger(this.outTrigger);

        if(isBack){

            current.GetComponent<Canvas>().sortingOrder = order;

        }else{

            current.GetComponent<Canvas>().sortingOrder = order - 1;
            target.GetComponent<Canvas>().sortingOrder = order;
        }

        target.SetActive(true);
    }
}
```
### 套用
完成以上的工作以及程式撰写之后，只要建立一个空的 GameObject 并将 UIManager script 挂上去，在 Start Screen 栏位放入第一个要显示的 UI 画面 GameObject，在 Out Trigger 填写跟 Animator 设置的 Trigger 参数相同的名称。
![这里写图片描述](https://img-blog.csdn.net/20180704152221469?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
设置 Start Screen 及 Out Trigger 栏位

然后，在每个 UI 的 Button 中的 On Click 设置附有这个 UIManager script 的 GameObject，如果按钮是要前往下一个画面，则在下拉选单选择 ToScreen 并在其参数栏位设置目标 UI 画面的 GameObject。如果按钮是要返回上一个画面，则在下拉选单选择 GoBack。

![这里写图片描述](https://img-blog.csdn.net/20180704152231326?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
前往下个 UI 画面，选择 ToScreen
![这里写图片描述](https://img-blog.csdn.net/20180704152252593?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
设置好下个 UI 画面的 GameObject
![这里写图片描述](https://img-blog.csdn.net/20180704152314832?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
返回上个 UI 画面，选择 GoBack

接下来，执行时，就能将每个进入的画面历程记录下来，并在每次返回时依序返回上一个画面，每个即将显示的 UI 画面也将会是在游戏画面的最上层，而不会被遮挡到，同时，在画面的进、退场动态期间也不会被不小心按到按钮而跳到非预期的画面；由于，动线被定义为从哪裡进去就从哪裡返回，所以，使用者在众多的画面浏览间，会比较直觉的操作而不容易迷路，同时，因为画面历程的纪录，返回按钮一律透过 GoBack 去处理时，将来不管是新增多少 UI 画面，或是 UI 画面流程如何改变，返回功能都不需要再进行任何修改，也可以返回到正确的画面。

以上，这个简单的 UI 流程管理机制算是完成了，之后，我们如果对于 UI 执行转换动作的逻辑要进行变更（例如，目标画面的排序规则），则只需要修改 UIManager 裡面的 PlayScreen；如果，要换成别的进场、退场动画效果，也只需要替换掉 Animator 裡面各状态的 motion；如果做了多种版本的 UIManager 并存于同一个专案或场景中，也可直接变更 UI 裡 Button 的 On Click 设置，整个机制相当的弹性，但是，玩家操作上却不至于混乱，在后续维护上也将会更为轻松。

案例工程：http://pan.baidu.com/s/1sk9bnGh 密码：do7y

PS：目前使用 Unity 版本为 5.1.0f3


----------
## Unity：使用 UnityEngine.Events 让程式更灵活、稳定
自从 Unity 4.6 发佈新的 GUI 系统之后，我们可以从所建立的 GUI Control 发现新的事件栏位，例如，建立一个 Button，可以从 Inspector 视窗裡面的 On Click 栏位指定当按钮被点击时要去执行那一个 GameObject 上的哪个 Component 的功能，使得按钮事件可以更视觉化、更弹性的编辑，当然，其他的 GUI Control 也都有类似的栏位可以做这样的设置，而这个栏位则是由 UnityEngine.Events 底下的 UnityEvent 型别所产生的，我们自己所製作的 Component 同样也可以提供这样的栏位，使可以视觉化编辑，以及让程式更加弹性。

在此影片使用两个范例来示范作法，透过画面操作以及语音的说明，相信可以更加了解到 


首先，简单来说明一下 Unity 写作程式的基础，就是当我们在 Project 视窗建立我们自己的 Script 之后，将这个 Script 附加到 GameObject 之上，就会成为该 GameObject 的 Component，通常，变数栏位（Field）如果是使用 public 修饰字来宣告，而其型别是可序列化的话，在 Inspector 视窗就会产生可以设定其值的栏位，这是因为 Unity 本身预设会对 public 栏位自动序列化的关系。
![这里写图片描述](https://img-blog.csdn.net/20180704152409778?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
public 的变数栏位会出现在 Inspector 视窗，可供编辑。

可是，如果 Class 裡所宣告的栏位并没有要提供给外部存取的话，使用 public 修饰字就有些不恰当，所以，当使用 private 或 protected 等其他修饰字来宣告时，如果，希望该栏位变数也可以在 Inspector 视窗出现可编辑的栏位，可以在它的上一行插入 SerializeField 的 Attribute（特性），那麽 Unity 就知道这个栏位是要序列化使可以在 Inspector 视窗编辑。


![这里写图片描述](https://img-blog.csdn.net/20180704152420228?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
有 SerializeField 的 private 变数栏位也会出现在 Inspector 视窗。

如果，要製作像是 UGUI 的 Button 那样的事件栏位，最基本的就是要在程式档最上方加个 using UnityEngine.Events，然后只要是使用 UnityEvent 为型别宣告的栏位，都会有个像是 Button 那样的事件栏位。
![这里写图片描述](https://img-blog.csdn.net/20180704152442110?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
UnityEvent 型别的变数栏位，在 Inspector 视窗会出现事件栏位。

这个 UnityEvent 栏位在编辑器上的编辑相当弹性，当目标 Component 裡面含有使用 public 宣告的属性（Property）或方法（Method），并且传入值的型别为 bool、int、float、string 其中一种的话，就能直接在这裡选择并提供传入的值，而且可以设置多个不同型别的目标，并在之后程式执行时直接呼叫执行，另外，如果 Method 并没有宣告传入参数的话，在此也可以选择设置，如果是宣告两个以上的参数，在这边的选单就不会出现了。

虽然 UnityEvent 栏位相当方便和弹性，不过，它所传入的值是在编辑器上面设置的，而且只能设置一个值，如果，我们的 Component 中的程式想要直接提供参数值给目标的话，光靠 UnityEvent 就没办法做到，幸好，UnityEngine.Events 除了 UnityEvent 之外，还另外提供了四个泛型类给我们扩充使用。

我们在影片中的两个范例可以看到，其中的 PassEvents Script 专门用来撰写扩充 UnityEvent 泛型类的新事件型别，相关资料可以参考官方文件的 UnityEngine.Events 相关资料，例如 UnityEvent<T0>。总共有 UnityEvent<T0>、UnityEvent<T0,T1>、UnityEvent<T0,T1,T2>、UnityEvent<T0,T1,T2,T3> 这四个可供扩充，其实，他们所具备的功能都是一样的，只是，能接受的参数数量不同而已，也就是说，我们可以透过继承这几个泛型类来宣告我们自己需求的参数型别的 UnityEvent，而且可容纳的参数数量可以有一到四个。例如：

```csharp
[System.Serializable]
public class PassString : UnityEvent<string>{}

[System.Serializable]
public class PassColor : UnityEvent<Color>{}

```
因为，所宣告的 class 希望用于宣告栏位变数时，可以显示于 Inspector 视窗，除了使用 SerializeField 标示在栏位上面之外，该型别也必须是可序列化的才行，所以，还要在 class 上方也加上 System.Serializable。

到这裡，我们就能够使用自己定义的 UnityEvent 来宣告事件栏位，并使它可以显示于 Inspector 视窗。

###1. Simple Computer
第一个范例裡，主要是製作一个简单的计算器来示范 UnityEngine.Events 的用法以及所带来的好处；製作任何东西之前，一定要先了解到这个东西的使用目的以及功能有哪些，所以，我们先用简单的 UI 排出一个运算式的样子，这个运算式提供了两个输入栏位、一个运算符号的文字、一个显示计算结果的文字，以及四个运算功能按钮。
![这里写图片描述](https://img-blog.csdn.net/20180704152516726?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
在此，我们先定义计算器的基本功能：

- 点击运算功能按钮，会依照计算种类去改变运算符号的文字。
- 点击运算功能按钮之后，在 UI 显示计算结果文字。

功能相当简单，所以，我们直接在 Project 视窗建立一个 C# Script 就差不多能处理全部的功能，先将此 Script 命名为 MyComputer。

由于，最后会将计算结果转换为字串传给 UI 去显示，在这裡会使用到 UnityEvent 去传递资料，只是一般的 UnityEvent 并无法直接在程式呼叫时就带入参数，所以我们还需要使用前面提到的做法来宣告可以带字串参数的 UnityEvent<T0>，因为，宣告这个衍生类只需要写一行，而且，将来还可能会宣告好几个这种带不同参数的 UnityEvent<T0>，所以，我们还要在 Project 视窗建立一个名为 PassEvents 的 C# Script，专门用来写这些 Class 用，如果分类好，这支 Script 还可以直接拿到其他专案中使用。

在 PassEvents Script 裡面，首先，由于是要製作 UnityEngine.Events 的那些泛型类的衍生类，所以，最开头一定要先写一行 using UnityEngine.Events，接下来，在这个示范裡只会用到传递字串，所以，先只宣告一个能传递字串的 Class 就好。

```csharp
using UnityEngine.Events;

[System.Serializable]
public class PassString : UnityEvent<string>{}
```
别忘了 System.Serializable，否则，在 Inspector 视窗会看不到栏位。

而在 MyComputer Script 之中，我们这个简单计算器主要就是对两个计算值计算出结果而已，配合 UI 的使用，所接收到的值原本会是字串，但我们在程式中却必须做为数值来计算，所以，先宣告两个仅限内部存取的栏位来暂存传入的数值，这个数值的来源，则是要由 UGUI 的 InputField 的 Edit End 事件传过来的，当然，我们在 MyComputer Script 裡面，并不用管那麽多，只要提供两个 Property 让外部可以把字串传入就行了，管他是谁要传进来的，只要提供入口，并且把传入的字串转为数值暂存下来，这样其他计算功能就能计算了。

因为 MyComputer Script 的工作主要工作就是接收计算资料并计算出结果传递出去，所以，我们需要宣告每个计算结果的事件，来表达事情的发生，至于，最后会把结果传给谁，其实，并不需要 MyComputer 去管。

目前，我们定义 MyComputer Script 只做加、减、乘、除四个计算功能，于是，就直接将获得的数值计算出结果，并将结果转为字串后，透过 Invoke 来呼叫执行个别对应的事件，那麽计算器的基本功能就算完成了。

```csharp
private float _value1;
private float _value2;

[SerializeField]
private PassString onAdd;
[SerializeField]
private PassString onSubtract;
[SerializeField]
private PassString onMultiply;
[SerializeField]
private PassString onDivide;

public string value1{
    set{
        float.TryParse(value , out this._value1);
    }
}

public string value2{
    set{
        float.TryParse(value , out this._value2);
    }
}

public void Add(){

    this.onAdd.Invoke((this._value1 + this._value2).ToString());
}

public void Subtract(){

    this.onSubtract.Invoke((this._value1 - this._value2).ToString());
}

public void Multiply(){

    this.onMultiply.Invoke((this._value1 * this._value2).ToString());
}

public void Divide(){

    if(this._value2 == 0) return;

    this.onDivide.Invoke((this._value1 / this._value2).ToString());
}
```
因此，获得以上的程式码，其中，除法 Divide() 的部分要特别注意，因为，除法裡的除数不得为零，不然，会发生错误，所以计算之前要先判断一下，如果除数为零，就不要执行后面的计算。

写好 MyComputer Script 的内容之后，在 Unity 中，先建立一个空物件并且赋予它 MyComputer 成为它的 Component，这时候，可以很明显地从 Inspector 视窗看到分别为加、减、乘、除的事件栏位。
![这里写图片描述](https://img-blog.csdn.net/20180704152607693?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
MyConputer 出现加、减、乘、除的事件栏位。

接下来就可以在 UI 以及 MyComputer 间设置彼此之间的关系，首先，需要让 UI 输入的值可以传递给 MyComputer，所以，要从两个 InputField 中的 End Edit 事件分别设置将输入的字串传给 MyComputer 的 value1 以及 value2 两个 Property。
![这里写图片描述](https://img-blog.csdn.net/20180704152626178?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Value1 Field 输入的字串传给 MyComputer 的 value1
![这里写图片描述](https://img-blog.csdn.net/20180704152632592?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Value2 Field 输入的字串传给 MyComputer 的 value2

在这裡可以发现这个 End Edit 其实也是跟我们所宣告的 PassString 一样的东西，设置好之后，不需要在 Inspector 视窗上设置传入什麽字串，因为，这个 End Edit 事件是当 InputField 中的文字输入完毕后，按下 Enter 或者点击 UI 文字栏位外的地方，使它不能被输入文字时，而视为文字输入完毕时被呼叫执行的，当他被呼叫执行时，就会将其输入的文字透过这个事件而传递出去，所以，这裡设置完毕之后，只要 UI 上的文字栏位输入完毕后，都会将所输入的内容传递给 MyComputer，而 MyComputer 则如我们程式中所写的一样，将收到的字串转为数值暂存下来。

接下来，就是个别选择 UI 上分别代表加、减、乘、除功能的按钮，并在它们的 On Click 事件栏位设置执行 MyComputer 裡面相对应的计算功能。
![这里写图片描述](https://img-blog.csdn.net/20180704152659658?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Button + 的 On Click 事件执行 MyComputer 的 Add()
![这里写图片描述](https://img-blog.csdn.net/20180704152706391?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Button - 的 On Click 事件执行 MyComputer 的 Subtract()
![这里写图片描述](https://img-blog.csdn.net/20180704152713154?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Button x 的 On Click 事件执行 MyComputer 的 Multiply()
![这里写图片描述](https://img-blog.csdn.net/2018070415272228?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Button / 的 On Click 事件执行 MyComputer 的 Divide()


而这个 On Click 事件，相信大家都非常熟悉，就是按钮被点击时触发执行。正确来说，应该是点击按钮之后，并在同一个按钮之内放开按钮才会执行。

到这裡，UI 已可将输入的资料提供给 MyComputer，并将需要执行的功能要求 MyComputer 去进行，然后，我们就要来为 MyComputer 设置它的加、减、乘、除事件，使得 UI 可以显示出结果。

回想一下我们前面写的计算器主要功能，第一步，要让计算符号改变，所以，每个计算事件，都要设置事件发生时去改变 UI 上的计算符号文字，在这裡，虽然 MyComputer 的计算事件预设是会由程式裡面传递出结果，但 UnityEvent 栏位本身就能直接传入静态参数，所以，我们的程式码裡面不用为那些计算功能撰写有关改变运算符号的部分，只要直接在 Inspector 视窗上面设置就行了。

第二步，则是要让 UI 上显示出计算结果，由于 MyComputer 程式码裡面直接将计算结果的字串传给了个别对应的事件，所以，直接为每个事件设置他们的目标是 UI 上显示结果的 Text 即可。
![这里写图片描述](https://img-blog.csdn.net/20180704152745403?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
每个计算功能事件让 UI 变更运算符号以及显示计算结果

这样子，每当 MyComputer 执行计算时，就会把字串传递给 UI 显示出来。
![这里写图片描述](https://img-blog.csdn.net/20180704152755382?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
计算结果的画面

现在，计算器需求的功能齐全了，我们可能还想要做些更细微的调整，例如，在输入的值未改变的情况下，已经执行过的计算的，就不想要再次计算，所以，要将该功能按钮关闭。在 UGUI 的 Button 中，有一个 Interactable 栏位，是用来管理使用者与按钮之间的互动开关，当它被关闭时，Button 就会失去作用而呈现较暗的颜色，所以，我们在这边可以制订一个功能是当计算结果出来之后，把所对应的功能按钮上的 Interactable 关闭，而这个功能完全不用去修改程式码，只要再次去为每个计算功能的事件栏位加入执行目标即可。
![这里写图片描述](https://img-blog.csdn.net/20180704152817914?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
计算结果之后关闭按钮。
![这里写图片描述](https://img-blog.csdn.net/20180704152832990?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
计算过的按钮都关闭了。

虽然，按钮在计算之后可以被关闭了，可是，如果输入数值的栏位内容被改变了，应该还要有可以重新计算的机会，所以，需要有个能让按钮被重新打开的功能，这时候，其实我们可以直接在两个输入资料的 InputField 的 End Edit 事件对每个按钮执行开启 Interactable 的动作，只是，如果计算器有很多个功能按钮、以及有很多个输入栏位的话，要一个一个慢慢设置，执行流程会太过分散了。

所以，我们可以想像成 MyComputer 除了负责计算之外，还提供一个状态重置的功能，这个状态重置的功能本身并不执行任何事情，只是呼叫执行状态重置事件，那麽，设置在这个事件上的目标功能，只要状态重置的功能被呼叫执行，它们都会被执行，所以，我们在 MyComputer Script 要再加上以下的程式码：

```csharp
[SerializeField]
private UnityEvent onResetStatus;

public void ResetStatus(){

    this.onResetStatus.Invoke();
}
```
而在 Unity 编辑器中，我们就可以让 MyComputer 的状态重置事件（On Reset Status）栏位，设置开启四个功能按钮的 Intractable。
![这里写图片描述](https://img-blog.csdn.net/20180704152904220?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
状态重置时，再次启用按钮。

如此，两个 InputField 的 End Edit 事件则是指定执行 MyComputer 的状态重置功能即可。

![这里写图片描述](https://img-blog.csdn.net/20180704152919463?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
End Edit 事件指定执行 MyComputer 的 ResetStatus()。

虽然，影片中状态重置事件是让按钮重新启用，但到这边也可以任意变更状态重置事件所要执行的动作，例如，让计算结果文字变成问号，那麽，当每次输入栏位重新被输入完毕之后，不但被停用的按钮会重新启用，也会使结果文字变成问号，等按下功能按钮之后才显示正确的结果。

既然有了状态重置的功能，那麽，我们是不是可以只让当前计算出结果的按钮被停用，其它按钮是启用的状态呢？这样就不用一定要重新在输入栏位输入资料完毕才能启用按钮。在做这个功能之前，要先解释一下，每个事件栏位可以指定多个执行目标，而这些执行目标其实是有执行顺序的，它会在执行完第一个之后才会执行第二个，一个一个往下执行。

因此，我们要做这个功能就不需要去修改程式码，而能直接变更各计算功能的事件内容以及顺序，让每次执行计算之后先执行状态重置，然后才显示计算结果、停用当前的计算按钮、变更运算符号。

![这里写图片描述](https://img-blog.csdn.net/20180704152935233?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
更改为先执行状态重置之后，才去执行其他行为。

到这裡，简单的计算器功能将告一段落，我们可以发现 MyComputer Script 的程式码相当的独立，只是提供传入资料的入口让传入的资料可以暂存下来，以及提供计算功能给外部执行，并将执行结果丢到事件中，至于，谁会传入资料、谁会执行计算功能、谁会回应事件的执行，通通不需要去管，因此，如果全部的事件栏位都未设置目标，当被要求执行计算时，它也能照常执行计算，而不会因为未设置目标而发生错误，而究竟事件目标应该是谁，基本上，MyComputer 也不需要知道，一切就在 Inspector 视窗上，依照实际需求去设置或变更即可。

状态重置的部分也是一样，MyComputer Script 只负责提供状态重置功能并执行状态重置事件，至于，谁要求状态重置、状态重置到底要做些什麽，都不用去管。

这样，就可以让程式撰写变得相当简洁，而实际使用上的弹性变化却非常的大，另外，因为一些需求功能的变更不需要依赖修改程式码达成，除了节省程式编译的时间之外，也可避免因为修改程式码的人为失误而发生错误，最重要的是，因为 MyComputer Script 根本就不需要知道谁要执行它的功能，以及它的事件最后会去执行什麽目标功能，所以，也使程式间的藕合度降到最低，如果将这种做法的 Script 移到其它专案中使用，也不用特别顾虑会与其它什麽程式有关联而发生许多缺漏的情形。

以下是 MyComputer.cs 的完整内容：

```csharp
using UnityEngine;
using UnityEngine.Events;

public class MyComputer : MonoBehaviour {

    private float _value1;
    private float _value2;

    [SerializeField]
    private PassString onAdd;
    [SerializeField]
    private PassString onSubtract;
    [SerializeField]
    private PassString onMultiply;
    [SerializeField]
    private PassString onDivide;
    [SerializeField]
    private UnityEvent onResetStatus;

    public string value1{
        set{
            float.TryParse(value , out this._value1);
        }
    }

    public string value2{
        set{
            float.TryParse(value , out this._value2);
        }
    }

    public void Add(){

        this.onAdd.Invoke((this._value1 + this._value2).ToString());
    }

    public void Subtract(){

        this.onSubtract.Invoke((this._value1 - this._value2).ToString());
    }

    public void Multiply(){

        this.onMultiply.Invoke((this._value1 * this._value2).ToString());
    }

    public void Divide(){
        if(this._value2 == 0) return;
        this.onDivide.Invoke((this._value1 / this._value2).ToString());
    }

    public void ResetStatus(){

        this.onResetStatus.Invoke();
    }
}
```
###2. Sphere Control
第二个范例裡，设计了五个球体个别使用相同的 Componet，但却因为实际使用时设置的不同，直接反应出不同的行为，并且示范如何让 UnityEvent 除了传递参数之外，也能带回资料。

首先，在场景中建立好五个球体，这只是 Unity 预设的原生物件，预设上，Unity 会个别为它们赋予 Sphere Collider，以及预设的 Material，Material 的 Shader 则是 Unity 5 的 Standand Shader，这部分，我们都不需要做任何更改。
![这里写图片描述](https://img-blog.csdn.net/20180704153154737?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
原生物件球体的 Componet。
![这里写图片描述](https://img-blog.csdn.net/20180704153209414?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
在此，定义了几个球体的功能需求：

- 球体可以被点击触发。
- 球体可以弹跳起来。
- 球体可以变色。

依据这几个需求，先在 Project 视窗分别建立它们个别的 C# Script，并个别命名为 SphereTouch、SphereJump、SphereDiscolor。

先来写 SphereTouch 的程式码，SphereTouch 只提供让使用者透过滑鼠点击球体时的事件反应而已，所以，SphereTouch 会有一个 UnityEvent 事件栏位，以及实作 Unity 内建的 OnMouseDown，只要 GameObject 本身有 Collider 的 Componet，当滑鼠在 GameObject 上按下按键时，就可以触发 OnMouseDown 执行其内容，而其内容裡，只是直接呼叫 UnityEvent 事件栏位执行而已，至于点击之后要反应什麽行为，那是别人的事。

```csharp
using UnityEngine;
using UnityEngine.Events;

public class SphereTouch : MonoBehaviour {

    [SerializeField]
    private UnityEvent onTouch;

    public void DoTouch(){

        this.onTouch.Invoke();
    }

    void OnMouseDown(){

        this.DoTouch();
    }
}
```
看到这个程式码可能觉得奇怪，为什麽不在 OnMouseDown 裡面直接 Invoke 呢？其实，这只是要让 SphereTouch 多提供一个外部功能，让其他物件也可以传达点击讯息进来，例如，A 物件被滑鼠点击，然后它的 On Touch 事件栏位设置去执行 B、C、D 物件的 DoTouch 功能，那麽就可以一个物件被点击，四个物件同时做出反应。

再来是 SphereJump 的程式码，让球体跳动的行为可以有很多做法，例如使用 Unity 的动画系统去做，或者给予物体一个往上的推力，再让它因为重力而落下，不过，这边为了简化操作步骤，直接用程式码让它靠移动位置来达到跳动的效果，而这个跳动的动作，说穿了就是从原位置移动到一个指定高度的位置，再移动回来原来的位置，至于，要跳多高、移动速度多快，预先并不确定，所以，首先需要宣告两个可以在 Inspector 视窗设置的数值栏位，让我们可以在编辑器调整目标高度及跳动速度。

```csharp
[SerializeField]
private float hight = 1;
[SerializeField]
private float speed = 5;
```
另外，在跳动的行为进行中，如果马上又被要求跳动，就会跳到一半又往上跳，这样的行为是有问题的，所以必须要设置一个状态纪录，当动作进行中，不接受再次的跳动要求，等到动作完毕之后才能再次接受并执行要求的行为。


```csharp
private enum Status{

    None,
    Moving
}

private Status _status = Status.None;
```
因为我们这裡要做的跳动行为其实是两个移动行为的组合，所以要先写个可以提供物体本身从起点移动到终点的功能，两点间的移动，直接透过 Unity 内建的 Vector3.Lerp 就可以达成。

```csharp
private IEnumerator Move(Vector3 source , Vector3 target){

    float t = 0;

    while(t < 1){

        transform.position = Vector3.Lerp(source , target , t);
        t += Time.deltaTime * this.speed;

        yield return null;
    }

    transform.position = target;
}
```
Vector3.Lerp 的 t 是介于 0 到 1 的值，我们可以把它当作是起点到终点的进度位置来看待，0 是起点，1 是终点。因为实际执行时每次刷新画面的时间都不一样，所以，我们不能让 t 随著时间的推移增加固定的值，而应该要为我们预计增加的值（即是速度）乘上 Time.deltaTime，于是，我们在这裡透过 yield return null 让 while 迴圈每经过一个 frame 才执行一次，当 t 超过 1 的时候，表示已经到达终点，即可结束迴圈。

要完成移动还有最后的步骤，就是 Vector3.Lerp 最后一次执行时，不可能刚刚好 t = 1，所以，最后还要校正一下位置到正确的终点位置，如此，移动行为才算是真正完成。

在这裡要特别注意的是，这个 Method 所回传的是 IEnumerator，代表它是做为 Coroutine 来使用，所以才可以在其内部使用 yield 来控制一些流程和时间，而要呼叫这个 Method 执行则需要使用 StartCoroutine 来执行。

接下来要做跳的动作，就是跳动开始时，变更状态为移动中，然后，取得起点和终点的位置，先执行起点移动到终点，执行完之后，再执行终点移动到起点的行为，等待动作完成之后，跳动就结束了，所以，就可以再将状态改回 None 的状态。

```csharp
private IEnumerator DoJump(){

    this._status = Status.Moving;

    Vector3 source = transform.position;
    Vector3 target = source;
    target.y += this.hight;

    yield return StartCoroutine(this.Move(source , target));
    yield return StartCoroutine(this.Move(target , source));

    this._status = Status.None;
}
```
既然跳动行为已经写好，那麽就要提供一个让外部呼叫的功能，当被外部呼叫执行时，先判断有没有在跳动中，没有的话就执行跳动。

```csharp
public void Jump(){

    if(this._status == Status.None) StartCoroutine(this.DoJump());
}
```
如此，SphereJump 就算完成了，它并没有使用到 UnityEvent 事件，主要负责被呼叫执行动作而已，当然，视需求还是可以另外帮它补上基本事件，例如，开始跳动、跳动中、跳动结束。不过，这个示范中并没有用到，所以在此省略。

以下为 SphereJump.cs 的内容：

```csharp
using UnityEngine;
using System.Collections;

public class SphereJump : MonoBehaviour {

    private enum Status{

        None,
        Moving
    }

    [SerializeField]
    private float hight = 1;
    [SerializeField]
    private float speed = 5;

    private Status _status = Status.None;

    public void Jump(){

        if(this._status == Status.None) StartCoroutine(this.DoJump());
    }

    private IEnumerator Move(Vector3 source , Vector3 target){

        float t = 0;

        while(t < 1){

            transform.position = Vector3.Lerp(source , target , t);
            t += Time.deltaTime * this.speed;

            yield return null;
        }

        transform.position = target;
    }

    private IEnumerator DoJump(){

        this._status = Status.Moving;

        Vector3 source = transform.position;
        Vector3 target = source;
        target.y += this.hight;

        yield return StartCoroutine(this.Move(source , target));
        yield return StartCoroutine(this.Move(target , source));

        this._status = Status.None;
    }
}
```
在撰写 SphereDiscolor 之前，我们要先回到 PassEvents Script 档裡面，宣告一个能够用来传递颜色参数的 UnityEvent<T0>。

```csharp
[System.Serializable]
public class PassColor : UnityEvent<Color>{}
```

在 SphereDiscolor 中，先宣告用来暂存 Material 以及 Material 的颜色的两个变数栏位，然后，也宣告一个用来设置球体预设颜色的栏位，在 Awake 中取得球体本身的 Material 暂存下来，并使用所设置的预设颜色改变球体的颜色。

```csharp
private Material _material;
private Color _color;

[SerializeField]
private Color color = Color.white;

void Awake(){

    this._material = GetComponent<Renderer>().material;
    this.DefaultColor();
}

public void DefaultColor(){

    this._material.color = this.color;
    this._color = this.color;
}
```

同样的，改变为预设颜色的功能，也是独立做为可提供外部呼叫执行的功能。

然后，改变球体颜色的功能，我们分别宣告了直接改变为指定颜色的功能以及改变为随机颜色的功能，都是可提供给外部呼叫执行。

在此，也宣告了一个可以传递颜色值的 UnityEvent<T0> 事件栏位，当颜色被改变的时候，事件就会被执行，并且将所改变的颜色传递出去，所以，当球体颜色被改变时，可以让它引发其它行为，甚至是提供一个颜色去影响被引发的行为。


```csharp
[SerializeField]
private PassColor onChangeColor;

void Awake(){

    this._material = GetComponent<Renderer>().material;
    this.DefaultColor();
}

public void DefaultColor(){

    this._material.color = this.color;
    this._color = this.color;
}

public void Discolor(Color color){

    this._material.color = color;
    this._color = color;
    this.onChangeColor.Invoke(color);
}

public void RandomColor(){

    this.Discolor(new Color(Random.value , Random.value , Random.value));
}
```
到这裡，这个范例的程式撰写部分暂告一段落，回到 Unity 画面，为每个球体加入这三个 Script 做为 Component。

在这裡可能会有疑问，为什麽要分成三个 Script，而不写在同一个 Script 中呢？因为，Unity 的游戏物件所具备的功能是 Component 导向的，具备哪些 Component 就能拥有什麽样的功能，拔除哪个 Component，该游戏物件就不具备哪个功能，所以，我们将功能分开来独立不同的 Script 来撰写，每个 Script 只要提供本身的功能就好了，不要去牵扯到其他的功能，那麽，当球体拥有 SphereTouch Component 时，他就可被点击，没有的话就无法被点击，当球体拥有 SphereJump 时，它就具备跳动的功能，这样，我们就能明确的为游戏物件抽换并改变其能力。

所以，当每个球体都具备变色功能时，变色功能有个预设颜色栏位会将球体于 Play Mode 开始时变更为其所指定的初始颜色。

接著，当每个球体都具有被触发功能时，我们就能为其指定当球体被触发时要引发什麽行为，如影片所示，我们可以为每个球体指定被点击触发时，去要求它的下一颗球跳起来，要求它的前一颗球随机改变颜色。

![这里写图片描述](https://img-blog.csdn.net/20180704153436717?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
第 2 颗球设置被点击时，第三颗球跳起来，第一颗球随机改变颜色。

而最后一颗球被点击触发时，则是让前四颗球回到初始颜色。
![这里写图片描述](https://img-blog.csdn.net/20180704153443955?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
第五颗球被点击时，前四颗球改变成初始颜色。

然后，我们也可以为第二颗球设置当它颜色被改变时，影响其他颗球的颜色。
![这里写图片描述](https://img-blog.csdn.net/20180704153503564?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
第二颗球变色的时候，让第一颗球和第五颗球改变为随机的颜色。

在此，我们再次体验到，单纯写好 Script 提供的功能，不用在程式码中指定它人的行为，而能在编辑器中自由变更的好处以及功能弹性。

虽然，UnityEvent 可以直接在 Inspector 视窗指定一个固定值传递给某个 Component 的功能并执行它，也可以透过程式码使用 Invoke 呼叫执行并传递参数进去，但是，它却无法像我们平常呼叫执行一般的 Method 那样回传资料，似乎有点美中不足之感。

接下来，我们就来讨论如何也让 UnityEvent 带回资料，其实，这主要就是利用参考型别物件在参数间传送并不是传送实值的原理，来达到带回资料的目的。也就是说，我们可以透过实例化一个参考型别物件，传入 UnityEvent 的参数中，当这个物件内含的资料在 UnityEvent 所执行的功能中有任何变更后，在呼叫执行 UnityEvent 事件的这一方也可以从原本的物件获得改变后的资料。

只是，为了带回不同的型别的资料而宣告许多不同的 Class，似乎太麻烦了，因此，我们最好是可以製作一个通用的 Class，专门用来做为传递资料的持有者物件。所以，可以建立了一个名为 PassHolder 的 C# Script，专门用来做这件事。

```csharp
public class PassHolder {

    public object value{set; private get;}

    public T GetValue<T>(){

        if(this.value == null) return default(T);

        return (T)this.value;
    }
}
```
因为，所有的型别都是直接或间接继承自 Object（这裡所指的并不是 Unity 的 Object），所以，宣告一个 Property 统一接收任何型别的物件，然后，利用泛型方法的宣告方式，来取出所储存的资料，这裡只是很简单的判断有没有资料，如果没有资料则传回预计要取得的型别预设值。如此，这个 class 的物件就能通用存取任何型别的资料。

有了这样的 class，那麽就可以拿 SphereDiscolor 来实验看看；先来为 SphereDiscolor 加入交换颜色的功能。当然，要宣告能够传递 PassHolder 的 UnityEvent 事件栏位之前，还是要先回到 PassEvents 的 Script 档中，加入宣告可传递颜色以及 PassHolder 两个参数的型别。

```csharp
[System.Serializable]
public class PassColorReturn : UnityEvent<Color , PassHolder>{}
```
这个功能要做的是，当它被呼叫执行时会透过交换颜色事件把自己的颜色传送出去，并透过 PassHolder 带回对方的颜色来改变自己的颜色。

而我们目前的范例裡，能被改变颜色的就属 SphereDiscolor 了，所以，再帮它加入另一个改变颜色的功能是除了接受目标颜色之外，还要能接收 PassHolder 物件，因此，除了拿接收到的颜色为自己变色之外，也要把原本的颜色写入到 PassHolder 中，那么呼叫执行这个变色功能的彼方，也就能收到所要带回去的颜色值。

```csharp
[SerializeField]
private PassColorReturn onSwapColor;

public void SwapColor(){

    PassHolder holder = new PassHolder();
    this.onSwapColor.Invoke(this._color , holder);
    this.Discolor(holder.GetValue<Color>());
}

public void Discolor(Color color , PassHolder holder){

    holder.value = this._color;
    this.Discolor(color);
}
```
完成之后，我们在 Unity 编辑器的 Inspector 视窗中，就可以直接在 On Swap Color 的事件栏位指定要跟哪颗球交换颜色。
![这里写图片描述](https://img-blog.csdn.net/20180704153559605?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
当第四颗球被点击时，使第五颗球跳起来、第三颗球改变颜色、要求自己执行交换颜色并指明与第一颗球交换颜色。

如此，几支程式、简短的程式码，只是定义它们本身的功能，以及什麽时候呼叫该执行的事件，而不需要特别指定所影响的型别及功能是什麽，而能在 Inspector 视窗中，很弹性的为具备同样 Component 的 GameObject 配置出完全不同的行为。也不会让程式中因为型别不对或参数数量不符合而出现错误，使得程式执行及设计上变得更弹性、更稳定，也让程式码内容变得更简洁，逻辑更清晰。在维护上以及流程调整上也会变得更视觉化、更清楚一些。

过去曾经发佈「Unity：使用 UGUI 的 ScrollRect 製作虚拟摇杆」的文章与影片，其中为虚拟摇杆传递操作行为的事件就是 UnityEngine.Events 的应用，善用这些做法，所写的程式复用性以及扩充性将会大大提高。

好了，有关 UnityEngine.Events 的说明及示范解说，到此结束，如果，你喜欢这个文章或所展示的影片，请帮忙介绍给你的朋友，也别忘了订阅影片频道以及来粉丝专页按个讚，谢谢！

UnityEvent官方文档：

http://docs.unity3d.com/ScriptReference/Events.UnityEvent.html

目前版本 Unity 5.2.1f1
