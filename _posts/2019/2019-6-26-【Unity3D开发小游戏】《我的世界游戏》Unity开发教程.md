---
layout: post
category: Unity3D-Game
title: 【Unity3D开发小游戏】《我的世界游戏》Unity开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
这套教程涵盖了Unity Mesh编程、模拟水算法（water simulations）、方块移动算法（marching-cubes）等等。这是一套比较有深度的教程，可能需要你了解一些Unity和C#相关的知识。


## 二、效果图
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhINmpmczYxdTRiMEhPQ2NqMWliSWJBVGxmbTVpYXBwMnJvVkFJaWF6NmpDUFdralJKMk1NdTFTcVhZWGF5bmRwU0drR2VWM0xOZzBMcTNnLzA?x-oss-process=image/format,png)


## 三、正文
### 一、基础篇：生成数据块
预备开始

  首先，我们先来创建一个空的项目，命名随意即可。

   然后创建在Assets下创建一个文件夹，命名为“Scripts”，并创建三个C#脚本，如下图所示：
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153207760.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
   Chunk用于存储方块数据和创建网格，并且对网格进行渲染和碰撞；Block用于存放方块需要的信息；MeshData用于存储网格数据。

Chunk.cs脚本：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153222366.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
                首先，我们要求该脚本必须包含上个组件：MeshFilter、MeshRenderer、MeshCollider。我们的数据块（就是由一堆方块组成的大方块~）需要这三个组件完成网格的创建和碰撞。

   然后，我们有三个变量。我们有一个Block类型的三维数组变量 blocks，Block类用于存放方块需要的信息，因此我们这个blocks变量就是用于存放一个数据块的方块的信息。

   chunkSize是一个静态变量，它用于表示我们的数据块各个方向的大小（就是长宽高的大小）。

   最后我们有一个bool类型的变量，用于标志该数据块是否在每帧结束后更新。

   其次有三个函数，分别为GetBlock、UpdateChunk、RenderMesh。GetBlock用于获取对应位置的方块；UpdateChunk用于更新数据块的网格数据，然后将更新的数据提供给RenderMesh去渲染。

MeshData.cs脚本：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153233658.png)
 由于这个脚本只是为了存储数据，因此不必继承自MonoBehaviour。        

   前三个变量（vertices、triangles、uv）是用于渲染网格用的，后两个用于网格碰撞（colVetices、colTriangles）。

Block.cs脚本：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153243566.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
同样的，Block脚本也不需要继承自MonoBehaviour，并且它将会是所有方块的基类。主要用于存储方块所需信息。

BlockData函数用于生成该方块的网格信息。

我们来设想一下，假如我们的数据块是有25个方块组成的，那么在相邻的方块之间，有一些面就不必渲染出来，浪费系统资源。因此，接下来让我们进行剔除多余面数的处理。

首先，我们需要一个函数去判断两个方块是否相邻，如果相邻则对各个方向的面进行剔除处理。

在那之前，让我们先定一个表示方向的枚举（Block脚本中）：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153254343.png)
  然后让我们为Block脚本添加判断的函数：
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153304388.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
   因为Block脚本是所有方块类的基类，所有我们在Block脚本中对IsSolid函数没有进行判断处理，全部返回true。

   现在让我们开始写一些我们的BlockData函数，在这个函数中，我们会根据当前方块对应方向上相邻方块的面进行剔除处理。   
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153334765.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)



上图中的注释也说得很清楚了。举个栗子：判断当前方块顶上相邻的方块是否有底面，如果有则当前方块就不制作顶面，如下图分析所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153351625.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
接下来就是添加需要绘制对应的面的函数了。

上：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153402322.png)
上面的注释也说得很清楚了，但是克森还是给你们秀一秀我的美术功底。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153421226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
其它的面就不细讲，代码如下：

下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153430696.png)
东：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153445459.png)
北：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153454617.png)
南：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153503604.png)
西：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153512593.png)
添加点之后，我们还要把这些点组合成三角形，因此在函数的最后调用了MeshData里的AddQuadTriangles()，由名字可知道，该函数用于添加面片，因此我们要用这四个顶点组合成一个面片，让我们回到MeshData中添加该函数：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153521176.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
再给大家上一次克森的美术作品，相信大家都能理解了吧。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153529887.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
接下来，让我们创建一个新的脚本，命名为“BlockAir”，让它继承值Block类，如下所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/201909041535406.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
Okey，现在让我们回到Chunk脚本中，开始添加方块进行测试咯。添加如下代码：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019090415355099.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
首先声明两个变量，一个为MeshFilter（网格过滤器）类型，另一个为MeshCollider（网格碰撞器）类型。分别用于存储和设置我们对应数据库上的组件属性。

首先通过GetComponent方法获取对应物体上的对应的组件，然后初始化了我们的数据块（blocks），我们的数据块是一个16*16*16大小的正方体，里面由一堆小方块组成。当前数据库的小方块类型为 BlockAir。

然后修改了该数据库blocks[3, 5, 2]的方块数据，修改为Block类型方块。

最后调用UpdateChunk函数进行数据的更新。

好的，接下来让我们完善我们的UpdateChunk函数：

首先声明一个类型为MeshData的变量。然后循环遍历blocks进行数据的更新（就是调用每一个方块的BlockData函数，而BlockData函数则是用来处理方块的网格数据，例如剔除面等等）。

最后就是调用RenderMesh函数将更新好的网格数据传入，然后进行网格的渲染。

那么，接下来让我们完成我们的RenderMesh函数：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153618696.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
这个函数很简单，就是先调用Clear函数清除上一次网格的数据，然后重新设置即可。

这一篇只是简单的介绍怎么生成数据块，还没涉及到贴图和碰撞，所以在RenderMesh函数里只是更新了网格数据。下一篇则教大家如何添加贴图到数据块上。

说那么多，先看看效果。

首先创建一个空物体，然后为该物体添加Chunk脚本。你将会发现如下效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019090415362776.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153635736.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
为什么方块会跑那去了，为什么会是紫色的呢？因为该方块没有材质，所以是紫色的。因为我们设置了该方块的位置为（3，5，2），那我们是在哪里设置的呢，其实是在这个地方设置了，如下图所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153644112.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
这个时候你可以创建一个Cube物体去比对一下就知道了，下图演示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019090415365337.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
看来是这样的没错。好，接下来克森带大家来走一走这个运行时候的步骤：

   1.首先我们先进行实例化数据块，也就是Chunk类里面的blocks变量。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153701728.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
    2.调用UpdateChunk更新网格数据，在UpdateChunk中又调用了各个方块的BlockData函数生成网格数据。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019090415371122.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
    3.在BlockData中我们对当前方块根据检测相邻方块进行剔除面操作。
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153720692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
    （由于函数太大，所以只截一小部分）

   4.最后调用RenderMesh对网格数据进行更新。
    
    
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153728976.png)
在这里，为什么我们只生成了一个方块呢。因为要想生成方块，就必须调用BlockData函数，而BlockAir的BlockData函数里我们只做了一个返回，并没有生成网格数据，

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153736516.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
因此只生成了一个方块，也就是我们在后面修改的那个位置为（3，5，2）的方块，因为在Block类里BlockData函数已经生成了网格数据。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153748509.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
为什么克森不直接将所有的方块实例化为Block类呢，原因是这样做会造成数组下标越界。大家还记得下面这个函数吗？
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153803863.png)
假设当前方块的y为16，这y+1便会越界。对于这个处理，后续的文章中会有介绍。敬请关注吧。

忘记说最后一点了，对于为什么会生成一个方块呢。原因就是在下图判断中，如果返回为false则制作该方块对应的面，然后我们的BlockAir的IsSolid函数返回的就是false，因此我们的方块就出来了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153817834.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
## 二、基础篇：生成贴图
上一次我们制作了一些函数去设置网格数据，在这篇文章中，我将教大家怎么为方块贴图。

   贴图嘛，首先你得有图才行呀。因此请你将下面这张图右键保存到你的工程目录中：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153850993.png)

打开该图片的导入设置，设置图片类型（Texture Type）为 Advanced，然后将所有的勾都去掉，将过滤模式（Filter Mode）设置为 Point（no filter）。最后的设置如下所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153859780.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
好，让我们来看看为什么要这么设置呢？大家可从下面几张图中看出效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153907977.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
（point模式）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153931703.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
（Bilinear模式）

为什么会这样呢？总之请大伙们记住的就是，如果做像素游戏那么就选择Point模式就对了。而Bilinear模式会对纹理进行插值计算，会先找出最接近像素的四个图素进行插值运算，这会使得纹理更为平滑；而Point模式只是对纹理进行简单的插值，会使用包含像素最多的部分的图素来贴图，容易出现所谓的“马赛克”想象。而“马赛克”想象正是我们想要的效果。

接下来拖拽该图片到上一篇创建好的Chunk物体上，这时你会看到工程目录中多出了一个Metarials文件夹，这是由Unity根据拖拽的纹理自动生成的，这边省去了手动创建纹理的时间。

接下来打开我们的Block脚本，我们需要使用一个结构体去保存纹理信息，以便对纹理坐标进行修改：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904153940202.png)
        接下来我们创建一个函数，该函数用于根据指定方面的面对纹理进行修改，简单的说就是给指定方向上的面贴指定的纹理：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019090415394595.png)
    该函数很简单，就是返回一个修改好的Tile类型的结构体，也就是纹理上的坐标位置。

接下来我们需要一个float类型的产量，用于表示纹理位置的比例：

![在这里插入图片描述](https://img-blog.csdnimg.cn/201909041539551.png)
  为什么会是0.25f呢，下图有解释：
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904154003147.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
      如果还不理解，没关系，我们继续往下走。

下面的函数用于生成对应方向面的UV位置：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904154012524.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
好，暂时不解释，接下来再在faceData*（*代表各个方向上的函数）函数中添加如下代码：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019090415402298.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
最后在Chunk.cs脚本的RenderMesh函数中添加两行代码来渲染我们的UV贴图即可：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904154034436.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
 先不做解释，先测试一番看看效果：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190904154045126.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
         一个石块便完美的展示出来了。

好，接下来让我们来分析分析该石块制作的过程：

首先，用于生成UV位置的主要函数是如下函数：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhINmpmczYxdTRiMEhPQ2NqMWliSWJBVDVaWGJXY0ttYU4yUGljU1BZWEhHS3BuTndpYjZvWkFQTHpGNGgyTUVkMk5nOHl4OFEzN1l2NGJnLzA?x-oss-process=image/format,png)
 然后该函数调用了TexturePosition函数来生成UV的 x 和 y 的位置：
 ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhINmpmczYxdTRiMEhPQ2NqMWliSWJBVFdpYjFNbGphbzZZOFlHb2ZTclJQVFhuWmlidURkRW5zZVA4OTVzc1BVVWlhejRNa0ZyNm1JWEJkQS8w?x-oss-process=image/format,png)
 在该函数中将 x 和 y 都设置为0。然后让我们回到FaceUVs函数中来，计算计算最终生成UV的位置：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhINmpmczYxdTRiMEhPQ2NqMWliSWJBVHNmbUlnT3NCdzg0TkhUNmlhVWZRdU10NWliS2V1VUVDMklsZGliWjdpYUwzbTZrUlFPZnNHczFVc3cvMA?x-oss-process=image/format,png)
对应我们纹理中的位置如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhINmpmczYxdTRiMEhPQ2NqMWliSWJBVGQ5T0R3bTlmbHl2Z2RnSWNSNVFqaWFoRUN6dWNrNmduU1YwelpNZ0M0RDByQUJleFhuMTJaVmcvMA?x-oss-process=image/format,png)
      由于我们在各个方向的面都使用了同一个纹理坐标，因此该方块的每个都面都是上图中的纹理，接下来让我们生成文章前头看到的那个草块。

 让我们新建一个C#脚本，命名为“BlockGrass.cs”，双击打开脚本，为其添加如下代码：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhINmpmczYxdTRiMEhPQ2NqMWliSWJBVHBSMndYajFLVG03SmhHNW9hNzJWd21lTjNRQTBnMGN3dVdkYmRoZTMzTVB0dVBCT01QYzJzdy8w?x-oss-process=image/format,png)
其实这个脚本很简单，我们只是在TexturePosition函数中对某个方面上的面做了些特殊操作，仅此而已。

 然后在初始化创建方块的地方为其添加如下代码：
 ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhINmpmczYxdTRiMEhPQ2NqMWliSWJBVFE5aWJnVjlxdXB0UlY1bWEwNXNvZGlhbmJLc3lxZWpOSnV1bWNpYkNpY2Z2YzFrRm1ITmdvcHRLRncvMA?x-oss-process=image/format,png)
  这时候Play游戏，你将会看到如下图所示效果：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhINmpmczYxdTRiMEhPQ2NqMWliSWJBVGxmbTVpYXBwMnJvVkFJaWF6NmpDUFdralJKMk1NdTFTcVhZWGF5bmRwU0drR2VWM0xOZzBMcTNnLzA?x-oss-process=image/format,png)
  至于为什么会生成，前面已经有做过解释，这里就不再赘述。

## 三、基础篇：生成网格碰撞
 上一次我们为方块添加了贴图，这一章我将为方块添加网格碰撞。

从下图可知，我们的方块已经有网格数据了，然而我们的Mesh Collider的Mesh属性还没有任何网格数据。

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhGbE5GRjdTQXVMUW5rbDFMV28xSEVHamhXZWc0R1NvYXFicDZSMko3ZGh4djRodzhHTGZQUmdDUW1rUFBGbFpFRGM0TldqWjRaUWljZy8w?x-oss-process=image/format,png)

接下来，让我们来为方块添加网格碰撞。

提示
更新网格碰撞数据是很耗性能的，因此我们应该把网格碰撞设计得越简洁越好。如果你不打算使用Unity自带的物理系统，那么你可以百度一下“AABB”碰撞检测算法，这是最简单的碰撞算法之一了。

因为网格碰撞会随着网格数据的变化而变化，为了方面网格碰撞的数据能与网格数据同步变化，首先在“MeshDta.cs”脚本中添加下面变量：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhGbE5GRjdTQXVMUW5rbDFMV28xSEVHY3RFNTcyZEw0bEdlaWFkR28xWVd4dU1TN3J0T2hCaWNsZ1NMMWxLT1ZpYWFldVU5cEc3Q21oa1FRLzA?x-oss-process=image/format,png)
当该布尔变量为true的时候，我们在对网格添加面片数据的时候，也会对网格碰撞的面片数据进行更新，如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhGbE5GRjdTQXVMUW5rbDFMV28xSEVHS0s4aWJ2a0NOS2ljUDZQQkRSdDVUOW82eE5wQ01HN2lhWTVPODVoSWFyOU9WTTdvb0dLaEQ3Vk93LzA?x-oss-process=image/format,png)
请大家注意一下，我们为网格碰撞添加面片的时候使用的是colVertices.Count，而不是vertices.Count。

那么接下来，我们为网格碰撞添加顶点，如下图所示：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhGbE5GRjdTQXVMUW5rbDFMV28xSEVHSTRRSnIyUDFkdkpac2xRMUtBc0REMjMxRE4yY0NjZjNZOHhCMG9pY2ljRlBIRzE2N1RmOEhCVkEvMA?x-oss-process=image/format,png)
    好了，让我们回到“Block.cs”脚本中，修改一下网格顶点的添加方式：如下图所示：
    ![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhGbE5GRjdTQXVMUW5rbDFMV28xSEVHZ1I2Y2dCb05UQ0JLRlJVQVh3cDdRTzgzNWc1Y3hjMDhJUWVqQWJkRExQWGFhSVQ3WnJUakJRLzA?x-oss-process=image/format,png)
    提示
一定要将“Block.cs”原来添加顶点的方式换成转换的方式!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

这样便可在useRenderDataForCol为true的时候，添加网格的顶点数据的同时也添加了网格碰撞的顶点数据，是不是既方便又简单呀。

网格碰撞的顶点数据添加好了，接下来添加网格碰撞的三角形数据的步骤和添加顶点的步骤相似。

首先处理一下添加三角形的函数，如下图所示：

![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhGbE5GRjdTQXVMUW5rbDFMV28xSEVHNFMwUndHak9XWkR5YmpwWGdUcDU5VnQ4RHJnamRlMkY5VmE2QzdJSktobWZnNjk4dEV5a2hnLzA?x-oss-process=image/format,png)
提示
目前我们只使用了AddQuadTriangies()函数，上图的函数也许会在后面的开发中用的，之所以提前写上是因为该函数和本篇文章相关联。

接下来回到我们的“Block.cs”脚本的添加下面一行代码就可以跑一跑测试了：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhGbE5GRjdTQXVMUW5rbDFMV28xSEVHWVpNaWFoemljc2liSGM0OG1kaWFUMVJ0VzVscWZEMTZDUVoxT0E0d29od0FvY1VJZzRiaWFaQVhXckEvMA?x-oss-process=image/format,png)
将这行代码添上之后，按下ctrl+s，之后回到Unity点击Play按钮，将会看到如下图所示效果：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL21tYml6LnFwaWMuY24vbW1iaXpfcG5nL0xKMkZLT1NoNDhGbE5GRjdTQXVMUW5rbDFMV28xSEVHZDRpYVZ6Y2ljekV4UjdGd3M2T2JRMERrY3BhUHpYMnJVWkNlcU1YSlFxM25ZaFJXZG1FN05rNlEvMA?x-oss-process=image/format,png)
OK，这一篇结束

## 四、基础篇：添加地形管理
上一次我们为方块添加了网格碰撞，这一章将会创建一个地形管理相关的类。

首先创建一个名为“WorldPos.cs”的脚本，双击打开，码入如下代码：

```csharp
using System.Collections;

public struct WorldPos
{
    public int x, y, z;

public WorldPos(int x, int y, int z)
    {
        this.x = x;
        this.y = y;
        this.z = z;
    }
    // 重写了 Equals 方法，便于比较和方便字典操作（后面有讲）
    public override bool Equals(object obj)
    {
        if (!(obj is WorldPos))
            return false;

WorldPos pos = (WorldPos)obj;
        if (pos.x != x || pos.y != y || pos.z != z)
        {
            return false;
        }
        else
        {
            return true;
        }
    }
}
```
这个脚本里的代码很简单，也就是创建了一个带有三个 int 类型的结构体，然后重写了 Equals 方法，至于为什么这么做，接着往下看便知道了。


接下来创建一个名为“World.cs”的脚本，双击打开脚本，码入如下代码：

```csharp
sing UnityEngine;
using System.Collections.Generic;

public class World : MonoBehaviour {

// 用来管理 chunk
    public Dictionary<WorldPos, Chunk> chunks = new Dictionary<WorldPos, Chunk>();
    // chunk 预设体，用做创建对象的模板
    public GameObject chunkPrefab;
}
```
咳咳咳，到这里就知道为什么了要创建“WorldPos.cs”脚本，和重写 Equals 方法了吧。


那就是因为我们使用了字典结构来管理我们的 Chunk，而 key 为 WorldPos，我们都知道字典的 key 是唯一的，如果想要得到某个 key 对应的值，那么我们就需要传入相等的 key 才能够得到对应的值，因此我们就需要重写 WorldPos 的 Equals 方法。如果不重写 Equals 方法的话，默认对比两个 WorldPos 则是通过它们各自的 Hash 值来对比的，而每个 new 出来的对象的 Hash 都不相同，所以这就是为什么要重写 Equals 方法的主要原因。

逼逼那么多，也不知道说得对不对，233。 


Ok，让我们回到 Unity 编辑器中，然后按步骤执行如下操作：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9MSjJGS09TaDQ4SE1JTDdjVmduTGliU1FxWElnQWlhTElJbWVpYm1iZnppYmRlNVlWQ2d3VUt2a25rdklJNmY2V1MzZzlZaWM3b2JRblNmaWNndTlCeDhPRXdhQS82NDA?x-oss-process=image/format,png)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9MSjJGS09TaDQ4SE1JTDdjVmduTGliU1FxWElnQWlhTElJanZZblk4UnRVdXVpY2FSa29HSW1kNHFWZ1pxVmNuaWNHVHp1emxoRXppYWlicVRLRWNZN3pjeTVjdy82NDA?x-oss-process=image/format,png)
Ok，接下来让我们运行 Unity，你会发现什么都没有。


让我们双击打开“Chunk.cs”脚本，修改如下代码：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9MSjJGS09TaDQ4SE1JTDdjVmduTGliU1FxWElnQWlhTElJVDZXcDJwNXVUbWdTbmV4aWFFWTJNYzZ2dGtXT25iVkFoNlVYbFlGbzFHeUk5NG83d0VaNFVEZy82NDA?x-oss-process=image/format,png)
嚯嚯嚯，初始化代码都去掉了，我们要怎么创建 chunk 方块呀？


稍等骂爹，我们不是有一个用于管理 chunk 的类吗？对的，接下来让我们在 “World.cs”脚本里对“Chunk”进行初始化。回到“World.cs”脚本，添加如下代码：

```csharp
void Start()
    {
        // 初始化世界
        for (int x = 0; x < 1; x++)
        {
            for (int y = 0; y < 1; y++)
            {
                for (int z = 0; z < 1; z++)
                {
                    // 之所以乘上 Chunk.chunkSize，是用于确保每一个 Chunk 的范围
                    CreateChunk(x * Chunk.chunkSize, y * Chunk.chunkSize, z * Chunk.chunkSize);
                }
            }
        }
    }
```
上图中的代码较为简单，就不细说了，接下来添加“CreateChunk”函数，如下图所示：

```csharp
 public void CreateChunk(int x, int y, int z)
    {
        // 创建 WorldPos 对象，并初始化
        WorldPos worldPos = new WorldPos(x, y, z);

// 使用预设体创建游戏对象
        GameObject newChunkObject = Instantiate(
                        chunkPrefab, new Vector3(x, y, z),
                        Quaternion.Euler(Vector3.zero)
                    ) as GameObject;

// 获取对象上的 Chuck 组件
        Chunk newChunk = newChunkObject.GetComponent<Chunk>();

// 为组件赋值
        newChunk.pos = worldPos;
        newChunk.world = this;

// 将该 chuck 添加到字典中管理
        chunks.Add(worldPos, newChunk);

// 这段代码其实就是原来 Chunk.cs 脚本里初始化的代码
        for (int xi = 0; xi < Chunk.chunkSize; xi++)
        {
            for (int yi = 0; yi < Chunk.chunkSize; yi++)
            {
                for (int zi = 0; zi < Chunk.chunkSize; zi++)
                {
                    SetBlock(x + xi, y + yi, z + zi, new BlockGrass());
                }
            }
        }
    }
```
上图代码基本都上了注释，因此不再细说。接下来添加“SetBlock”函数，代码如下：

```csharp
 public void SetBlock(int x, int y, int z, Block block)
    {
        // 在这里封装了一层，用于做相关检测逻辑
        Chunk chunk = GetChunk(x, y, z);

if (chunk != null)
        {
            // 调用 chunk 的 SetBlock 函数，其实就是为 chunk 里的 blocks 数组设置对应的值，
            // 只不过也在该函数中做了相关检测处理逻辑
            chunk.SetBlock(x - chunk.pos.x, y - chunk.pos.y, z - chunk.pos.z, block);
            chunk.update = true;
        }
    }
```
接下来先添加“GetChunk”函数，代码如下：

```csharp
  public Chunk GetChunk(int x, int y, int z)
    {
        // 下面五行代码主要用于计算当前 Block 位置对应的 chunk 于字典中的位置，并为 pos 赋值
        // 因为我们创建 chunk 是使用 CreateChunk(x * Chunk.chunkSize, y * Chunk.chunkSize, z * Chunk.chunkSize); 来创建的
        // 下面的操作只是将其操作反向计算了一下，仅此而已。
        WorldPos pos = new WorldPos();
        float multiple = Chunk.chunkSize;
        pos.x = Mathf.FloorToInt(x / multiple) * Chunk.chunkSize;
        pos.y = Mathf.FloorToInt(y / multiple) * Chunk.chunkSize;
        pos.z = Mathf.FloorToInt(z / multiple) * Chunk.chunkSize;

Chunk chunk = null;

chunks.TryGetValue(pos, out chunk);

return chunk;
    }
```
接下来让我们回到“Chunk.cs”脚本中，添加如下关联函数，代码如下所示：

```csharp
 public static bool InRange(int index)
    {
        // 因为我们的 chunk 为一个正方体，
        // 因此这里的逻辑就是判断 x、y、z 的位置时候在该立方体内
        if (index < 0 || index >= chunkSize)
            return false;

return true;
    }

public void SetBlock(int x, int y, int z, Block block)
    {
        // InRange 故名思意就是判断传入的位置是否正确
        if (InRange(x) && InRange(y) && InRange(z))
        {
            blocks[x, y, z] = block;
        }
    }
```
Ok，码到这里基本功能就完工了，让我们运行 Unity，查看效果，此时会报如下错误：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9MSjJGS09TaDQ4SE1JTDdjVmduTGliU1FxWElnQWlhTElJNG5aOUdiRVBKQVBQaWNwUDFTdlA3NWFsWGlhNVU3NGpaUGtUZ1dFdnBQc05Id3llenBDc2pzeUEvNjQw?x-oss-process=image/format,png)
双击点击错误，便来到了报错的位置。报错的位置于“Chunk.cs”脚本中的“SetBlock”函数，这里为什么会报错，怎么看也没毛病啊。经过克森一系列的猜测，果然如此，组件的一些方法调用顺序出了问题。具体解决方案就是把“Chunk.cs”脚本你的“Start”函数该为“Awake”即可，如下所示：

```csharp
   void Awake()
    {
        filter = gameObject.GetComponent<MeshFilter>();
        coll = gameObject.GetComponent<MeshCollider>();
        blocks = new Block[chunkSize, chunkSize, chunkSize];
    }
```
好的，继续运行 Unity，居然还是报错了，错误如下：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9MSjJGS09TaDQ4SE1JTDdjVmduTGliU1FxWElnQWlhTElJa3lWM2RUUXQyNmw5VGtldkpTNnlHZU9nTDdVNGZQMmM5Y0NWUDBXYmliRjJRZVAyNEdBeVZnUS82NDA?x-oss-process=image/format,png)
双击点击错误，便来到了报错的位置。报错的位置于“Chunk.cs”脚本中的“GetBlock”函数，报错信息为数组下标越界。

嘿，这个错误非常眼熟呀，之前的文章中也报了这个错误。其实就是因为我们在“Block.cs”脚本的“BlockData”函数中进行了如下判断：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9MSjJGS09TaDQ4SE1JTDdjVmduTGliU1FxWElnQWlhTElJY3ZSMXVCbFlZMjY5ejA0T3RxQXRsWW1BSnZrU1YzWVpISENycXlQd3hYbnoxS1g4UHhzTUdRLzY0MA?x-oss-process=image/format,png)
然后我们是在“Chunk.cs”脚本的“UpdateChunk”函数中调用了“BlockData”函数，如下所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9MSjJGS09TaDQ4SE1JTDdjVmduTGliU1FxWElnQWlhTElJV0VLbXljV1dpYk0yckRsaWMwUlkyVDMxOUJUcFJHVHhQdWc0R1lKUjhpYzc1VUV5SmNGdGZRaWFzZy82NDA?x-oss-process=image/format,png)
在这个函数中，我们会传入的最大值为“chunkSize - 1” ，然而在“BlockData”函数中的判断中会进行“* + 1”(*表示 x、y、z 任意一个)，最终会传入“GetBlock”函数中的最大参数值为“chunkSize”，因此便造成了数组下标越界。

Ok，逼逼了那么多，我们该怎么处理了，其实很简单，做个简单的判断即可，这个时候我们的“IsRange”函数便派上用场咯，修改的代码如下所示：

```csharp
  public Block GetBlock(int x, int y, int z)
    {
        // 判断传入的位置时候位于该 chunk 中，
        // 如果不存在则默认返回一个“BlockAir”方块（因为 BlockAir 方块是一个空方块，不会影响其它逻辑）
        if (InRange(x) && InRange(y) && InRange(z))
            return blocks[x, y, z];
        return new BlockAir();
    }
```
Ok，这个时候再运行 Unity，便会看到如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9MSjJGS09TaDQ4SE1JTDdjVmduTGliU1FxWElnQWlhTElJb2RTcUcyQ1NNT2pCWVM5cGNEeGs0dTI1RnFibnBhajNXcDJtdVdIdjV1N2xaVXhjWDhLMTZRLzY0MA?x-oss-process=image/format,png)
嚯嚯嚯，看来是成功了，倍儿棒。


Ok，让我们杂耍一下我们的成果，修改如下代码：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9MSjJGS09TaDQ4SE1JTDdjVmduTGliU1FxWElnQWlhTElJRWR3TXVWSFUxYTdWVjlvRUd0eG90RFhzQjM3M0w4R3k0dklBYjZOZFhjYWRQaWJLOGFnSjVBZy82NDA?x-oss-process=image/format,png)
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9MSjJGS09TaDQ4SE1JTDdjVmduTGliU1FxWElnQWlhTElJZGN3QnAzc1B6bVNGemhIQzJ6TWliVk11WDRuVG5IQlJMWTVrQzRsT3FManE1U2tpY3M3clB0T3cvNjQw?x-oss-process=image/format,png)
运行 Unity，便看到如下图所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9MSjJGS09TaDQ4SE1JTDdjVmduTGliU1FxWElnQWlhTElJNWtZWmNpY09rTENiT1h1NEtTNTBBMklZZjd3cEtHazd0NFJCRHFER0Y1RW12eHBTamJ6Uk1Ydy82NDA?x-oss-process=image/format,png)
嚯嚯嚯，是我们想要的效果，不错，可以的，兄Dei。


接下来让我们看一下生成的 Chunk 网格是怎么样的，具体操作步骤如下所示：
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy9MSjJGS09TaDQ4SE1JTDdjVmduTGliU1FxWElnQWlhTElJdTRwek5ORVlZMDM0UTQ4cjZpY3NoZ2liczZEcnlyVk9WeUxsSGliVnlBUDk1WVdzbGlha2RRTzREZy82NDA?x-oss-process=image/format,png)
嚯嚯嚯，不错，也是我们想要的效果。chunk 里的多余的面被过滤掉了，这样便节省了贼多性能，哦耶！


Ok，文章至此基本结束了，最后让我们为“World.cs”脚本添加两个有用的函数，代码如下所示：

```csharp
    public Block GetBlock(int x, int y, int z)
    {
        Chunk chunk = GetChunk(x, y, z);

if (chunk != null)
        {
            Block block = chunk.GetBlock(
                x - chunk.pos.x,
                y - chunk.pos.y,
                z - chunk.pos.z);

return block;
        }
        else
        {
            return new BlockAir();
        }
    }


    public void DestroyChunk(int x, int y, int z)
    {
        // 逻辑简单，就是找到指定的 chunk，然后先销毁游戏对象，再移除管理即可（移除对应字典的值）
        Chunk chunk = null;
        if (chunks.TryGetValue(new WorldPos(x, y, z), out chunk))
        {
            Object.Destroy(chunk.gameObject);
            chunks.Remove(new WorldPos(x, y, z));
        }
    }
```
代码逻辑较为简单，因此就不逼逼了，好了，文章至此就结束吧。

本章源码：

链接：https://pan.baidu.com/s/1O4n7JjQVy8ibc6G1DhBmHQ 
提取码：6403 
