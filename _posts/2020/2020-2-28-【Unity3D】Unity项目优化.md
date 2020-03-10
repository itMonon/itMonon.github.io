---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity鼠标控制物体的旋转、移动、缩放等
tagline: by 恬静的小魔龙
tag: Unity3D
---

##一、前言
做游戏经验比较丰富的人都知道，优化的好坏一直是一个游戏的评判标准之一，它直接影响着玩家们的游戏体验，优化一直是项目中开发周期比较长的一个点，也是开发者头疼的一个问题，要求掌握的知识点比较全面，经验也要求比较丰富。
这篇文章参考很多文章的知识点，加以总结与学习，从最基础的概念讲起，配合讲解各种优化技巧，希望大家可以在我的文章中学到一些东西。

##二、参考文章
【Unity优化总结】https://blog.csdn.net/ynnmnm/article/details/36759789
【Unity教程之再谈Unity中的优化技术】https://www.cnblogs.com/gabo/p/4592194.html
【Unity优化之GC——合理优化Unity的GC】https://www.cnblogs.com/zblade/p/6445578.html
【Unity优化Unity优化技巧进阶(持续更新中...)】https://blog.csdn.net/andrewfan/article/details/59057811
【Unity性能优化】https://blog.csdn.net/wwlcsdn000/article/details/78777639
【Unity优化技巧】http://www.360doc.com/content/17/1215/10/30388632_713252002.shtml#
【Unity教程之再谈Unity中的优化技术】http://www.unity.5helpyou.com/2786.html

##三、正文
###目录
###一、CPU优化
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DrawCall优化
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;算法优化
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;物理算法优化
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;物理效果优化
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;GC优化
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;脚本优化
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;文件优化

###二、GPU优化
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;顶点计算优化
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;片元计算优化
###三、带宽（资源）优化
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;纹理贴图优化
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;帧优化
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;资源优化
###四、Unity3d自带的优化技术
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;优化几何体
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;LOD
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;遮挡剔除
####&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;批处理
###五、官方建议


<br></br>
<br></br>

<br></br>
<br></br>

----------


###一、CPU优化
####DrawCall优化
DrawCall一直都是老生常谈的问题了，为什么总是这个东西在消耗资源呢，这是个什么东西呢

##1.什么是DrawCall
Drawcall是CPU向GPU发送绘制命令的接口调用。理论上每一个不同材质的物件需要渲染在屏幕上时，CPU都会调用图形API ( openGL or Diract3D ) 的Draw接口触发显卡进行绘制。

##2.为什么优化Drawcall?
Drawcall对硬件和驱动而言，要求大量设置状态（使用哪些顶点、哪些shader等）和状态转换。而Drawcall最大的消耗在于：如果每次drawcall只提交少量的数据将导致CPU瓶颈，CPU无法将GPU填满。Drawcall对GPU的耗费在于硬件一直等待CPU提交数据，而无法得到有效利用。GPU大量的时间耗费在不断切换状态和正确性检测上。 GPU在Draw Call之间，为了防止前后Draw的依赖关系造成绘制错误或者资源竞用，一般会在Draw Call后Flush整个流水线，小粒度的Draw Call对GPU流水线来说是个很大的浪费。（这个问题在D3D老版本存在，在新版D3D11中得到改善。）实际上unity官方指出，Drawcall数量的降低并非重点，重点是减少批次的数量，Drawcall优化实际上是对批次数量的优化。 

延伸阅读：
　　· [why are draw calls expensive? — Stack Overflow](https://stackoverflow.com/questions/4853856/why-are-draw-calls-expensive)
　　· [Direct3D Draw函数 异步调用原理解析](http://www.cnblogs.com/lingjingqiu/p/3170727.html)

##3.如何优化Drawcall？
在Unity中对Drawcall的优化有以下几个策略：Drawcall batching，合并打包图集，减少光照和阴影以及遮挡剔除和视锥剔除等。以下分别谈一下各个策略的优缺点。

##3.1Drawcall Batching

- 静态批次 Drawcall static batching
 　　场景中的多个物件如果是不移动的（包括位置、缩放、旋转等），并且共享同一材质，比如地形、建筑、花盆等，那么可以选择采用静态批次。静态批次只需要在Inspector勾选static选项即可。静态批次需要注意的是，unity会将进行批次的多个对象合并成一个大的对象，也会导致内存损耗，有时候要避免太多对象静态批次造成的内存过高。这也表明，优化并非绝对做好某一方面，而是平衡各个硬件的瓶颈和效率，选择相对适中的方案。
- 动态批次 Drawcall dynamic batching
　　动态批次是运动的物件在unity中也可以进行批次渲染，动态批次不需要手动设置，是unity自动进行的，但是这里有诸多陷阱和约束，开发者需要遵守一定的限制条件才能享受动态批次的好处。
　　
　　1) . 动态批次是逐顶点处理的，因此仅对少于900个顶点的mesh有效。如果shader使用了顶点位置，法线和UV那么仅支持低于300顶点的mesh，而如果shader使用了顶点位置，法线、UV0、UV1和切向量，则之多仅支持180顶点。

　　2) . 缩放问题

　　缩放对于批次是有影响的，这里涉及到一个统一缩放和非统一缩放的概念。统一缩放即为三轴同比例缩放，比如（1,1,1），（2,2,2）（5,5,5）... 非统一缩放即为三轴不同比例缩放，如（1,2,1）（2,1,1）（1,2,3）等等。

　　Unity对统一缩放的对象是不进行动态批次的，而对非同一缩放的对象是可以进行动态批次的。这里有点诡异，查阅了一些资料，解释如下：

　　对于非同一缩放的物件，unity将其mesh进行了复制，因此即便是从相同物件进行的非同一缩放的两个对象是两份mesh；对统一缩放的对象来说，unity不对mesh进行复制，而是使用同一mesh进行缩放，此时复制mesh来进行批次渲染是不值得的，但是对于非统一缩放的对象，既然已经复制了mesh（不是为了批次，而是其他原因决定复制mesh）,那么进行批次是顺带实现的。

　　很多时候分辨率也是造成性能下降的原因，尤其是现在很多国内山寨机，除了分辨率高其他硬件简直一塌糊涂，而这恰恰中了游戏性能的两个瓶颈：过大的屏幕分辨率+糟糕的GPU。因此，我们可能需要对于特定机器进行分辨率的放缩。当然，这样会造成游戏效果的下降，但性能和画面之间永远是个需要权衡的话题。
在Unity中设置屏幕分辨率可以直接调用Screen.SetResolution。

   （参考 [Dynamic Batching and Scale ——unity3d answers](http://answers.unity3d.com/questions/607772/dynamic-batching-and-scale.html)  ）

　　3) . 使用了不同的材质，即便实质上是相同的（比如两个一模一样的材质），也不会进行批次。

　　4) . 拥有lightmap的物件含有额外的材质属性，比如lightmap偏移和缩放系数等，所以拥有lightmap的物件不能批次。

　　5) . 多通道的shader会妨碍批处理操作，接受实时阴影的物件无法批次。
　　
　　6).   unity渲染是有顺序的，渲染排序有可能打断动态批次。
　　
##3.2渲染顺序跟什么有关呢？

   首先根据物件到摄像机的距离，进行远处物件先渲染近处物件后渲染。相同材质的物件尽量在一层，不要让不同材质的物件进入这一层。如果无法保证这一点，那么还有一种方法：修改shader中渲染队列值。即打开shader 将subshader中的tag｛｝中queue 修改为小于2500的值。

   　　渲染队列小于等于2500时，unity认为其是不透明的，对于不同材质但z值相同对象，unity不对其进行排序，这样能保证相同材质的多个对象能是一个批次，不同材质的对象如果进入两个相同材质的对象之间，不会打破批次；

   　　 渲染队列大于2500时，unity会对不同材质的对象进行排序，此时如果不同材质的对象进入到两个相同材质的对象之间的话，会使相同材质的对象批次被打破。

　　批次先写到这，其实很多网上都有，不过有些没深入讲解，也有些没给出解决办法，我就使用每个方案时遇到的困难给出了自己的解决方案。其实批次还有不少研究的地方，之后想到了会继续更新。 

##3.3如何控制渲染顺序
需要控制绘制顺序，主要原因是为了最大限度的避免overdraws，也就是同一个位置的像素可以需要被绘制多变。在PC上，资源无限，为了得到最准确的渲染结果，绘制顺序可能是从后往前绘制不透明物体，然后再绘制透明物体进行混合。但在移动平台上，这种会造成大量overdraw的方式显然是不适合的，我们应该尽量从前往后绘制。从前往后绘制之所以可以减少overdraw，都是因为深度检验的功劳。
在Unity中，那些Shader中被设置为“Geometry” 队列的对象总是从前往后绘制的，而其他固定队列（如“Transparent”“Overla”等）的物体，则都是从后往前绘制的。这意味这，我们可以尽量把物体的队列设置为“Geometry” 。
而且，我们还可以充分利用Unity的队列来控制绘制顺序。例如，对于天空盒子来说，它几乎覆盖了所有的像素，而且我们知道它永远会在所有物体的后面，因此它的队列可以设置为“Geometry+1”。这样，就可以保证不会因为它而造成overdraws。


　　延伸阅读： 　　

　　· [Unity -Draw Call Batching](http://docs.unity3d.com/Manual/DrawCallBatching.html) 

　　· [Unity Drawcall 优化手记](http://www.cnblogs.com/ybgame/p/3588795.html) 

##4.合并图集
　　其实合并图集也是利用了Unity的Drawcall batching。将多个纹理进行打包成图集是为了减少材质，这样多个对象共享一个材质，并进而使用同一个纹理和shader，触发unity的动态批次。图集打包工具有很多，Asset store中也可以搜到不少，比如Texture Packer Free 、 DrawCall Optimizer(收费) Mesh Baker Free 等等都可以将贴图打包合并。

　　但是合并图集也有缺点，合并贴图时应该注意选择同时出现在屏幕的对象贴图进行合并。如果不能做到这一点，那么合并图集可能起到反作用，即渲染一个对象需要加载过多无用贴图，造成内存占用率升高。我的项目这个方案也是采用之后又弃用的，因为归类同时出现在屏幕的贴图并非易事！
　　
##5.光照和阴影 
实时光照和阴影可能增加Drawcall，带有光源计算的shader材质会因为光照产生多个Drawcall。使用灯光会打断Drawcall batching，尽量使用烘焙灯光贴图等技巧来实现灯光效果。


##6.时刻警惕透明物体
而对于透明对象，由于它本身的特性（可以看之前关于Alpha Test和Alpha Blending的一篇文章）决定如果要得到正确的渲染效果，就必须从后往前渲染（这里不讨论使用深度的方法），而且抛弃了深度检验。这意味着，透明物体几乎一定会造成overdraws。如果我们不注意这一点，在一些机器上可能会造成严重的性能下面。例如，对于GUI对象来说，它们大多被设置成了半透明，如果屏幕中GUI占据的比例太多，而主摄像机又没有进行调整而是投影整个屏幕，那么GUI就会造成屏幕的大量overdraws。
因此，如果场景中大面积的透明对象，或者有很多层覆盖的多层透明对象（即便它们每个的面积可以都不大），或者是透明的粒子效果，在移动设备上也会造成大量的overdraws。这是应该尽量避免的。
对于上述GUI的这种情况，我们可以尽量减少窗口中GUI所占的面积。如果实在无能为力，我们可以把GUI绘制和三维场景的绘制交给不同的摄像机，而其中负责三维场景的摄像机的视角范围尽量不要和GUI重叠。对于其他情况，只能说，尽可能少用。当然这样会对游戏的美观度产生一定影响，因此我们可以在代码中对机器的性能进行判断，例如首先关闭所有的耗费性能的功能，如果发现这个机器表现非常良好，再尝试开启一些特效功能。



　　延伸阅读：

　　　　· [Forward Rendering Path Details](http://docs.unity3d.com/Manual/RenderTech-ForwardRendering.html) 

　　　　· [Light Troubleshooting and Performance ](http://docs.unity3d.com/Manual/LightPerformance.html)


####算法优化

1. 降低算法的复杂度，了解常见的数据结构和算法
2. 使用协程来避免多余的更新计算。
3. 缓存耗时计算的中间值
4. 缓存获取过的Componenet
5. IMGUI只用与测试，发布时应该屏蔽

####物理算法优化
- 尽可能少用刚体，不重要的物体可以自己写简易的模拟物理
- 尽可能减少直接的模型网格碰撞，应使用包围盒简易碰撞体替代
- 如果可以的话，增加fixedUpdate的运行间隔
- 使用高效的射线检测算法，并避免产生GC

####物理效果优化
- 镜头
Clipping Planes 
Occlusion Culling 默认勾上了。但是没有任何效果 
打开Window-Occlusion Culling 需要bake 一下 
需要bake的东西，必须是Static 
Smallers Occluder 挡住后面的东西，优化做不好，就可能是负优化 
在Scene场景，选中摄像机，可以设置Occlusion Culling是Edit或者Visualize。这个时候随着镜头的移动，镜头中的物体就会动态的显示了。 
从屏幕上看到的点，都不会剔除掉的。 
Unity 3专业版内置了一个强大的 Occlusion Culling 插件 Umbra免费的
- 光照
Bake and Probes 
我们的目标就是降低：SetPass Calls 
Light 选则Bake光，使用静态光。出现色差 
设置：Scenes In Build Player Setting 
找到Color Space 有 Linear（不支持移动端） Gamma 
缺点：移动的物体不会受到光照的影响。这个时候就需要创建光照探针了。Light-Light Probe Group.记录静态光照的效果。
- 碰撞
Collider尽可能简单 
控制rigidbody数量 
Rigidbody检查方式，检测间隔，Collision Detection 持续的。离散型的。
- CheckList 
Simple checklist to make your game faster 
对于PC建筑（取决于目标GPU）时，请记住下面的200K和3M顶点数每帧。 
如果你使用内置着色器，从挑选的那些移动或熄灭类别。他们在非移动平台以及工作，但更复杂的着色器的简化和近似版本。 
保持每个场景低的不同材料的数量，并共享不同的对象尽可能之间尽可能多的材料。 
将Static非运动物体的属性，以允许像内部优化静态批次。 
只有一个（最好是定向）pixel light影响几何体，而不是整数倍。 
烘烤照明，而不是使用动态照明。 
尽可能使用压缩纹理格式，以及超过32位纹理使用16位纹理。 
避免使用雾在可能的情况。 
使用遮挡剔除，以减少可见的几何图形的量和抽取呼叫中的有很多闭塞复杂静态场景的情况。闭塞记扑杀设计你的水平。 
使用包厢到“假”遥远的几何体。 
使用像素着色器或纹理组合搭配，而不是多遍方法有几个纹理。 
使用half精度变量在可能的情况。 
尽量减少使用复杂的数学运算，如的pow，sin并cos在像素着色器。 
使用每个片段较少纹理。


####GC优化
　　在游戏运行的时候，数据主要存储在内存中，当游戏的数据在不需要的时候，存储当前数据的内存就可以被回收以再次使用。内存垃圾是指当前废弃数据所占用的内存，垃圾回收（GC）是指将废弃的内存重新回收再次使用的过程。

　　Unity中将垃圾回收当作内存管理的一部分，如果游戏中废弃数据占用内存较大，则游戏的性能会受到极大影响，此时垃圾回收会成为游戏性能的一大障碍点。

　　下面我们主要学习垃圾回收的机制，垃圾回收如何被触发以及如何提GC收效率来提高游戏的性能。

##1.  Unity内存管理机制简介

　　要想了解垃圾回收如何工作以及何时被触发，我们首先需要了解unity的内存管理机制。Unity主要采用自动内存管理的机制，开发时在代码中不需要详细地告诉unity如何进行内存管理，unity内部自身会进行内存管理。这和使用C++开发需要随时管理内存相比，有一定的优势，当然带来的劣势就是需要随时关注内存的增长，不要让游戏在手机上跑“飞”了。

　　unity的自动内存管理可以理解为以下几个部分：

　　1）unity内部有两个内存管理池：堆内存和堆栈内存。堆栈内存(stack)主要用来存储较小的和短暂的数据，堆内存(heap)主要用来存储较大的和存储时间较长的数据。

　　2）unity中的变量只会在堆栈或者堆内存上进行内存分配，变量要么存储在堆栈内存上，要么处于堆内存上。

　　3）只要变量处于激活状态，则其占用的内存会被标记为使用状态，则该部分的内存处于被分配的状态。

　　4）一旦变量不再激活，则其所占用的内存不再需要，该部分内存可以被回收到内存池中被再次使用，这样的操作就是内存回收。处于堆栈上的内存回收及其快速，处于堆上的内存并不是及时回收的，此时其对应的内存依然会被标记为使用状态。

　　5) 垃圾回收主要是指堆上的内存分配和回收，unity中会定时对堆内存进行GC操作。

　　在了解了GC的过程后，下面详细了解堆内存和堆栈内存的分配和回收机制的差别。

##2.堆栈内存分配和回收机制

　　堆栈上的内存分配和回收十分快捷简单，因为堆栈上只会存储短暂的或者较小的变量。内存分配和回收都会以一种顺序和大小可控制的形式进行。

　　堆栈的运行方式就像stack: 其本质只是一个数据的集合，数据的进出都以一种固定的方式运行。正是这种简洁性和固定性使得堆栈的操作十分快捷。当数据被存储在堆栈上的时候，只需要简单地在其后进行扩展。当数据失效的时候，只需要将其从堆栈上移除。

 

##3. 堆内存分配和回收机制

　　堆内存上的内存分配和存储相对而言更加复杂，主要是堆内存上可以存储短期较小的数据，也可以存储各种类型和大小的数据。其上的内存分配和回收顺序并不可控，可能会要求分配不同大小的内存单元来存储数据。

　　堆上的变量在存储的时候，主要分为以下几步：

　　1）首先，unity检测是否有足够的闲置内存单元用来存储数据，如果有，则分配对应大小的内存单元；

　　2）如果没有足够的存储单元，unity会触发垃圾回收来释放不再被使用的堆内存。这步操作是一步缓慢的操作，如果垃圾回收后有足够大小的内存单元，则进行内存分配。

　　3）如果垃圾回收后并没有足够的内存单元，则unity会扩展堆内存的大小，这步操作会很缓慢，然后分配对应大小的内存单元给变量。

　　堆内存的分配有可能会变得十分缓慢，特别是在需要垃圾回收和堆内存需要扩展的情况下，通常需要减少这样的操作次数。

##4.垃圾回收时的操作

　　当堆内存上一个变量不再处于激活状态的时候，其所占用的内存并不会立刻被回收，不再使用的内存只会在GC的时候才会被回收。

　　每次运行GC的时候，主要进行下面的操作：

　　1）GC会检查堆内存上的每个存储变量；

　　2）对每个变量会检测其引用是否处于激活状态；

　　3）如果变量的引用不再处于激活状态，则会被标记为可回收；

　　4）被标记的变量会被移除，其所占有的内存会被回收到堆内存上。

　　GC操作是一个极其耗费的操作，堆内存上的变量或者引用越多则其运行的操作会更多，耗费的时间越长。

##5. 何时会触发垃圾回收

 　　主要有三个操作会触发垃圾回收：

　　 1） 在堆内存上进行内存分配操作而内存不够的时候都会触发垃圾回收来利用闲置的内存；

　　 2） GC会自动的触发，不同平台运行频率不一样；

　　 3） GC可以被强制执行。

　　特别是在堆内存上进行内存分配时内存单元不足够的时候，GC会被频繁触发，这就意味着频繁在堆内存上进行内存分配和回收会触发频繁的GC操作。

 

##6. GC操作带来的问题

　　在了解GC在unity内存管理中的作用后，我们需要考虑其带来的问题。最明显的问题是GC操作会需要大量的时间来运行，如果堆内存上有大量的变量或者引用需要检查，则检查的操作会十分缓慢，这就会使得游戏运行缓慢。其次GC可能会在关键时候运行，例如在CPU处于游戏的性能运行关键时刻，此时任何一个额外的操作都可能会带来极大的影响，使得游戏帧率下降。

　　另外一个GC带来的问题是堆内存的碎片划。当一个内存单元从堆内存上分配出来，其大小取决于其存储的变量的大小。当该内存被回收到堆内存上的时候，有可能使得堆内存被分割成碎片化的单元。也就是说堆内存总体可以使用的内存单元较大，但是单独的内存单元较小，在下次内存分配的时候不能找到合适大小的存储单元，这也会触发GC操作或者堆内存扩展操作。

　　堆内存碎片会造成两个结果，一个是游戏占用的内存会越来越大，一个是GC会更加频繁地被触发。

 

##7. 分析GC带来的问题

　　GC操作带来的问题主要表现为帧率运行低，性能间歇中断或者降低。如果游戏有这样的表现，则首先需要打开unity中的profiler window来确定是否是GC造成。

　　了解如何运用profiler window，可以参考此处，如果游戏确实是由GC造成的，可以继续阅读下面的内容。

##8. 分析堆内存的分配

　　如果GC造成游戏的性能问题，我们需要知道游戏中的哪部分代码会造成GC，内存垃圾在变量不再激活的时候产生，所以首先我们需要知道堆内存上分配的是什么变量。

　　堆内存和堆栈内存分配的变量类型

 　　在Unity中，值类型变量都在堆栈上进行内存分配，其他类型的变量都在堆内存上分配。如果你不知道值类型和引用类型的差别，可以查看此处。

　　下面的代码可以用来理解值类型的分配和释放,其对应的变量在函数调用完后会立即回收：
　　

```
void ExampleFunciton()
{
    
  int localInt = 5;  
}
```
对应的引用类型的参考代码如下，其对应的变量在GC的时候才回收：

```
void ExampleFunction()
{
  List localList = new List();      
}
```
##9. 利用profiler window 来检测堆内存分配
 　　我们可以在profier window中检查堆内存的分配操作：在CPU usage分析窗口中，我们可以检测任何一帧cpu的内存分配情况。其中一个选项是GC Alloc，通过分析其来定位是什么函数造成大量的堆内存分配操作。一旦定位该函数，我们就可以分析解决其造成问题的原因从而减少内存垃圾的产生。现在Unity5.5的版本，还提供了deep profiler的方式深度分析GC垃圾的产生。
##10. 降低GC的影响的方法
　大体上来说，我们可以通过三种方法来降低GC的影响：

　　1）减少GC的运行次数；

　　2）减少单次GC的运行时间；

　　3）将GC的运行时间延迟，避免在关键时候触发，比如可以在场景加载的时候调用GC

似乎看起来很简单，基于此，我们可以采用三种策略：

　　1）对游戏进行重构，减少堆内存的分配和引用的分配。更少的变量和引用会减少GC操作中的检测个数从而提高GC的运行效率。

　　2）降低堆内存分配和回收的频率，尤其是在关键时刻。也就是说更少的事件触发GC操作，同时也降低堆内存的碎片化。

　　3）我们可以试着测量GC和堆内存扩展的时间，使其按照可预测的顺序执行。当然这样操作的难度极大，但是这会大大降低GC的影响。
　　
##11. 减少内存垃圾的数量
减少内存垃圾主要可以通过一些方法来减少：

##11.1缓存

 　　如果在代码中反复调用某些造成堆内存分配的函数但是其返回结果并没有使用，这就会造成不必要的内存垃圾，我们可以缓存这些变量来重复利用，这就是缓存。

　　 例如下面的代码每次调用的时候就会造成堆内存分配，主要是每次都会分配一个新的数组：

```
void OnTriggerEnter(Collider other)
{
     Renderer[] allRenderers = FindObjectsOfType<Renderer>();
     ExampleFunction(allRenderers);      
}
```
对比下面的代码，只会生产一个数组用来缓存数据，实现反复利用而不需要造成更多的内存垃圾：

```
private Renderer[] allRenderers;
 
void Start()
{
   allRenderers = FindObjectsOfType<Renderer>();
}
 
void OnTriggerEnter(Collider other)
{
    ExampleFunction(allRenderers);
}
```
##11.2 不要在频繁调用的函数中反复进行堆内存分配
 　　在MonoBehaviour中，如果我们需要进行堆内存分配，最坏的情况就是在其反复调用的函数中进行堆内存分配，例如Update()和LateUpdate()函数这种每帧都调用的函数，这会造成大量的内存垃圾。我们可以考虑在Start()或者Awake()函数中进行内存分配，这样可以减少内存垃圾。

　　下面的例子中，update函数会多次触发内存垃圾的产生：

```
void Update()
{
    ExampleGarbageGenerationFunction(transform.position.x);
}
```
通过一个简单的改变，我们可以确保每次在x改变的时候才触发函数调用，这样避免每帧都进行堆内存分配：

```
private float previousTransformPositionX;

void Update()
{
    float transformPositionX = transform.position.x;
    if(transfromPositionX != previousTransformPositionX)
    {
        ExampleGarbageGenerationFunction(transformPositionX);    
        previousTransformPositionX = trasnformPositionX;
    }
}
```
另外的一种方法是在update中采用计时器，特别是在运行有规律但是不需要每帧都运行的代码中，例如：

```
void Update()
{
    ExampleGarbageGeneratiingFunction()
}
```
通过添加一个计时器，我们可以确保每隔1s才触发该函数一次：

```
private float timeSinceLastCalled;
private float delay = 1f;
void Update()
{
    timSinceLastCalled += Time.deltaTime;
    if(timeSinceLastCalled > delay)
    {
         ExampleGarbageGenerationFunction();
         timeSinceLastCalled = 0f;
    }
}
```
　　通过这样细小的改变，我们可以使得代码运行的更快同时减少内存垃圾的产生。

　　附： 不要忽略这一个方法，在最近的项目性能优化中，我经常采用这样的方法来优化游戏的性能，很多对于固定时间的事件回调函数中，如果每次都分配新的缓存，但是在操作完后并不释放，这样就会造成大量的内存垃圾，对于这样的缓存，最好的办法就是当前周期回调后执行清除或者标志为废弃。

##11.3 清除链表
　　在堆内存上进行链表的分配的时候，如果该链表需要多次反复的分配，我们可以采用链表的clear函数来清空链表从而替代反复多次的创建分配链表。

```
void Update()
{
    List myList = new List();
    PopulateList(myList);       
}
```
通过改进，我们可以将该链表只在第一次创建或者该链表必须重新设置的时候才进行堆内存分配，从而大大减少内存垃圾的产生：

```
private List myList = new List();
void Update()
{
    myList.Clear();
    PopulateList(myList);
}
```
##11.4 对象池
　　即便我们在代码中尽可能地减少堆内存的分配行为，但是如果游戏有大量的对象需要产生和销毁依然会造成GC。对象池技术可以通过重复使用对象来降低堆内存的分配和回收频率。对象池在游戏中广泛的使用，特别是在游戏中需要频繁的创建和销毁相同的游戏对象的时候，例如枪的子弹这种会频繁生成和销毁的对象。

　　要详细的讲解对象池已经超出本文的范围，但是该技术值得我们深入的研究This tutorial on object pooling on the Unity Learn site对于对象池有详细深入的讲解。

　　附：对象池技术属于游戏中比较通用的技术，如果有闲余时间，大家可以学习一下这方面的知识。

##12 造成不必要的堆内存分配的因素
我们已经知道值类型变量在堆栈上分配，其他的变量在堆内存上分配，但是任然有一些情况下的堆内存分配会让我们感到吃惊。下面让我们分析一些常见的不必要的堆内存分配行为并对其进行优化。
##12.1字符串
 　　在c#中，字符串是引用类型变量而不是值类型变量，即使看起来它是存储字符串的值的。这就意味着字符串会造成一定的内存垃圾，由于代码中经常使用字符串，所以我们需要对其格外小心。

　　c#中的字符串是不可变更的，也就是说其内部的值在创建后是不可被变更的。每次在对字符串进行操作的时候（例如运用字符串的“加”操作），unity会新建一个字符串用来存储新的字符串，使得旧的字符串被废弃，这样就会造成内存垃圾。

　　我们可以采用以下的一些方法来最小化字符串的影响：

　　1）减少不必要的字符串的创建，如果一个字符串被多次利用，我们可以创建并缓存该字符串。

　　2）减少不必要的字符串操作，例如如果在Text组件中，有一部分字符串需要经常改变，但是其他部分不会，则我们可以将其分为两个部分的组件，对于不变的部分就设置为类似常量字符串即可，见下面的例子。

　　3）如果我们需要实时的创建字符串，我们可以采用StringBuilderClass来代替，StringBuilder专为不需要进行内存分配而设计，从而减少字符串产生的内存垃圾。

　　4）移除游戏中的Debug.Log()函数的代码，尽管该函数可能输出为空，对该函数的调用依然会执行，该函数会创建至少一个字符（空字符）的字符串。如果游戏中有大量的该函数的调用，这会造成内存垃圾的增加。

　　在下面的代码中，在Update函数中会进行一个string的操作，这样的操作就会造成不必要的内存垃圾：

```
public Text timerText;
private float timer;
void Update()
{
    timer += Time.deltaTime;
    timerText.text = "Time:"+ timer.ToString();
}
```
通过将字符串进行分隔，我们可以剔除字符串的加操作，从而减少不必要的内存垃圾：

```
public Text timerHeaderText;
public Text timerValueText;
private float timer;
void Start()
{
    timerHeaderText.text = "TIME:";
}
 
void Update()
{
   timerValueText.text = timer.ToString();
}
```
##12.2 Unity3d函数的调用
　　在代码编程中，当我们调用不是我们自己编写的代码，无论是Unity自带的还是插件中的，我们都可能会产生内存垃圾。Unity的某些函数调用会产生内存垃圾，我们在使用的时候需要注意它的使用。

　　这儿没有明确的列表指出哪些函数需要注意，每个函数在不同的情况下有不同的使用，所以最好仔细地分析游戏，定位内存垃圾的产生原因以及如何解决问题。有时候缓存是一种有效的办法，有时候尽量降低函数的调用频率是一种办法，有时候用其他函数来重构代码是一种办法。现在来分析unity中常见的造成堆内存分配的函数调用。

　　在Unity中如果函数需要返回一个数组，则一个新的数组会被分配出来用作结果返回，这不容易被注意到，特别是如果该函数含有迭代器，下面的代码中对于每个迭代器都会产生一个新的数组：

```
void ExampleFunction()
{
    for(int i=0; i < myMesh.normals.Length;i++)
    {
        Vector3 normal = myMesh.normals[i];
    }
}
```
对于这样的问题，我们可以缓存一个数组的引用，这样只需要分配一个数组就可以实现相同的功能，从而减少内存垃圾的产生：

```
void ExampleFunction()
{
    Vector3[] meshNormals = myMesh.normals;
    for(int i=0; i < meshNormals.Length;i++)
    {
        Vector3 normal = meshNormals[i];
    }
}
```
　　此外另外的一个函数调用GameObject.name 或者 GameObject.tag也会造成预想不到的堆内存分配，这两个函数都会将结果存为新的字符串返回，这就会造成不必要的内存垃圾，对结果进行缓存是一种有效的办法，但是在Unity中都对应的有相关的函数来替代。对于比较gameObject的tag，可以采用GameObject.CompareTag()来替代。

　　在下面的代码中，调用gameobject.tag就会产生内存垃圾：

```
private string playerTag="Player";
void OnTriggerEnter(Collider other)
{
    bool isPlayer = other.gameObject.tag == playerTag;
}
```
　　采用GameObject.CompareTag()可以避免内存垃圾的产生：
private string playerTag = "Player";
void OnTriggerEnter(Collider other)
{
    bool isPlayer = other.gameObject.CompareTag(playerTag);
}
不只是GameObject.CompareTag，unity中许多其他的函数也可以避免内存垃圾的生成。比如我们可以用Input.GetTouch()和Input.touchCount()来代替Input.touches，或者用Physics.SphereCastNonAlloc()来代替Physics.SphereCastAll()。

##12.3 装箱操作
　　装箱操作是指一个值类型变量被用作引用类型变量时候的内部变换过程，如果我们向带有对象类型参数的函数传入值类型，这就会触发装箱操作。比如String.Format()函数需要传入字符串和对象类型参数，如果传入字符串和int类型数据，就会触发装箱操作。如下面代码所示：
　　

```
void ExampleFunction()
{
    int cost = 5;
    string displayString = String.Format("Price:{0} gold",cost);
}
```
　　在Unity的装箱操作中，对于值类型会在堆内存上分配一个System.Object类型的引用来封装该值类型变量，其对应的缓存就会产生内存垃圾。装箱操作是非常普遍的一种产生内存垃圾的行为，即使代码中没有直接的对变量进行装箱操作，在插件或者其他的函数中也有可能会产生。最好的解决办法是尽可能的避免或者移除造成装箱操作的代码。
##12.4 协程
　　调用 StartCoroutine()会产生少量的内存垃圾，因为unity会生成实体来管理协程。所以在游戏的关键时刻应该限制该函数的调用。基于此，任何在游戏关键时刻调用的协程都需要特别的注意，特别是包含延迟回调的协程。

　　yield在协程中不会产生堆内存分配，但是如果yield带有参数返回，则会造成不必要的内存垃圾，例如：

```
yield return 0;
```
　　由于需要返回0，引发了装箱操作，所以会产生内存垃圾。这种情况下，为了避免内存垃圾，我们可以这样返回：

```
	
yield return null;
```

　　另外一种对协程的错误使用是每次返回的时候都new同一个变量，例如：

```
while(!isComplete)
{
    yield return new WaitForSeconds(1f);
}
```
　　我们可以采用缓存来避免这样的内存垃圾产生：

```
WaitForSeconds delay = new WaiForSeconds(1f);
while(!isComplete)
{
    yield return delay;
}
```
　　如果游戏中的协程产生了内存垃圾，我们可以考虑用其他的方式来替代协程。重构代码对于游戏而言十分复杂，但是对于协程而言我们也可以注意一些常见的操作，比如如果用协程来管理时间，最好在update函数中保持对时间的记录。如果用协程来控制游戏中事件的发生顺序，最好对于不同事件之间有一定的信息通信的方式。对于协程而言没有适合各种情况的方法，只有根据具体的代码来选择最好的解决办法。

##12.4 foreach 循环
　　在unity5.5以前的版本中，在foreach的迭代中都会生成内存垃圾，主要来自于其后的装箱操作。每次在foreach迭代的时候，都会在堆内存上生产一个System.Object用来实现迭代循环操作。在unity5.5中解决了这个问题，比如，在unity5.5以前的版本中，用foreach实现循环：
　　

```
void ExampleFunction(List listOfInts)
{
    foreach(int currentInt in listOfInts)
    {
        DoSomething(currentInt);
    }
}
```
如果游戏工程不能升级到5.5以上，则可以用for或者while循环来解决这个问题，所以可以改为：

```
void ExampleFunction(List listOfInts)
{
    for(int i=0; i < listOfInts.Count; i++)
    {
        int currentInt = listOfInts[i];
        DoSomething(currentInt);
    }
}
```
##12.4.1 foreach怎么产生垃圾的
　　研究过这个问题的人都应该知道，就是它会引起频繁的GC Alloc。也就是说，使用它之后，尤其在Update方法中频繁调用时，会快速产生小块垃圾内存，造成垃圾回收操作的提前到来，造成游戏间歇性的卡顿。 
　　问题大家都知道，也都给出了建议，就是尽可能不要用。在start方法里倒无所谓，因为毕竟它只执行一次。Update方法一秒钟执行大概50-60次，这里就不要使用了。这个观点整体上是正确的，因为这样做毕竟避开了问题。 
　　不过有一点点不是很方便的就是，foreach确实带来了很多便捷性的编码。尤其是结合了var之后，那么我们究竟还能不能使用它，能使用的话，应该注意哪些问题？带着这些问题，我做了以下的测试。
##12.4.2 重现GC Alloc问题
　　首先，我写了一个简单的脚本来重现这个问题。 
这个类中包括一个int数组，一个泛型参数为int的List。 
　　代码如下：

```
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class ForeachTest : MonoBehaviour {

    int[] m_intArray;
    List<int> m_intList;
    ArrayList m_arryList;
    public void Start () 
    {
        m_intArray = new int[2];
        m_intList = new List<int>();
        m_arryList = new ArrayList();
        for (int i = 0; i < m_intArray.Length; i++)
        {
            m_intArray[i] = i;
            m_intList.Add(i);
            m_arryList.Add(i);
        }
    }

    void Update () 
    {
         testIntListForeach();
    }

    void testIntListForeach()
    {
        for (int i = 0; i < 1000; i++)
        {
            foreach (var iNum in m_intList)
            {
            }
        }
    }
}
```
##12.4.3 应用于IntList的foreach
　　首先我们看应用于泛型List的情况，如下图：
![这里写图片描述](https://img-blog.csdn.net/20170301214543669?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQW5kcmV3RmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/NorthEast)
　　这里确实是在产生GC Alloc，每帧产生39.1KB的新内存。我使用的Unity版本是64位的5.4.3f1，可能不同的版本产生的内存大小有些差别，但是产生新内存是不可避免的。

##12.4.4 应用于IntList的GetEnumerator
　　接下来，我又做了另外一种尝试，就是用对等的方式写出同样的代码。将测试代码部分改成如下：

```
for (int i = 0; i < 1000; i++)
        {
            var iNum = m_intList.GetEnumerator();
            while (iNum.MoveNext())
            {
            }
        }
```
　　原本以为，这个结果与上面的方式应该相同。不过结果出乎意料。
![这里写图片描述](https://img-blog.csdn.net/20170301215737799?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQW5kcmV3RmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/NorthEast)
　　它并没产生任何的新内存。于是，我准备使用IL反编译器来了解它的GCAlloc是如何产生的。 
我们知道，List是动态数组，是可以随时增长、删减的，而int[]这种形式，在C#里面被编译成Array的子类去执行。为了有更多的对比，我将foreach和GetEmulator也写一份同样的代码，应用于Int数组和ArrayList，先查看运行的结果，然后一起查看他们的IL代码。
##12.4.5 应用于IntArray的foreach

```
 for (int i = 0; i < 1000; i++)
        {
            foreach (var iNum in m_intArray)
            {
            }
        }
```
![这里写图片描述](https://img-blog.csdn.net/20170301221537199?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQW5kcmV3RmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/NorthEast)
结果是没有产生GC Alloc。

##12.4.6 应用于IntArray的GetEnumerator

```
  for (int i = 0; i < 1000; i++)
        {
            var iNum = m_intArray.GetEnumerator();
            while (iNum.MoveNext())
            {
            }
        }
```
![这里写图片描述](https://img-blog.csdn.net/20170301221815889?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQW5kcmV3RmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/NorthEast)
结果是这里也在产生GC Alloc，每帧产生31.3KB的新内存。
##12.4.7 应用于ArrayList的foreach

```
for (int i = 0; i < 1000; i++)
        {
            foreach (var iNum in m_intArray)
            {
            }
        }
```
![这里写图片描述](https://img-blog.csdn.net/20170302084901037?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQW5kcmV3RmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/NorthEast)
结果是这里也在产生GC Alloc，每帧产生23.4KB的新内存（在32位版Unity5.3.4f1测试）。
##12.4.8 应用于ArrayList的GetEnumerator

```
 for (int i = 0; i < 1000; i++)
        {
            var iNum = m_intArray.GetEnumerator();
            while (iNum.MoveNext())
            {
            }
        }
```
![这里写图片描述](https://img-blog.csdn.net/20170302084233825?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQW5kcmV3RmFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/NorthEast)
##12.4.9 GC Alloc产生情况小结
<table>
<thead>
<tr>
  <th align="left">小结</th>
  <th align="left">int[] (Array)</th>
  <th align="left">List&lt; int &gt;</th>
  <th align="left">ArrayList</th>
</tr>
</thead>
<tbody><tr>
  <td align="left">foreach</td>
  <td align="left">不产生</td>
  <td align="left">产生</td>
  <td align="left">产生</td>
</tr>
<tr>
  <td align="left">GetEnumerator</td>
  <td align="left">产生</td>
  <td align="left">不产生</td>
  <td align="left">产生</td>
</tr>
</tbody></table>


延伸阅读：
	[【Unity优化】Unity中究竟能不能使用foreach？](https://blog.csdn.net/andrewfan/article/details/59147132)
##12.5 函数引用
 　　函数的引用，无论是指向匿名函数还是显式函数，在unity中都是引用类型变量，这都会在堆内存上进行分配。匿名函数的调用完成后都会增加内存的使用和堆内存的分配。具体函数的引用和终止都取决于操作平台和编译器设置，但是如果想减少GC最好减少函数的引用。

##12.6 LINQ和常量表达式
　　由于LINQ和常量表达式以装箱的方式实现，所以在使用的时候最好进行性能测试。

##13. 重构代码来减小GC的影响
　　即使我们减小了代码在堆内存上的分配操作，代码也会增加GC的工作量。最常见的增加GC工作量的方式是让其检查它不必检查的对象。struct是值类型的变量，但是如果struct中包含有引用类型的变量，那么GC就必须检测整个struct。如果这样的操作很多，那么GC的工作量就大大增加。在下面的例子中struct包含一个string，那么整个struct都必须在GC中被检查：

```
public struct ItemData
{
    public string name;
    public int cost;
    public Vector3 position;
}
private ItemData[] itemData;
```
　　我们可以将该struct拆分为多个数组的形式，从而减小GC的工作量：

```
private string[] itemNames;
private int[] itemCosts;
private Vector3[] itemPositions;
```
　　另外一种在代码中增加GC工作量的方式是保存不必要的Object引用，在进行GC操作的时候会对堆内存上的object引用进行检查，越少的引用就意味着越少的检查工作量。在下面的例子中，当前的对话框中包含一个对下一个对话框引用，这就使得GC的时候会去检查下一个对象框：

```
public class DialogData
{
     private DialogData nextDialog;
     public DialogData GetNextDialog()
     {
           return nextDialog;
                     
     }
}
```
　　通过重构代码，我们可以返回下一个对话框实体的标记，而不是对话框实体本身，这样就没有多余的object引用，从而减少GC的工作量：

```
public class DialogData
{
    private int nextDialogID;
    public int GetNextDialogID()
    {
       return nextDialogID;
    }
}
```
　　当然这个例子本身并不重要，但是如果我们的游戏中包含大量的含有对其他Object引用的object，我们可以考虑通过重构代码来减少GC的工作量。

##14. 定时执行GC操作
主动调用GC操作

 　　如果我们知道堆内存在被分配后并没有被使用，我们希望可以主动地调用GC操作，或者在GC操作并不影响游戏体验的时候（例如场景切换的时候），我们可以主动的调用GC操作：

```
System.GC.Collect()
```
通过主动的调用，我们可以主动驱使GC操作来回收堆内存。



####脚本优化

- 常规循环   尽量少放在Update

- 变量的隐性调用  go.transform不推荐 (这是一种遍历查找组件的方式) 推荐GetComponent(Transform)（Markdown不支持应该是尖角括号）

- Gmaeobject.Find 推荐使用Tag查找 保存变量 

- 多线程
  协程处理，大量循环使用分携程执行，前边添加yield return设置执行顺序
  

```
IEnumerator Work()
    {
        //线程不安全
        //StartCoroutine(MyIoWork1());
        //StartCoroutine(MyIoWork2());
        yield return StartCoroutine(MyIoWork1());
        yield return StartCoroutine(MyIoWork2());
    }

    IEnumerator MyIoWork1()
    {
        for (int i = 0; i < 1000; i++)
        {
            System.IO.File.Delete("c:\a.zip");
            yield return null;
        }

    }

    IEnumerator MyIoWork2()
    {
        for (int i = 0; i < 1000; i++)
        {
            System.IO.File.Delete("c:\a.zip");
            yield return null;
        }

    }
```
- 数学 合理降低计算的精度
例子：
计算距离：Mathf.Sqrt 
计算方向：Vector3.Angle 
Minimize use of complex mathematical operations such as pow, sin and cos in pixel shaders.

```
using UnityEngine;
using System.Collections;

public class TestMagnitude : MonoBehaviour {

    public Transform player1;
    public Transform player2;

    void Start()
    {
        Calc();
    }

    void Calc()
    {
        float distance1 = (player1.position - transform.position).magnitude;
        float distance2 = (player2.position - transform.position).magnitude;
        Debug.Log("Player1 is closer than Player2:" + (distance1 < distance2).ToString());

        float distance11 = (player1.position - transform.position).sqrMagnitude;
        float distance22 = (player2.position - transform.position).sqrMagnitude;
        Debug.Log("Player1 is closer than Player2:" + (distance1 < distance2).ToString());
    }
}
```

```
using UnityEngine;
using System.Collections;

public class Compare : MonoBehaviour {

    public Transform player1;
    public Transform player2;

    // Use this for initialization
    void Start () {

        float angle = Vector3.Angle(player2.forward, player1.forward);
        float dot = Vector3.Dot(player2.forward,player1.forward);

        Debug.Log("Dot=" + dot +" Angle=" + angle);
    }

    // Update is called once per frame
    void Update () {

    }
}
```
- Object Pool 适用于频繁操作的对象

```
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class TestPool : MonoBehaviour
{
    public Transform root;
    public GameObject prefab;

    public float loopCount;
    public float prefabCount;
    List<GameObject> objects;

    // Use this for initialization
    void Start()
    {

        objects = new List<GameObject>();
        System.DateTime startTime = System.DateTime.Now;
        TestCaseWaitOutPool();
        System.DateTime endTime = System.DateTime.Now;
        string totalMs = (endTime - startTime).TotalMilliseconds.ToString();

        Debug.Log("Test case Without Pool take " + totalMs + "ms.");
    }

    // Update is called once per frame
    void Update()
    {

    }

    void TestCaseWaitOutPool()
    {
        for (int j = 0; j < loopCount; j++)
        {
            for (int i = 0; i < prefabCount; i++)
            {
                //create prefab
                GameObject go = Instantiate(prefab);
                //set parent
                go.transform.parent = root;
                //add to list
                objects.Add(go);
            }
            for (int i = 0; i < prefabCount; i++)
            {
                GameObject.Destroy(objects[i]);
            }

            //destory prefab
            objects.Clear();
        }

    }
}
```
使用pool之后

```
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class TestPool : MonoBehaviour
{
    public SimplePool pool;
    public Transform root;
    public GameObject prefab;

    public float loopCount;
    public float prefabCount;
    List<GameObject> objects;

    // Use this for initialization
    void Start()
    {

        objects = new List<GameObject>();
        // with out pool
        System.DateTime startTime = System.DateTime.Now;
        TestCaseWaitOutPool();
        System.DateTime endTime = System.DateTime.Now;
        string totalMs = (endTime - startTime).TotalMilliseconds.ToString();

        Debug.Log("Test case Without Pool take " + totalMs + "ms.");

        // with pool
        System.DateTime poolstartTime = System.DateTime.Now;
        TestCaseWithPool();
        System.DateTime poolendTime = System.DateTime.Now;
        string pooltotalMs = (poolendTime - poolstartTime).TotalMilliseconds.ToString();

        Debug.Log("Test case With Pool take " + pooltotalMs + "ms.");
    }

    void TestCaseWaitOutPool()
    {
        for (int j = 0; j < loopCount; j++)
        {
            for (int i = 0; i < prefabCount; i++)
            {
                //create prefab
                GameObject go = Instantiate(prefab);
                //set parent
                go.transform.parent = root;
                //add to list
                objects.Add(go);
            }
            for (int i = 0; i < prefabCount; i++)
            {
                GameObject.Destroy(objects[i]);
            }

            //destory prefab
            objects.Clear();
        }

    }

    void TestCaseWithPool()
    {
        for (int i = 0; i < loopCount; i++)
        {
            List<GameObject> objectsList = pool.GetObjects((int)prefabCount);
            pool.DestroyObjects(objectsList);
        }

    }
}
```

```
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class SimplePool : MonoBehaviour
{
    public Transform root;
    public GameObject prefab;
    public int size;

    List<GameObject> pooled;

    void Start()
    {
        pooled = new List<GameObject>();
        Prewarm();
    }
    void Prewarm()
    {
        PoolObjects(size);
    }

    List<GameObject> PoolObjects(int _amount)
    {
        List<GameObject> newPooled = new List<GameObject>();
        for (int i = 0; i < _amount; i++)
        {
            GameObject go = Instantiate(prefab);
            go.transform.parent = root;
            go.SetActive(false);
            newPooled.Add(go);
        }
        pooled.AddRange(newPooled);
        return newPooled;
    }

    public List<GameObject> GetObjects(int _amount)
    {
        List<GameObject> pooledObjects = pooled.FindAll(_go => !_go.activeSelf);
        if (pooledObjects.Count < _amount)
        {
            List<GameObject> newObjects = PoolObjects(_amount - pooledObjects.Count);
            pooledObjects.AddRange(newObjects);
            foreach (var go in pooledObjects)
            {
                go.SetActive(true);
            }
            return pooledObjects;
        }
        else
        {
            foreach (var go in pooledObjects)
            {
                go.SetActive(true);
            }
            return pooledObjects;
        }
    }

    public void DestroyObjects(List<GameObject> objects)
    {
        for (int i = 0; i < objects.Count; i++)
        {
            objects[i].SetActive(false);
        }
    }
}
```
Test case Without Pool take 226.1487ms. 
UnityEngine.Debug:Log(Object) 
TestPool:Start() (at Assets/TestPool.cs:26) 
Test case With Pool take 58.0407ms. 
UnityEngine.Debug:Log(Object) 
TestPool:Start() (at Assets/TestPool.cs:34)

时间从 226.1487ms 缩短到58.0407ms。这就是效率。


- 删除脚本中为空或不需要的默认方法，尽量少在Update中做事情，脚本不用时把它deactive。

- 如何找到需要优化的代码


- Total 与 Self
在 Unity-Window-Profiler 
Overview 里面的 Total(总的占用包括调用其他人的部分)，Self(仅自身占用)

```
using UnityEngiusing System.Collections;

public class TestTime : MonoBehaviour {

    public GameObject prefab;

    void Start()
    {
        //self
        System.Threading.Thread.Sleep(2000);

        Create(); // others
    }

    void Create()
    {
        for (int i = 0; i < 10000; i++)
        {
            GameObject go = GameObject.Instantiate(prefab);
            GameObject.Destroy(go);
        }
    }
}
```
####文件优化
1.AssetBundle 创建 读取
设置Prefab。 
AssetBundle New:env/go 路径自行设置

```
using UnityEngine;
using System.Collections;
using System;

public class TestAssetBundle : MonoBehaviour {

    public string path;
    public string file;

    // Use this for initialization
    void Start () {

        StartCoroutine( Load());
    }

    IEnumerator Load()
    {
        string _path = "file:///" + Application.dataPath + path;
        WWW www = WWW.LoadFromCacheOrDownload(_path,1);
        yield return www;

        AssetBundle bundle = www.assetBundle;
        AssetBundleRequest request = bundle.LoadAssetAsync(file);
        yield return request;

        GameObject prefab = request.asset as GameObject;
        Instantiate(prefab);

        //Clean
        bundle.Unload(false);
        www.Dispose();
    }

}
```
2.移动端打包优化
缩减包体积 
设置 Andriod player setting Optimlzation .NET2.0(完整版) .NET2.0 Subset(简化版) 
Stripping Leve Disabled .Strip Assembies. Strip Byte Code(一般用这个就可以了) .Use Micro mscorlib

```
1.momo version 
    full 
    subset 
2.stripping level 
    disabled 
    strip bute code 
3.媒体文件 
    图片 psd/png/jpg 
    音频 ogg/mp3/wav 
    fbx 公用animationclip
```
如何找到打包出的资源占比 
先进行打包，打包完毕后查看控制台输出 
![这里写图片描述](https://img-blog.csdn.net/20171212172036035?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3dsY3NkbjAwMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
打开文本日志，查看各资源的占比，确定优化的方向（此处是空场景测试） 
![这里写图片描述](https://img-blog.csdn.net/20171212172047183?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3dsY3NkbjAwMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
3.跨平台开发效率优化
- 节省时间 utomate 自动化打包插件
- 显示面板改进 Odin - Inspector and Serializer
- DebugConsole Editor Console Pro

![这里写图片描述](https://img-blog.csdn.net/20171211224051209?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3dsY3NkbjAwMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

###二、GPU优化
####顶点计算优化

- 尽可能在制作时控制顶点数目
- 移除那些不需要的硬边缘和UV缝接
- 使用Unity的LODGroup
- 调整视椎体裁剪Frustum culling
- 使用遮蔽剔除Occlusion culling

####片元计算优化
- 尽可能减少复杂的片元计算，如实时光照和实时阴影
- 使用光照贴图实现全局光照，light probes实现简易的动态阴影
- 使用更加高效的Shader（Unity上的Mobile版本）
- 减少半透明物体，并控制它们的渲染顺序，尽可能减少重绘（OverDraw）
###三、带宽（资源）优化
####纹理贴图优化
- 图集/材质/Mesh合并
优化图集
- 纹理压缩格式
   在ios设备上，请使用PVR格式。wp8和win8设备上，DXT格式。android设备，不透明贴图选择通用支持的ETC1格式；而透明贴图，4大GPU厂商各自有自己的压缩格式，可以把贴图分成一张RGB图和一张alpha通道图，都用ETC1格式，游戏里再合成；也可以简单选择RGBA4444格式。
- 合并纹理（Atlas）
虽然批处理是个很好的方式，但很容易就打破它的规定。例如，场景中的物体都使用Diffuse材质，但它们可能会使用不同的纹理。因此，尽可能把多张小纹理合并到一张大纹理（Atlas）中是一个好主意。
- 利用网格的顶点数据
但有时，除了纹理不同外，还有对于不同的物体，它们在材质上还有一些微小的参数变化，例如颜色不同、某些浮点参数不同。但铁定律是，不管是动态批处理还是静态批处理，它们的前提都是要使用同一个材质。是同一个，而不是同一种，也就是说它们指向的材质必须是同一个实体。这意味着，只要我们调整了参数，就会影响到所有使用这个材质的对象。那么想要微小的调整怎么办呢？由于Unity中的规定非常死，那么我们只好想些“歪门邪道”，其中一种就是使用网格的顶点数据（最常见的就是顶点颜色数据）。
前面说过，经过批处理后的物体会被处理成一个VBO发送给GPU，VBO中的数据可以作为输入传递给Vertex Shader，因此我们可以巧妙地对VBO中的数据进行控制，从而达到不同效果的目的。一个例子是，还是之前的森林，所有的树使用了同一种材质，我们希望它们可以通过动态批处理来实现，但不同树的颜色可能不同。这时我么可以利用网格的顶点数据来调整。具体方法，可以参见后面会写的一篇文章。
但这种方法的缺点就是会需要更多的内存来存储这些用于调整参数用的顶点数据。没办法，永远没有绝对完美的方法。

- 减少纹理大小
之前提到过，使用Texture Atlas可以帮助减少Draw Calls，而这些纹理的大小同样是一个需要考虑的问题。在这之前要提到一个问题就是，所有纹理的长宽比最好是正方形，而且长度值最好是2的整数幂。这是因为有很多优化策略只有在这种时候才可以发挥最大效用。
Unity中查看纹理参数可以通过纹理的面板：
![这里写图片描述](http://img.blog.csdn.net/20141226160748390?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
而调整参数可以通过纹理的Advance面板：
![这里写图片描述](http://img.blog.csdn.net/20141226160826664?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
上面各种参数的说明可以参见文档。其中和优化相关的主要有“Generate Mip Maps”、“Max Size”和“Format”几个选项。
“Generate Mip Maps”会为同一张纹理创建出很多不同大小的小纹理，构成一个纹理金字塔。而在游戏中可以根据距离物体的远近，来动态选择使用哪一个纹理。这是因为，在距离物体很远的时候，就算我们使用了非常精细的纹理，但肉眼也是分辨不出来的，这种时候完全可以使用更小、更模糊的纹理来代替，而这大量可以节省访问的像素的数目。但它的缺点是，由于需要为每一个纹理建立一个图像金字塔，因此它会需要占用更多的内存。例如上面的例子，在勾选“Generate Mip Maps”前，内存占用是0.5M，而勾选了“Generate Mip Maps”后，就变成了0.7M。除了内存的占用以外，一些时候我们也不希望使用Mipmaps，例如GUI纹理等。我们还可以在面板中查看生成的Mip Maps：
![这里写图片描述](http://img.blog.csdn.net/20141226212907562?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
Unity中还提供了查看场景中物体的Mip Maps的使用情况。更确切的说是，展示了物体理想的纹理大小。其中红色表示这个物体可以使用更小的纹理，蓝色表示应该使用更大的纹理。
![这里写图片描述](http://img.blog.csdn.net/20141226213035909?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
“Max Size”决定了纹理的长宽值，如果我们使用的纹理本身超过了这个最大值，Unity会对其进行缩小来满足这个条件。这里再重复一点，所有纹理的长宽比最好是正方形，而且长度值最好是2的整数幂。这是因为有很多优化策略只有在这种时候才可以发挥最大效用。
“Format”负责纹理使用的压缩模式。通常选择这种自动模式就可以了，Unity会负责根据不同的平台来选择合适的压缩模式。而对于GUI类型的纹理，我们可以根据对画质的要求来选择是否进行压缩，具体可以参见之前关于画质的文章。
我们还可以根据不同的机器来选择使用不同分辨率的纹理，以便让游戏在某些老机器上也可以运行。

####帧优化
- 限帧
    在移动设备上，Unity默认是60帧/秒，建议关掉垂直同步，把FPS限为30，进入后台时为1。限帧可以显著的减少发热和耗电。
    
####资源优化 
- 资源预加载
    将需要的资源预先加载完成，做一个过渡的页面，效果会好很多
- 资源卸载
    每30秒自动进行一次资源卸载（UnloadUnusedAssets），有时还会触发垃圾收集（GC.Collect）。改为进、出战场时卸载未被引用的资源，而战斗中不再定时卸载，解决了偶尔卡顿的问题。


###四、Unity3d自带的优化技术
####优化几何体
   这一步主要是为了针对性能瓶颈中的”顶点处理“一项。这里的几何体就是指组成场景中对象的网格结构。
3D游戏制作都由模型制作开始。而在建模时，有一条我们需要记住：尽可能减少模型中三角形的数目，一些对于模型没有影响、或是肉眼非常难察觉到区别的顶点都要尽可能去掉。例如在下面左图中，正方体内部很多顶点都是不需要的，而把这个模型导入到Unity里就会是右面的情景：
![这里写图片描述](http://img.blog.csdn.net/20141224210509394?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)![这里写图片描述](http://img.blog.csdn.net/20141224211031113?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
在Game视图下，我们可以查看场景中的三角形数目和顶点数目：
![这里写图片描述](http://img.blog.csdn.net/20141224211224046?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
可以看到一个简单的正方形就产生了这么多顶点，这是我们不希望看到的。
同时，尽可能重用顶点。在很多三维建模软件中，都有相应的优化选项，可以自动优化网格结构。最后优化后，一个正方体可能只剩下8个顶点：

![这里写图片描述](http://img.blog.csdn.net/20141224210907497?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)![这里写图片描述](http://img.blog.csdn.net/20141224211439937?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
它对应的顶点数和三角形数目如下：
![这里写图片描述](http://img.blog.csdn.net/20141224211505705?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
等等！这里，你可能要问了，为什么顶点数是24，而不是8呢？美术朋友们经常会遇到这样的问题，就是建模软件里显示的模型顶点数和Unity中的不一样，通常Unity会多很多。谁才是对的呢？其实，它们是站在不同的角度上计算的，都有各自的道理，但我们真正应该关心的是Unity里的数目。
我们这里简单解释一下。三维软件里更多地是站在我们人类的角度理解顶点的，即我们看见的一个点就是一个。而Unity是站在GPU的角度上，去计算顶点数目的。而在GPU看来，看起来是一个的很有可能它要分开处理，从而就产生了额外的顶点。这种将顶点一分为多的原因，主要有两个：一个是UV splits，一个是Smoothing splits。而它们的本质其实都是因为对于GPU来说，顶点的每一个属性和顶点之间必须是一对一的关系。UV splits的产生，是因为建模时，一个顶点的UV坐标有多个。例如之前的立方体的例子，由于每个面都有共同的顶点，因此在不同面上，同一个顶点的UV坐标可能发生改变。这对于GPU来说，这是不可理解的，因此它必须把这个顶点拆分成两个具有不同UV坐标的定顶点，它才甘心。而Smoothing splits的产生也是类似的，不同的时，这次一个顶点可能会对应多个法线信息或切线信息。这通常是因为我们要决定一个边是一条Hard Edge还是Smooth Edge。Hard Edge通常是下面这样的效果（注意中间的折痕部分）：
![这里写图片描述](http://img.blog.csdn.net/20141224213100458?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)![这里写图片描述](http://img.blog.csdn.net/20141224213112720?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
而如果观察它的顶点法线，就会发现，折痕处每个顶点其实包含了两个不同的法线。因此，对于GPU来说，它同样无法理解这样的事情，因此会把顶点一分为二。而相反，Smooth Edge则是下面的情况：
![这里写图片描述](http://img.blog.csdn.net/20141224213352885?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)![这里写图片描述](http://img.blog.csdn.net/20141224213336989?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
对于GPU来说，它本质上只关心有多少个顶点。因此，尽可能减少顶点的数目其实才是我们真正对需要关心的事情。因此，最后一条优化建议就是：移除不必要的Hard Edge以及纹理衔接，即避免Smoothing splits和UV splits。

####LOD
LOD技术有点类似于Mipmap技术，不同的是，LOD是对模型建立了一个模型金字塔，根据摄像机距离对象的远近，选择使用不同精度的模型。它的好处是可以在适当的时候大量减少需要绘制的顶点数目。它的缺点同样是需要占用更多的内存，而且如果没有调整好距离的话，可能会造成模拟的突变。
在Unity中，可以通过LOD Group来实现LOD技术：
![这里写图片描述](http://img.blog.csdn.net/20141226173207281?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)![这里写图片描述](http://img.blog.csdn.net/20141226173313880?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
通过上面的LOD Group面板，我们可以选择需要控制的模型以及距离设置。下面展示了油桶从一个完整网格到简化网格，最后完全被剔除的例子：
![这里写图片描述](http://img.blog.csdn.net/20141226173626104?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)![这里写图片描述](http://img.blog.csdn.net/20141226173638515?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)![这里写图片描述](http://img.blog.csdn.net/20141226173651796?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)![这里写图片描述](http://img.blog.csdn.net/20141226173702280?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

####使用Lightmaps
Lightmaps的很常见的一种优化策略。它主要用于场景中整体的光照效果。这种技术主要是提前把场景中的光照信息存储在一张光照纹理中，然后在运行时刻只需要根据纹理采样得到光照信息即可。
当然与之配合的还有Light Probes技术。风宇冲有一个系列文章讲过，但是时间比较久远，但教程我相信网上有很多。

####使用God Rays
场景中很多小型光源效果都是靠这种方法模拟的。它们一般并不是真的光源产生的，很多情况是通过透明纹理进行模拟。
####减少实时光照
实时光照对于移动平台是个非常昂贵的操作。如果只有一个平行光还好，但如果场景中包含了太多光源并且使用了很多多Passes的shader，那么很有可能会造成性能下降。而且在有些机器上，还要面临shader失效的风险。例如，一个场景里如果包含了三个逐像素的点光源，而且使用了逐像素的shader，那么很有可能将Draw Calls提高了三倍，同时也会增加overdraws。这是因为，对于逐像素的光源来说，被这些光源照亮的物体要被再渲染一次。更糟糕的是，无论是动态批处理还是动态批处理（其实文档中只提到了对动态批处理的影响，但不知道为什么实验结果对静态批处理也没有用），对于这种逐像素的pass都无法进行批处理，也就是说，它们会中断批处理。
例如，下面的场景中，四个物体都被标识成了“Static”，它们使用的shader都是自带的Bumped Diffuse。而所有的点光源都被标识成了“Important”，即是逐像素光。可以看到，运行后的Draw Calls是23，而非3。这是因为，只有“Forward Base”的Pass时发生了静态批处理（这里的动态批处理由于多Pass已经完全失效了），节省了一个Draw Calls，而后面的“Forward Add” Pass，每一次渲染都是一个单独的Draw Call（而且可以看到Tris和Verts数目也增加了）：

这点正如文档中说的：The draw calls for “additional per-pixel lights” will not be batched。原因我不是很清楚，这里有一个讨论，但里面的意思说是对静态批处理没有影响，和我这里的结果不一样，知道原因的麻烦给我留言，非常感谢。我也在Unity论坛里提问里。
我们看到很多成功的移动游戏，它们的画面效果看起来好像包含了很多光源，但其实这都是骗人的。

####遮挡剔除
遮挡剔除是用来消除躲在其他物件后面看不到的物件，这代表资源不会浪费在计算那些看不到的顶点上，进而提升性能。关于遮挡剔除，Unity Taiwan有一个系列文章大家可以看看（需翻墙）：
[Unity 4.3 关于Occlusion Culling : 基本篇](http://unitytaiwan.blogspot.tw/2013/12/unity-43-occlusion-culling.html)
[Unity 4.3 关于Occlusion Culling : 最佳做法](http://unitytaiwan.blogspot.com/2013/12/unity-43-occlusion-culling_26.html)
[Unity 4.3 关于Occlusion Culling : 错误诊断](http://unitytaiwan.blogspot.tw/2014/01/unity-43-occlusion-culling.html)
具体的内容大家可以自行查找。
现在我们来谈像素优化。
####像素优化
像素优化的重点在于减少overdraw。之前提过，overdraw指的就是一个像素被绘制了多次。关键在于控制绘制顺序。
Unity还提供了查看overdraw的视图，在Scene视图的Render Mode->Overdraw。当然这里的视图只是提供了查看物体遮挡的层数关系，并不是真正的最终屏幕绘制的overdraw。也就是说，可以理解为它显示的是如果没有使用任何深度检验时的overdraw。这种视图是通过把所有对象都渲染成一个透明的轮廓，通过查看透明颜色的累计程度，来判断物体的遮挡。
![这里写图片描述](http://img.blog.csdn.net/20141226211843550?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
上图图，红色越是浓重的地方表示overdraw越严重，而且这里涉及的都是透明物体，这意味着性能将会受到很大影响。

####批处理
这方面的优化教程想必是最多的了。最常见的就是通过批处理（Batching）了。从名字上来理解，就是一块处理多个物体的意思。那么什么样的物体可以一起处理呢？答案就是使用同一个材质的物体。这是因此，对于使用同一个材质的物体，它们之间的不同仅仅在于顶点数据的差别，即使用的网格不同而已。我们可以把这些顶点数据合并在一起，再一起发送给GPU，就可以完成一次批处理。
Unity中有两种批处理方式：一种是动态批处理，一种是静态批处理。对于动态批处理来说，好消息是一切处理都是自动的，不需要我们自己做任何操作，而且物体是可以移动的，但坏消息是，限制很多，可能一不小心我们就会破坏了这种机制，导致Unity无法批处理一些使用了相同材质的物体。对于静态批处理来说，好消息是自由度很高，限制很少，坏消息是可能会占用更多的内存，而且经过静态批处理后的所有物体都不可以再移动了。
首先来说动态批处理。Unity进行动态批处理的条件是，物体使用同一个材质并且满足一些特定条件。Unity总是在不知不觉中就为我们做了动态批处理。例如下面的场景：
![这里写图片描述](http://img.blog.csdn.net/20141224215400873?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
这个场景共包含了4个物体，其中两个箱子使用了同一个材质。可以看到，它的Draw Calls现在是3，并且显示Save by batching是1，也就是说，Unity靠Batching为我们节省了1个Draw Call。下面，我们来把其中一个箱子的大小随便改动一下，看看会发生什么：
![这里写图片描述](http://img.blog.csdn.net/20141224215802736?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
可以发现，Draw Calls变成了4，Save by batching的数目也变成了0。这是为什么呢？它们明明还是只使用了一个材质啊。原因就是前面提到的那些需要满足的其他条件。动态批处理虽然自动得令人感动，但它对模型的要求很多：

- 顶点属性的最大限制为900，而且未来有可能会变。不要依赖这个数据。
- 一般来说，那么所有对象都必须需要使用同一个缩放尺度（可以是(1, 1, 1)、(1, 2, 3)、(1.5, 1.4, 1.3)等等，但必须都一样）。但如果是非统一缩放（即每个维度的缩放尺度不一样，例如(1, 2, 1)），那么如果所有的物体都使用不同的非统一缩放也是可以批处理的。这个要求很怪异，为什么批处理会和缩放有关呢？这和Unity背后的技术有关系，有兴趣的可以自行谷歌，比如这里。
- 使用lightmap的物体不会批处理。多passes的shader会中断批处理。接受实时阴影的物体也不会批处理。
上述除了最常见的由于缩放导致破坏批处理的情况，还有就是顶点属性的限制。例如，在上面的场景中我们添加之前未优化后的箱子模型：
![这里写图片描述](http://img.blog.csdn.net/20141226153354981?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
可以看到Draw Calls一下子变成了5。这是因为新添加的箱子模型中，包含了474个顶点，而它使用的顶点属性有位置、UV坐标、法线等信息，使用的总和超过了900。
动态批处理的条件这么多，一不小心它就不干了，因此Unity提供了另一个方法，静态批处理。接着上面的例子，我们保持修改后的缩放，但把四个物体的“Static Flag”勾选上：
![这里写图片描述](http://img.blog.csdn.net/20141224221213156?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
点击Static后面的三角下拉框，我们会看到其实这一步设置了很多东西，这里我们想要的只是“Batching static”一项。这时我们再看Draw Calls，恩，还是没有变化。但是不要急，我们点击运行，变化出现了：
![这里写图片描述](http://img.blog.csdn.net/20141224221513742?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
Draw Calls又回到了3，并且显示Save by batching是1。这就是得利于静态批处理。而且，如果我们在运行时刻查看模型的网格，会发现它们都变成了一个名为Combined Mesh (roo: scene)的东西。这个网格是Unity合并了所有标识为“Static”的物体的结果，在我们的例子里，就是四个物体：
![这里写图片描述](http://img.blog.csdn.net/20141224222505139?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2FuZHljYXQxOTky/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
你可以要问了，这四个对象明明不是都使用了一个材质，为什么可以合并成一个呢？如果你仔细观察上图的话，会发现里面标明了“4 submeshes”，也就是说，这个合并后的网格其实包含了4个子网格，也就是我们的四个对象。对于合并后后的网格，Unity会判断其中使用同一个材质的子网格，然后对它们进行批处理。
但是，我们再细心点可以发现，我们的箱子使用的其实是同一个网格，但合并后却变成了两个。而且，我们观察运行前后Stats窗口中的“VBO total”，它的大小由241.6KB变成了286.2KB，变大了！还记得静态批处理的缺点吗？就是可能会占用更多的内存。文档中是这样写的：
“Using static batching will require additional memory for storing the combined geometry. If several objects shared the same geometry before static batching, then a copy of geometry will be created for each object, either in the Editor or at runtime. This might not always be a good idea – sometimes you will have to sacrifice rendering performance by avoiding static batching for some objects to keep a smaller memory footprint. For example, marking trees as static in a dense forest level can have serious memory impact.”
也就是说，如果在静态批处理前有一些物体共享了相同的网格（例如这里的两个箱子），那么每一个物体都会有一个该网格的复制品，即一个网格会变成多个网格被发送给GPU。在上面的例子看来，就是VBO的大小明显增大了。如果这类使用同一网格的对象很多，那么这就是一个问题了，这种时候我们可能需要避免使用静态批处理，这意味着牺牲一定的渲染性能。例如，如果在一个使用了1000个重复树模型的森林中使用静态批处理，那么结果就会产生1000倍的内存，这会造成严重的内存影响。这种时候，解决方法要么我们可以忍受这种牺牲内存换取性能的方法，要么不要使用静态批处理，而使用动态批处理（前提是大家使用相同的缩放大小，或者大家都使用不同的非统一缩放大小），或者自己编写批处理的方法。当然，我认为最好的还是使用动态批处理来解决。
有一些小提示可以使用：
- 尽可能选择静态批处理，但得时刻小心对内存的消耗。
- 如果无法进行静态批处理，而要使用动态批处理的话，那么请小心上面提到的各种注意事项。例如：
- 尽可能让这样的物体少并且尽可能让这些物体包含少量的顶点属性。
- 不要使用统一缩放，或者都使用不同的非统一缩放。
- 对于游戏中的小道具，例如可以捡拾的金币等，可以使用动态批处理。
- 对于包含动画的这类物体，我们无法全部使用静态批处理，但其中如果有不动的部分，可以把这部分标识成“Static”。

###五、官方建议
　1.PC平台的话保持场景中显示的顶点数少于200K~3M，移动设备的话少于10W，一切取决于你的目标GPU与CPU。
　2.如果你用U3D自带的SHADER，在表现不差的情况下选择Mobile或Unlit目录下的。它们更高效。
　3.尽可能共用材质。
　4.将不需要移动的物体设为Static，让引擎可以进行其批处理。
　5.尽可能不用灯光。
　6.动态灯光更加不要了。
　7.尝试用压缩贴图格式，或用16位代替32位。
　8.如果不需要别用雾效(fog)
　9.尝试用OcclusionCulling,在房间过道多遮挡物体多的场景非常有用。若不当反而会增加负担。
　10.用天空盒去“褪去”远处的物体。
　11.shader中用贴图混合的方式去代替多重通道计算。
　12.shader中注意float/half/fixed的使用。
　13.shader中不要用复杂的计算pow,sin,cos,tan,log等。
　14.shader中越少Fragment越好。
　15.注意是否有多余的动画脚本，模型自动导入到U3D会有动画脚本，大量的话会严重影响消耗CPU计算。
　16.注意碰撞体的碰撞层，不必要的碰撞检测请舍去。

　　延伸阅读:
　　　　· [Optimizing Graphics Performance  ](http://docs.unity3d.com/Documentation/Manual/OptimizingGraphicsPerformance.html)
　　　　·  [[Unity3D]图形渲染优化、渲染管线优化、图形性能优化](http://blog.sina.com.cn/s/blog_5b6cb9500101dmh0.html)
　　　　· [深入浅出聊优化：从Draw Calls到GC](http://www.cnblogs.com/murongxiaopifu/p/4284988.html)