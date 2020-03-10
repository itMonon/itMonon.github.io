---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《扫雷》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
这是一款经典的小游戏，是Win系统自带的小游戏，是一款大众类的益智小游戏，于1992年发行。游戏目标是在最短的时间内根据点击格子出现的数字找出所有非雷格子，同时避免踩雷，踩到一个雷即全盘皆输。

## 二、GitHub地址

GitHub地址：https://github.com/764424567/Game_MineSweeping
*注意：可以直接在GitHub仓库克隆或者下载源代码

## 三、正文
Unity 2D 扫雷游戏教程
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2UTA3Q1ZqdEFpYklpYUNlS3JNNHY2TDB4c3RFRlhXS2VpYVBUZHo2NG1JUE1DSEJkTWRNeGtuS0FBLzA)
**前言**

欢迎来到Unity2D扫雷教程。扫雷是单机益智游戏，最初发表在1960年代。游戏的玩法是，在不触碰到任何地雷的情况下去发现一个雷区。揭开一个没有地雷的块之后，将会显示一个数字来表示周围地雷的数量。增加了一个不错的策略性方面到游戏中去。

这听起来很简单也非常有趣，不同版本的扫雷时常包含在一些主要的操作系统中。

本教程中的扫雷将会做得很简单，只有85行代码和一些像素。我们将会学到不少关于Unity编程的东西，并且实现流行的填充游戏算法（Full Fill）。

如平常一样，尽可能简单的解释，并且让大家都能理解。

**相关准备**

**预备知识**

阅读本篇教程不需要太多专业知识，但仍需要了解Unity基础的GameObject和Transform等概念。了解递归知识（一个函数调用自己本身）对于填充算法将会有很大帮助。

**Unity版本**

本篇教程适用于Unity5.0.0及以上所有版本，5.0以下的版本可能会出现一些问题。Unity5是免费的且包含所有功能，推荐大家尽可能的使用该版本。

**项目设置**

首先新建一个项目，打开Unity并选择New Project：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2cXpXR3RzNlpYSzBvbzJPR3JvU1ZqYjBBVkJmZW1UT0RwdVpoNlV1Mklrd1VTeG5pYVhtYnNIUS8w)
我们将该项目命名为minesweeper，选取项目存储的目录如C:\，项目类型选择2D，然后点击Create Project：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2OUh0SU5kU0RoalhMRTZuaWJQTm5qcWswRVowYURsaWJ3MHFFVjhmcUhBaGlhYzRpY1ZJVnY3R25hZy8w)
在层次面板（Hierarchy）中选中Main Camera，将背景色（Background Color）设置为黑色。并如下图所示调整大小和位置：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2T3JyVEE2U0cwNTlHNGljRTFtb0EzSlI0YmxpY1FSOE05NjNOcWlhTXNlaWNvVWVVSzFneTlsbE5LQS8w)
**默认方块**

让我们为游戏添加默认方块吧，默认方块就是那些我们没点击之前看到的白色方块。他们的作用就是隐含地雷或数字。

首先我们需要一些方块图片来供我们使用，我们将用画图工具，制作一个16x16像素的图片，如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2U28zVTZKanNOWkY5a2hjb3JrSlpoNTdGOEpxVjMzeXdZcnBlTlJFZ1dEemdRd0F4SmdacU9RLzA)

**注意**：你可以右击在图片上，然后将上面的图片保存在项目的Asset文件夹下。
保存到Asset文件夹后，我们就可以在Project Area里找到这个图片，如下图：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2V1lUb2dtaWFHTmNZNGJOS0ppYjlXeTc1Y2N2UlZuczdnck44YjltN1RZdFFtbTBpYjRUT2QxUUp3LzA)
然后我们便能在Inspector修改导入设置，如下图：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2eDVEVVNKeWFXR1VOSnRUUXdhNUdsS3Q0RjV6MFNmbDAxVk5mekk5ekpZOGJ0RHhtY3hKdHpBLzA)
**注意**：导入设置将指定该图片在最终游戏里的大小，是否使用压缩。

好了，现在我们可以拖动在Project Area里的方块图片到场景中去，如下图：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2bTFXd0NFSGpHNXl2Y0N0d0k2VzA3OTRCODNzeHBRcE1RV2RGdTZhc3ZNMzlLYmNkT3p5N1VRLzA)
**注意**：所有在Project Area里的东西只是一个文件，有些东西我们可能会用到也可能不用到。我们再一次拖动这个方块到场景中去，它便变成了游戏世界的一部分哟。

接下来在Hierarchy 面板把dafault（方块）的position设置为：X=0，Y=0，Z=0。并如下所示来设置 ：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2OW1qZ2UzNjRjalRzRG15bjVacG9KdXd3Zzk1UXlKSXlTbjU5OUs2YTRyTUlEUmFPcEZCaWNpYmcvMA)
**注意**：X轴表示横坐标，Y轴表示垂直坐标，我们将Z轴设置为0，因为我们要做的是一个2D游戏，因此这里不需要第三维坐标（Z）。

当用户点击一个方块的时候，我们要获得点击信息。Unity提供一个函数去获得它，只有方块上有 Colliders组件才能触发该函数。



Colliders是物理对象的一部分。现在，我们的方块在游戏世界里就是一张图片，我们为它添加一个Colliders组件，该方块将成为物理世界的一部分咯，就像一堵墙。

我们可以在Inspector里通过 Add Component->Physics 2D->Box Collider 2D选择，为该方块添加Colliders组件：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2dVNYcnNxM3l6cjN2b2w1SWdKRnlMb0JMQTV1Q0lBUGliU0xJd3A2d2dhd2liMUlFczh5Z3RLRlEvMA)
就这样，它成为是物理世界的一部分 = =。

如果我们按下Play按钮，我们将可以在场景中看到这个方块：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2WWtjVXNYQVFHQjJ3UERCQVQ1T2RCMnNxUzVtVW9CM0dwaWE0dXBySmxNd1J5WnhpY1NFaEFWcGcvMA)
添加更多的方块

我们的2D扫雷游戏只有一个方块显得非常的无聊，但是我们可以通过上面的操作为游戏添加更多的方块，让它不再是个单身狗= =。或者在Hierarchy面板上右击这个default（方块）游戏对象，并且选择Duplicate：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2b1NJRHRWRkRIQ1ExSWxBb082cm9iZnF3bXFuWjdXS3BxRjExUnVhaWFBdEZncVpWSDZ2T0lPQS8w)
我们设置这个复制出来的方块的Position：X=1，Y=0：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2NDZkVmljeXRuT2hBY2JZeWlhQnF6TnZqWmE2a1hzdGZOT0Z3SXcwNWtjRE1qanRmNTFCT2JJNGcvMA)
现在我们可重复复制方块并且重复上面的操作，然后将他们排列成10行13列：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXovTEoyRktPU2g0OEd2REdFTmZBYUVoTE5MMXoxUElpYzJ2TWlhd2liY2xQcVA5aHNhUno5R3hsWVhHN0ZiVWlhQjhOSzdkMnZ1SnRUY1JJeGZGRVA0RVdnYlBRLzA)
注意：最底部的左边为：X=0，Y=0，最右边为：X=9，Y=12。他们的值总是整数的，就像X=2，Y=3，而不是X=2.003，Y=3.02。

天啊~！！终于像点样子了。

经典游戏扫雷的基本界面就这样创造好了


**关于邻接**

让我们花一分钟时间来了解邻近的矿场，这将是我们扫雷游戏的一个重要部分。

注：邻接是一个花哨的词，或直接说是邻居.

单击一个非地雷的元素后，用户应该看到一个指示相邻地雷数量的数字。我们将使用所谓的这里有8个邻近。或者换句话说，不要只看顶/底部/左边/右（边），正确的我们还将查看左上角/右上角/左下角/右下角元素。

以下是我们可以遇到的9种不同情况：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2FkamFjZW5jeTAucG5n) ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200114113511878.png)![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2FkamFjZW5jeTIucG5n)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2FkamFjZW5jeTMucG5n) ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2FkamFjZW5jeTQucG5n) ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2FkamFjZW5jeTUucG5n)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2FkamFjZW5jeTYucG5n) ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2FkamFjZW5jeTcucG5n) ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200114113535197.png)
因此，我们所要做的就是计算每个字段的相邻地雷数量，然后得出数字，如果没有相邻的地雷，则什么也不画。

**添加更多图像**

好的，为了绘制这些数字，我们可以使用联合的GUI系统，或者我们只是不太担心，然后快速地为每个数字绘制一个纹理：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200114113251749.jpg) ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200114113259515.png) ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2VtcHR5Mi5wbmc) ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2VtcHR5My5wbmc) ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2VtcHR5NC5wbmc) ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2VtcHR5NS5wbmc) ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2VtcHR5Ni5wbmc) ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2VtcHR5Ny5wbmc) ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200114113218496.png)
注意：右击每个图像，选择另存为.。把它们全部保存在项目中资产文件夹。

我们还需要一个地雷的图像：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL21pbmUucG5n)
注意：右击图像，选择另存为.。把它们全部保存在项目中资产文件夹。

将这些图片导入到项目中之后，设置一下属性：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200114113347169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
**代码：**
在项目面板中，右键空白处Create->C# Script->Element
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2NyZWF0ZV9zY3JpcHQucG5n)
将这个脚本添加对象上面：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2VsZW1lbnRfc2NyaXB0X2luc3BlY3Rvci5wbmc)
编写脚本Element.cs

```csharp
using UnityEngine;
using System.Collections;

public class Element : MonoBehaviour {

    // Use this for initialization
    void Start () {

    }

    // Update is called once per frame
    void Update () {

    }
}
```
我们可以移除更新因为我们不需要它。我们还添加了一个变量，该变量指示该元素是否是地雷：


```csharp
using UnityEngine;
using System.Collections;

public class Element : MonoBehaviour {

    // Is this a mine?
    public bool mine;

    // Use this for initialization
    void Start () {

    }
}
```

注：地雷变量是public这样其他元素就能看到它。这个启动函数在游戏开始时调用一次。
现在我们可以在Start函数中 ，使用Random.value对mine进行赋值：

```csharp
using UnityEngine;
using System.Collections;

public class Element : MonoBehaviour {

    // Is this a mine?
    public bool mine;

    // Use this for initialization
    void Start () {
        // Randomly decide if it's a mine or not
        mine = Random.value < 0.15;
    }
}
```
注：Random.value将始终返回一个新的随机数0和1..我们想要一个15%元素是地雷的概率，所以我们使用Random.value<0.15.
让我们创建一个辅助函数。我们希望能够在任何时候从默认纹理切换到空纹理、数字纹理或地雷纹理。首先，我们将定义几个纹理变量：

```csharp
using UnityEngine;
using System.Collections;

public class Element : MonoBehaviour {

    // Is this a mine?
    public bool mine;

    // Different Textures
    public Sprite[] emptyTextures;
    public Sprite mineTexture;

    // Use this for initialization
    void Start () {
        // Randomly decide if it's a mine or not
        mine = Random.value < 0.15;
    }
}
```
注：Sprite是纹理的另一个词。Sprite[]是一个数组，或者换句话说：多个Sprite。
现在我们可以看到一些新的插槽:
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2VsZW1lbnRfc2NyaXB0X3RleHR1cmVzbG90cy5wbmc)
这就是我们可以拖动纹理的地方。因此，让我们在项目面板然后把它们拖到插槽里：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2VsZW1lbnRfc2NyaXB0X3RleHR1cmVzbG90c19maWxsZWQucG5n)
现在我们可以通过loadTexture函数创建sprite：

```csharp
using UnityEngine;
using System.Collections;

public class Element : MonoBehaviour {

    // Is this a mine?
    public bool mine;

    // Different Textures
    public Sprite[] emptyTextures;
    public Sprite mineTexture;

    // Use this for initialization
    void Start () {
        // Randomly decide if it's a mine or not
        mine = Random.value < 0.15;
    }

    // Load another texture
    public void loadTexture(int adjacentCount) {
        if (mine)
            GetComponent<SpriteRenderer>().sprite = mineTexture;
        else
            GetComponent<SpriteRenderer>().sprite = emptyTextures[adjacentCount];
    }
}
```
注意：函数首先检查元素是否是地雷。如果它是一个地雷，那么它加载地雷的纹理。如果它不是地雷，它就会装载一个空的Texture，取决于邻接计数..这个GetComponent<SpriteRenderer>().sprite问题是我们如何改变当前的纹理。

我们可以通过更改Start函数的作用时间为：

```csharp
// Use this for initialization
void Start () {
    // Randomly decide if it's a mine or not
    //mine = Random.value < 0.15;

    // TEST
    loadTexture(1);
}
```
如果我们按下Play然后我们可以看到每个元素是如何加载数字1纹理的：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL2VsZW1lbnRfc2NyaXB0X2xvYWR0ZXh0dXJlX3Rlc3QucG5n)
我们现在可以将它更改回原来的开始功能：

```csharp
// Use this for initialization
void Start () {
    // Randomly decide if it's a mine or not
    mine = Random.value < 0.15;
}
```
稍后，我们需要知道一个元素是否还被覆盖。(如：尚未点击)或者不是，所以让我们添加一个简单地将当前纹理的名称与默认名称进行比较的函数：

```csharp
// Is it still covered?
public bool isCovered() {
    return GetComponent<SpriteRenderer>().sprite.texture.name == "default";
}
```
注意：只要元素具有默认的纹理，它就会被覆盖。一旦我们加载一个不同的纹理，如地雷或一个数字，它将不再被覆盖。

我们将向我们的元素脚本，这样我们就可以检测到鼠标的点击。我们的每个元素已经有一个Collider2D附加到它，这意味着每当我们单击一个元素时，函数OnMouseUpAsButton将被统一召唤。当然，只有在脚本中有一个具有该名称的函数时，才会发生这种情况，因此，让我们添加一个：

```csharp
void OnMouseUpAsButton() {
    // ToDo: do stuff..
}
```
在单击元素后，有两种情况可以发生。要么是地雷，要么不是地雷：

```csharp
void OnMouseUpAsButton() {
    // It's a mine
    if (mine) {
        // ToDo: do stuff..
    }
    // It's not a mine
    else {
        // ToDo: do stuff..
    }
}
```
如果它是一个地雷，那么所有其他的地雷都应该被发现。(我们将尽快实施)游戏结束了：

```csharp
void OnMouseUpAsButton() {
    // It's a mine
    if (mine) {
        // ToDo: uncover all mines
        // ...

        // game over
        print("you lose");
    }
    // It's not a mine
    else {
        // ToDo: do stuff..
    }
}
```
如果不是一个地雷，然后几件事就会发生。首先，我们应该根据相邻矿场的数量，用正确的数字加载空纹理。(我们也会尽快实施)..如果点击了一个没有相邻地雷的元素，那么我们就应该发现整个没有地雷的元素区域，如下图所示：.
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL3VuY292ZXJfbWluZWxlc3NfZWxlbWVudHMuZ2lm)
我们还应该找出除地雷以外的所有因素是否都被发现，在这种情况下，游戏就赢了。下面是第一个版本，还有一些东西还没有添加：

```csharp
void OnMouseUpAsButton() {
        // It's a mine
        if (mine) {
            // ToDo: uncover all mines
            // ...

            // game over
            print("you lose");
        }
        // It's not a mine
        else {
            // ToDo show adjacent mine number
            //loadTexture(...);

            // ToDo uncover area without mines
            // ...

            // ToDo find out if the game was won now
            // ...
        }
    }
```

我们所有的东渡特性有一个共同点：它们不仅需要元素本身的信息，还需要其他元素的信息。所以让我们再创建一个脚本来处理全部元素。

**Grid**

**创建类**
网格将成为我们的辅助类，它知道全这些元素可以处理更复杂的游戏逻辑，比如为某个元素计算相邻的地雷，或者发现整个区域的无雷元素。

我们将首先创建一个新的C#脚本并给它命名Grid:

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
此脚本不必是可以附加到GameObject的脚本类型，因此让我们移除Start和Update函数：

```csharp
using UnityEngine;
using System.Collections;

public class Grid {

}
```
**元素二维阵列**
我们的网格应该跟踪游戏中的所有元素。我们可以用二维阵列(亦称矩阵或表)这样做：

```csharp
using UnityEngine;
using System.Collections;

public class Grid {
    // The Grid itself
    public static int w = 10; // this is the width
    public static int h = 13; // this is the height
    public static Element[,] elements = new Element[w, h];
}
```

注意：这将创建一个新的二维数组，宽10而13高，换言之：10*13要素..如果我们想访问X=0, Y=1我们会写要素[0，1].

**在网格中注册**
让我们切换回我们的元素脚本非常快，并修改启动函数，以便每个元素在格网自动：

```csharp
// Use this for initialization
void Start () {
    // Randomly decide if it's a mine or not
    mine = Random.value < 0.15;

    // Register in Grid
    int x = (int)transform.position.x;
    int y = (int)transform.position.y;
    Grid.elements[x, y] = this;
}
```
注意：变换位置的x和y坐标类型是float类型，我们需要强制转化为int类型


**发现所有地雷**

好吧，让我们回到我们的格网类并实现发现所有地雷的函数。这将非常容易，因为我们所要做的就是遍历每一个元素，找出它是否是一个地雷，然后加载纹理：

```csharp
// Uncover all Mines
public static void uncoverMines() {
    foreach (Element elem in elements)
        if (elem.mine)
            elem.loadTexture(0);
}
```


注意：我们需要检查每个在地雷创建之间的元素变量，然后使用LoadTexture。如果它本身是一个地雷，我们只需要加载编号为0的Texture，因为函数是public static的，所以可以在任何地方使用它，并不仅仅在Grid类内部。


让我们跳回我们的Element.cs脚本并修改OnMouseUpAsButton函数，因此它使用我们最近创建的揭开地雷函数，以防用户单击地雷：

```csharp
void OnMouseUpAsButton() {
    // It's a mine
    if (mine) {
        // Uncover all mines
        Grid.uncoverMines();

        // game over
        print("you lose");
    }
    // It's not a mine
    else {
        // ToDo show adjacent mine number
        //loadTexture(...);

        // ToDo uncover area without mines
        // ...

        // ToDo find out if the game was won now
        // ...
    }
}
```
如果我们按下Play点击几个元素，直到我们发现一个地雷，然后我们就可以看到所有其他的地雷也被发现了：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL3VuY292ZXJtaW5lc19pbmdhbWUucG5n)
计数邻近地雷
接下来，我们将向我们的Grid类。给定位置上的元素x，y，这个函数将计算邻近地雷的数量。这听起来有点复杂，但函数最后只查看了以下8个周围的元素：
- 顶
- 右上角
- 右（边），正确的
- 右下角
- 底部
- 左下角
- 左边
- 左上角
当其中一个元素是地雷时，增加一个计数器。

因此，首先，我们将向我们的Grid类。此函数只需检查某一位置是否有地雷：

```csharp
// Find out if a mine is at the coordinates
public static bool mineAt(int x, int y) {
    // Coordinates in range? Then check for mine.
    if (x >= 0 && y >= 0 && x < w && y < h)
        return elements[x, y].mine;
    return false;
}
```
注意：我们必须检查坐标是否在elements数组，以防止访问以下内容要素[-1，-1]这会导致一个错误。
现在我们可以创建实际的邻接地雷函数的x和y作为参数的坐标和计数器将退还：

```csharp
// Count adjacent mines for an element
public static int adjacentMines(int x, int y) {
    int count = 0;

    // ToDo count adjacent mines
    // ...

    return count;
}
```
之后，我们将检查所有这些相邻元素：

```csharp
// Count adjacent mines for an element
public static int adjacentMines(int x, int y) {
    int count = 0;

    if (mineAt(x,   y+1)) ++count; // top
    if (mineAt(x+1, y+1)) ++count; // top-right
    if (mineAt(x+1, y  )) ++count; // right
    if (mineAt(x+1, y-1)) ++count; // bottom-right
    if (mineAt(x,   y-1)) ++count; // bottom
    if (mineAt(x-1, y-1)) ++count; // bottom-left
    if (mineAt(x-1, y  )) ++count; // left
    if (mineAt(x-1, y+1)) ++count; // top-left

    return count;
}
```
让我们跳回我们的元素脚本并修改OnMouseUpAsButton再次激活函数：

```csharp
void OnMouseUpAsButton() {
    // It's a mine
    if (mine) {
        // uncover all mines
        Grid.uncoverMines();

        // game over
        print("you lose");
    }
    // It's not a mine
    else {
        // show adjacent mine number
        int x = (int)transform.position.x;
        int y = (int)transform.position.y;
        loadTexture(Grid.adjacentMines(x, y));

        // ToDo uncover area without mines
        // ...

        // ToDo find out if the game was won now
        // ...
    }
}
```
如果我们按下Play然后，在发现一个元素之后，我们可以看到相邻的矿号：

[外链图片转存失败(img-502zsoDn-1562837166258)(https://noobtuts.com/content/unity/2d-minesweeper-game/adjacentminenumber_ingame.png)]
**发现一个区域**
好的，每当用户发现一个没有相邻地雷的元素时，就应该自动发现没有相邻地雷的整个区域，如下所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL3VuY292ZXJfbWluZWxlc3NfZWxlbWVudHMuZ2lm)
有许多算法可以做到这一点，但到目前为止最简单的算法是泛洪算法算法[(请点击该链接以获得包含图片和所有内容的精彩解释)](https://en.wikipedia.org/wiki/Flood_fill).如果我们理解递归，洪水填充是非常简单的。简而言之，以下是泛洪算法所做的工作：

- 从某种元素开始
- 用这个元素做我们想做的事
- 对每个相邻元素递归地继续

我们首先将默认的泛洪算法算法添加到Grid类中：

```csharp
// Flood Fill empty elements
public static void FFuncover(int x, int y, bool[,] visited) {
    // visited already?
    if (visited[x, y])
        return;

    // set visited flag
    visited[x, y] = true;

    // recursion
    FFuncover(x-1, y, visited);
    FFuncover(x+1, y, visited);
    FFuncover(x, y-1, visited);
    FFuncover(x, y+1, visited);
}
```
注：参观变量是一个2D数组，它简单地跟踪算法是否已经访问了某个元素。剩下的只是默认的Flood填充4邻居递归。或者换句话说：算法从某个元素开始，然后递归地继续在元素的顶部、右边、底部和左边的元素，直到它访问每个元素为止。它什么都不做真品工作的时候，它只访问过一次所有的东西。
我们还应该确保我们的算法不会尝试访问网格之外的任何元素，方法是检查x和y坐标在0而宽度或高度:

```csharp
// Flood Fill empty elements
public static void FFuncover(int x, int y, bool[,] visited) {
    // Coordinates in Range?
    if (x >= 0 && y >= 0 && x < w && y < h) {
        // visited already?
        if (visited[x, y])
            return;

        // set visited flag
        visited[x, y] = true;

        // recursion
        FFuncover(x-1, y, visited);
        FFuncover(x+1, y, visited);
        FFuncover(x, y-1, visited);
        FFuncover(x, y+1, visited);
    }
}
```
我们的算法应该发现它访问的每个元素。当一个元素靠近一个地雷时，它就不应该继续下去：

```csharp
// Flood Fill empty elements
public static void FFuncover(int x, int y, bool[,] visited) {
    // Coordinates in Range?
    if (x >= 0 && y >= 0 && x < w && y < h) {
        // visited already?
        if (visited[x, y])
            return;

        // uncover element
        elements[x, y].loadTexture(adjacentMines(x, y));

        // close to a mine? then no more work needed here
        if (adjacentMines(x, y) > 0)
            return;

        // set visited flag
        visited[x, y] = true;

        // recursion
        FFuncover(x-1, y, visited);
        FFuncover(x+1, y, visited);
        FFuncover(x, y-1, visited);
        FFuncover(x, y+1, visited);
    }
}
```
这就是实现和修改Flood的简单方法，在C#中填写。

现在我们可以回到我们的元素编写脚本并使用该算法在用户单击其中一个元素时发现所有空元素：

```csharp
void OnMouseUpAsButton() {
    // It's a mine
    if (mine) {
        // uncover all mines
        Grid.uncoverMines();

        // game over
        print("you lose");
    }
    // It's not a mine
    else {
        // show adjacent mine number
        int x = (int)transform.position.x;
        int y = (int)transform.position.y;
        loadTexture(Grid.adjacentMines(x, y));

        // uncover area without mines
        Grid.FFuncover(x, y, new bool[Grid.w, Grid.h]);

        // ToDo find out if the game was won now
        // ...
    }
}
```
注意：我们只是在当前元素的位置调用了一个新的具有网格大小的布尔数组的算法。布尔数组用于泛洪算法，以跟踪它已经访问过的元素。

如果我们按下Play发现一个空元素(没有邻近的地雷)然后我们可以看到泛洪算法在工作中：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9ub29idHV0cy5jb20vY29udGVudC91bml0eS8yZC1taW5lc3dlZXBlci1nYW1lL3VuY292ZXJfbWluZWxlc3NfZWxlbWVudHMuZ2lm)
**检查是否发现所有地雷**
还有最后一件事要做，我们仍然要找出游戏是否在用户发现一个元素后获胜。这个算法将再次变得非常容易。

让我们回到我们的Grid类并编写代码，以查明是否所有尚未发现的元素都是地雷：

```csharp
public static bool isFinished() {
    // Try to find a covered element that is no mine
    foreach (Element elem in elements)
        if (elem.isCovered() && !elem.mine)
            return false;
    // There are none => all are mines => game won.
    return true;
}
```
注意：算法只是试图找到一个仍被覆盖的元素，而不是一个地雷。如果找到这样的元素，则返回假的因为用户还有工作要做。如果没有找到这样的元素，则返回千真万确因为游戏赢了(因为所有覆盖的元素都有地雷)。
现在我们可以用我们的isfined函数中的Element脚本：

```csharp
void OnMouseUpAsButton() {
    // It's a mine
    if (mine) {
        // uncover all mines
        Grid.uncoverMines();

        // game over
        print("you lose");
    }
    // It's not a mine
    else {
        // show adjacent mine number
        int x = (int)transform.position.x;
        int y = (int)transform.position.y;
        loadTexture(Grid.adjacentMines(x, y));

        // uncover area without mines
        Grid.FFuncover(x, y, new bool[Grid.w, Grid.h]);

        // find out if the game was won now
        if (Grid.isFinished())
            print("you win");
    }
}
```
如果我们按下Play然后我们就可以享受这个游戏了：

**摘要**
这是我们的Unity2D扫雷教程。这次我们学到了很多关于UnityandC#编程的知识。理解洪水填充并能够在任何编程语言中实现它是每个开发人员工具箱中非常有用的资产。

和往常一样，现在该由读者来让游戏更有趣了。还有很多改进可以做，比如用标志标记元素，添加更大的级别，更漂亮的图形，一些不错的声音，输赢屏幕或者重新启动按钮。
