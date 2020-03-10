---
layout: post
category: Unity3D-Plugin
title: 【Unity3D插件】RuntimeTransformGizmos插件使用教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前文
&#8195;&#8195;Runtime Transform Gizmos看名字顾名思义就是一个可以让Unity在运行的时候，控制物体位置方向缩放的小工具。
&#8195;&#8195;Runtime Transform Gizmos是一个脚本API，它可以让你在游戏中转换对象，能够在游戏中直观和专业的转换对象是非常有用的，特别是当你在运行时编辑器或游戏中，使用者可以移动、旋转和缩放对象。想要做一个模型工具吗？你肯定需要一些方法让他们在场景中操作对象，这个插件将可以完成。
&#8195;&#8195;因为网上的教程特别少，所以特意编写这篇文章，如果能给大家带来点帮助，请大家给我点个赞哦。

Asset Store中的截图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110105400888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

官方Demo中的截图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110105440563.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019011010552068.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110111956427.gif)

## 二、下载地址
1. 可以直接在Asset Store中下载导入，30美金，如果想要追求原创，并且可以想要追求最新版的话可以从官方下载
2. https://download.csdn.net/download/q764424567/10908107 如果只是学习的话，就从这个链接用免费的积分下载就行了 。

## 三、正文
### 1、 导入
将压缩包解压之后得到
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110110412353.png)
拖入到Unity中就可以了 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110110430662.png)

### 2、官方Demo
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110110544920.png)
只是一些简单的模型，没有特别的地方
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110110616102.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
重点

如何去看官方Demo呢
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110113056623.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
官方Demo大部分場景都會比較簡單
所以主要就是看一下场景中都有哪些东西
灯光就不说了，Parent是模型，没有加载脚本，所以主要就是这个<font color="red">(Singleton)RTEditor.RuntimeEditorApplication</font>这个对象了。

接下来就主要讲一下这个对象


### 3、<font color="red">(Singleton)RTEditor.RuntimeEditorApplication</font>对象
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110111241125.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

### 4、工具使用步骤
新建场景
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110113811711.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
初始化
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110113830862.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110114011318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
新建父对象，然后新建一些模型
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110114255326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
保存场景然后运行
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190110114400766.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
已经可以正常运行了


### 5、快捷键
本章会列出默认的快捷键，注意:包含LCTRI的热键。在游戏视图中不能正常工作，你需要添加SHIFT键才能使它们正常工作。默认的快捷键也可以通过定制键映射来改变快捷键。
<table>
<tr><td>快捷键</td><td>作用</td></tr>
<tr><td>W</td><td>激活位置变换工具</td></tr>
<tr><td>E</td><td>激活旋转工具</td></tr>
<tr><td>R</td><td>激活缩放工具</td></tr>
<tr><td>U</td><td>激活体积缩放工具</td></tr>
<tr><td>Q</td><td>关闭装置，这可以使没有一个工具在活动</td></tr>
<tr><td>G</td><td>激活世界坐标轴</td></tr>
<tr><td>L</td><td>激活本地坐标轴</td></tr>
<tr><td>P</td><td>变换中心点和网格轴心点</td></tr>
<tr><td>F</td><td>相机对焦到当前选中的物体</td></tr>
<tr><td>空格+ 鼠标滑轮</td><td>修改网格Y的位置</td></tr>
<tr><td>空格+ 左CTRL + 鼠标滑轮</td><td>比例更大的修改网格Y的位置</td></tr>
<tr><td>左CTRL + D </td><td>复制对象</td></tr>
<tr><td>V </td><td>Translation模式下，执行模型顶点捕捉</td></tr>
<tr><td>B </td><td>Translation模式下，执行BoxCollider顶点捕捉</td></tr>
<tr><td> 左CTRL  </td><td>Translation模式下，执行顶点移动的时候以更大的比例移动</td></tr>
<tr><td>  SHIFT  </td><td>Translation模式下，当按住时，允许你沿着相机的右侧和轴向上平移;</td></tr>
<tr><td> 空格</td><td>Translation模式下，地形(或网格，如果没有地形盘旋)表面位置与Y轴对齐</td></tr>
<tr><td> 空格+X</td><td>Translation模式下，地形(或网格，如果没有地形盘旋)表面位置与X轴对齐</td></tr>
<tr><td> 空格+Z</td><td>Translation模式下，地形(或网格，如果没有地形盘旋)表面位置与Z轴对齐</td></tr>
<tr><td> 左CTRL + 空格</td><td>Translation模式下，地形(如果没有地形盘旋，则使用网格)没有轴向对齐的地表位置</td></tr>
<tr><td>  左ALT </td><td>Translation模式下，激活移动比例(可以在gizmo检查器中指定比例值)</td></tr>
<tr><td>  左 CTRL </td><td> Rotation 模式下，执行顶点移动的时候以更大的比例移动</td></tr>
<tr><td>  左 CTRL </td><td> Scale 模式下，执行顶点移动的时候以更大的比例移动</td></tr>
<tr><td>  左 SHIFT </td><td>Scale 模式下，按住不放时，允许您同时沿所有轴执行缩放操作</td></tr>
<tr><td>  左 CTRL </td><td>Volume scale 模式下，执行顶点移动的时候以更大的比例移动</td></tr>
<tr><td>  左SHIFT </td><td>Volume scale 模式下，在拖动开始之前按住它，将会使这个小工具从对象的中心缩放</td></tr>
<tr><td>  左CTRL </td><td>多选</td></tr>
<tr><td>  左SHIFT </td><td>取消多选，并选定当前对象</td></tr>
<tr><td>  CTRL/CMD + SHIFT + Z </td><td>撤销</td></tr>
<tr><td>  CTRL/CMD + SHIFT + Y  </td><td>返回</td></tr>
</table>

### 6、小工具Gizmos
在这一章中，我们将看看这些小发明是如何工作的，以及可以为每个小发明修改的所有属性。我们还会讨论变换空间和变换轴心点。除了一些小的例外，所有的小发明都像你在Unity编辑器中可以访问的那些一样。例如，scale gizmo允许您使用一组缩放三角形一次沿两个轴缩放。此外，当您用鼠标悬停其中一个gizmo轴时，该轴的颜色将更改为为所选状态设置的颜色。您不需要按鼠标左键来改变悬停轴的颜色
#### 6.1 Translation Gizmos
可以在场景中移动物体，就像在Unity Editor模式中一样，下面是软件在运行时候的截图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114090527675.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
如何使用呢？

如果单击并拖动其中一个gizmo轴，您将沿着相应的gizmo轴执行转换。这个小发明还有一组正方形。单击并拖动其中一个方块将允许vou同时沿两个轴执行转换。如果你按住Shift，一个正方形将显示在gizmo位置的中心。选择方块时，点击并拖动鼠标，将沿着摄像机的右轴和上轴进行平移。这有点像在屏幕空间中执行翻译。注:move gizmo可用于执行特定的特殊操作，如上面讨论的摄像机轴平移或顶点抓拍等。当执行特殊操作时，会在gizmo的中心出现一个正方形。我把它叫做特殊运算平方或者简称为特殊运算平方

- Translation Gizmos的属性
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114095729227.png)
<font color="red">Gizmo Base Scale</font>：统一的轴的比例，这个比例适用于所有的轴
<font color="red">Preserve Gizmo Screen Size</font>：勾选之后，轴的比例大小不受摄像机远近影响，取消勾线之后，轴的比例大小就跟着摄像机远近进行变化
<font color="red">X,Y,Z Axis Color</font>：可以调整三个轴的颜色
<font color="red">Show X,Y,Z Axis</font>：是否显示X,Y,Z轴
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019011410022678.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

<font color="red">Axis Length</font>：轴长度
<font color="red">Arrow Cone Radius</font>：圆锥体箭头的半径
<font color="red">Arrow Cone Length</font>：圆锥体箭头的长度
<font color="red">Are Arrow Cones Lit</font>：圆锥体箭头是否高亮显示
<font color="red">Multi Axis Square Alpha</font>：整体移动轴的Alpha值
<font color="red">Multi Axis Square Size</font>：整体移动轴的大小
<font color="red">Adjust Multi Axis For Better Visibility</font>：勾选之后整体移动轴的方向会随着摄像机的位置进行调整，以适合可以更方便的点击到
<font color="red">Color Of Special Op Square</font>：在特殊操作的时候显示的轴的颜色(相机轴向平移、顶点捕捉或地形/网格表面放置)
<font color="red">Color Of Special Op Square(Selected)</font>： 当移动到上面说的特殊情况下的颜色
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114110700860.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
<font color="red">Transformable Layers</font>：Transform工具可以控制的层，如果没有勾选将控制不了
<font color="red">Vertex Snap Layers</font>：顶点工具可以控制的层

所有的小发明都有相关的键映射，允许你改变热键，激活一个特定的小发明的行为(例如顶点捕捉，步骤捕捉，表面放置，特殊的op等)。检查器中键映射的显示格式如下图所示:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114111013699.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
举个栗子
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114112623916.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
用于激活带有Z轴的move gizmo的表面布局对齐
<font color="red">Num keys</font>：键值映射，最多有两个值，如果不想用的话可以设成0
<font color="red">Modifiers</font>：组合键使用，需要组合键的勾选Modifiers，比如左Ctrl+Space，就可以勾选LeftControl
<font color="red">Mouse buttons</font>：鼠标组合键

#### 6.2 Rotation Gizmos
旋转小工具允许你在场景中旋转对象，它的行为方式与Unity编辑器相同。下面显示了旋转的屏幕截图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114114848562.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
使用旋转装置旋转装置有3个彩色的圆圈，可以用来绕一个轴旋转。您可以通过单击其中一个圆来绕单个轴旋转，然后开始插入鼠标。正如你在上面的图像中看到的，还有一个外圆(这个外圆包围着旋转的球体，它也稍大一些)。点击这个圆，然后拖动鼠标将允许你围绕摄像机视图矢量旋转。如果你单击虚球上的一个点(不是我们目前讨论的任何组件上的点)，你将能够围绕摄像机向右和向上旋转轴。

旋转小发明支持步骤捕捉，它允许您以指定的步骤值(以度数单位表示)的增量旋转。例如，如果将该步骤值设置为15，则只在累计旋转量为> 15时执行旋转。为了使用阶跃捕捉，您必须保持Ctrl按钮按下，然后像往常一样操作这个小发明。注意:只有当你使用3种颜色的圆圈中的一种时，才可以使用步骤捕捉。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114120003687.png)
<font color="red">Gizmo Base Scale</font>：统一的轴的比例，这个比例适用于所有的轴
<font color="red">Preserve Gizmo Screen Size</font>：勾选之后，轴的比例大小不受摄像机远近影响，取消勾线之后，轴的比例大小就跟着摄像机远近进行变化
<font color="red">X,Y,Z Axis Color</font>：可以调整三个轴的颜色
<font color="red">Show X,Y,Z Axis</font>：是否显示X,Y,Z轴
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114120024367.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
<font color="red">Full Circle X,Y,Z</font>：默认情况下，旋转圆只被绘制一半，勾选之后，绘制整个圆
<font color="red">Rotation Sphere Radiu</font>：控制旋转球的半径大小
<font color="red">Rotation Sphere Color</font>：控制旋转球的颜色
<font color="red">Is Rotation Sphere Lit</font>：勾选之后，渲染的时候会旋转球受灯光影响
<font color="red">Show Rotation Guide</font>：显示一个旋转度数的旋转导轨
<font color="red">Rotation Guide Line Color</font>：改变旋转导轨的颜色
<font color="red">Rotation Guide Disc Color</font>：改变旋转导轨盘的颜色
<font color="red">Rotation Sphere Boundary</font>：如果选中此属性，则旋转的边界将会被绘制
<font color="red">Show Camera Look Rotation Circle</font>：指定是否可以沿着相机视图旋转向量画。注意:如果不是这样。选中后，您将无法沿摄像机视图矢量旋转。
<font color="red">Camera Look Rotation Circle Radius Scale</font> ：允许你对摄像机视旋转圆的半径进行比例尺缩放，使圆变大或变小。注:比例是相对于旋转球的半径。例如，如果旋转球的半径是2。半径是1。5。圆的最终半径是2 15 =3。
<font color="red">Camera Look Rotationn Circle Line Color</font>：允许你控制相机的颜色看起来旋转圆;
<font color="red">Camera Lool Rotation Circle Color (Selected) </font>：一样属性上面所讨论的,但是是当圆被选中时(即鼠标悬停时);
<font color="red">Snap Step Value (In Degress)</font>：这允许您控制步骤，启用步骤拍摄时使用的值。这个值用度数表示使用
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114120034764.png)
<font color="red">Transformable Layers</font>：Transform工具可以控制的层，如果没有勾选将控制不了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114120040903.png)

键映射，跟上面使用方法一样，不再详解

#### 6.3 Scale Gizmos
Scale gizmo的工作方式几乎与Unity编辑器中的缩放小工具相同，下面的图片显示了这个缩放装置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114115130891.png)
使用定标仪这个比例装置有3个彩色轴。单击这些轴并拖动鼠标，将单独对指定的轴执行缩放操作。正如您在上面的图像中看到的，还有3个多轴三角形。点击其中一个三角形，并拖动鼠标，将执行一次沿两个轴的缩放操作。如果你想同时在所有轴上执行缩放操作，你必须按住Shift键，然后拖动鼠标。
Scale gizmo支持步骤捕捉，允许您以指定的世界单位数量的增量进行缩放。例如，如果step值设置为1，则只在累计scale为>=1时执行scale操作。为了使用步骤捕捉，您必须按下ctrl键，然后像往常一样使用Scale gizmo(拖动轴或多轴三角形，或按Shift一次按下所有轴的比例)。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114162252701.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
<font color="red">Axis Length</font>：缩放轴的长度
<font color="red">Scale Box Width，Height，Depth</font>：这个属性控制这个gizmo轴顶端的盒子
<font color="red">Are Scale Boxes Lit</font>：如果选中，缩放框在渲染时会受到灯光的影响。如果你想要一个外观平直的小玩意，你可以取消这个属性;
<font color="red">Adjust Axis Length While Scaling Objects</font>：勾选之后,gizmo轴的长度将缩放以及对象同时进行操作。例如，如果你沿着X轴缩放这个属性被选中，当物体在X轴上变大或变小时，这个小玩意的X轴也会变大或变小;
<font color="red">Multi Axis Triangle Sid</font>：控制多轴三角形的相临边的长度，这可以让三角形变大或变小
<font color="red">Adjust Multi Axis For Better Visibility</font>：如果选中此属性，则多轴三角形将始终根据摄像机视图向量调整其位置，以获得更好的可视性。位置总是以这样一种方式计算:无论当前相机的角度如何，选择这3个三角形中的任何一个都很容易。如果未选中此选项，则三角形将始终位于相同的位置，对于某些摄像机角度，可能难以选择某些三角形;
<font color="red">Adjust Multi Axis Triangles While Scaling Objects</font>：在缩放对象时调整多轴三角形如果选中此选项，则多轴三角形的面积将与缩放操作中涉及的对象一起缩放。例如，如果你沿着X轴和Y轴缩放这个属性被选中，当物体在X轴和Y轴上变大或变小时，这个小玩意的XY三角形也会变大或变小;
<font color="red">Color Of All-Axes Square Lines</font>：控制线条的颜色，当你想要同时缩放所有轴时出现的方块
<font color="red">Color Of All-Axes Square Lines(Selected)</font>：跟上面的属性一致,但它适用于被选中(即鼠标悬停的时候)
<font color="red">Screen Size Of All-Axes Square</font> ：允许你控制哪个方块的屏幕大小，当你想要同时执行三个轴上的缩放操作时出现
<font color="red">Adjust All-Axes Square While Scaling Objects</font>：如果选中此选项，则允许您同时沿所有3个轴缩放的正方形的面积将与缩放操作中涉及的对象一起缩放。
<font color="red">Draw Objects Local Axes While Scaling</font>：如果选中此属性，则在执行缩放操作时，将绘制缩放操作所涉及的对象的本地轴。注意:这些轴的颜色将与用于绘制gizmo轴的颜色相同。
<font color="red">Objects Local Axes Length</font>：这个属性允许你控制obiject当地轴的长度时呈现。此属性仅适用于检查缩放时绘制对象的局部轴。
<font color="red">Preserve Axes Screen Size</font>：如果勾选这个属性,对象的大小当地轴线将保持大致相同的不管多远的对象是相机。否则，当它们靠近或远离相机时，线条就会变大或变小。此属性与一般讨论的保持小发明屏幕尺寸属性相同
<font color="red">Adjust Object Local Axes While Scaling</font>：如果这是勾选时,物体的长度本地线将被对象参与缩放管理。例如,如果你是沿着X和Y轴缩放和检查这个属性,作为对象变得更大沿着X轴和Y轴或更小,当地X和Y轴的对象还成为规模更大或更小
<font color="red">Snap Step Value(In World Units)</font>：这允许您控制在启用步骤捕捉时使用的步骤值。该值以世界单位表示

#### 6.4 Volume Scale Gizmos
volume scale gizmo是另一种gizmo类型，它允许您执行对象缩放
它的工作原理和标准的缩放小工具有点不同。，而不是使用可以拖动的轴
gizmo的工作方式与Unity提供给你的box collider小部件几乎相同
修改盒子对撞机。这与步骤捕捉功能结合起来可以提供很多功能
在某些场景中更直观的缩放界面。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114115559862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
这个小发明由6个拖动手柄组成，可以拖动它们来缩放对象。有两个拖动
每个轴的句柄。拖动其中一个将沿相应的对象本地轴缩放。
默认情况下，当您拖动时，gizmo的行为将类似于Unity Box Collider小部件，这意味着
物体的大小和位置都会受到影响。
在拖动之前按住SHIFT键将导致从对象的中心开始缩放。
这个键可以从gizmo的检查器中修改。注意:音量缩放小发明只有当一个对象被选中，并且该对象必须有amesh连接到它时才能工作。当选择多个对象时，gizmo将被隐藏

默认情况下，如果在拖动手柄时按住LCTRL, gizmo将缩放对象
指定步骤大小的增量(可以在检查器中修改)。启用的快捷键
也可以从检查器更改比例。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114162325589.png)
<font color="red">Line Color</font>：构成这个小发明盒子的线条的颜色。默认情况下不能用太小的alpha值，这样做的话不会完全覆盖对象选择框行。修改这个颜色可能意味着你还需要修改对象选择框线条的颜色，这样两个元素就可以很好地组合在一起;
<font color="red">Drag Handle Size (In pixels)</font>：拖动手柄的像素大小;
<font color="red">Snap Step(In World Units)</font>：当snap处于活动状态时，它控制snap step的增量。
注意:snap增量是用世界单位指定的。例如，如果这个值设置为2,gizmo将缩放对象，使其大小以2个世界单元的增量增加/减少

### 7、RunTime编辑器子系统
#### 7.1、(Singleton)RTEditor.RuntimeEditorApplication
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190115085311857.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
<font color="red">Enable Undo/Redo</font>：是否启用撤销/恢复
<font color="red">Use Custom Camera</font>：是否使用自己的自定义摄像机，注意：使用自己的摄像机之后，视角移动和对焦功能就失效了
<font color="red">Use Unity Colliders</font>：使用Unity碰撞器，如果这是选中的，你需要将碰撞器附加到游戏对象，以便能够与他们互动(例如选择，表面放置，顶点捕捉等)。如果没有选中，系统将使用自己的API来处理对象交互。这样做的好处是你不需要将任何碰撞器附加到你的游戏对象上。注意:如果你使用的网格分辨率很高，我建议你检查这个选项。在这种情况下，您可能会在应用程序启动时体验到较慢的帧速率。另一方面，如果您使用的是sprite对象，则需要取消勾选，否则你将无法与它们进行交互。
<font color="red">Light/Particle/Empty/ Object Volume Size</font>：光/粒子系统/空物体体积大小，只有在使用Unity碰撞器时才可见。在这种情况下，该工具现在使用自己的内部对象表示，这使得与场景对象进行交互成为可能。这3个值允许您定义光、粒子系统和空对象的体积大小。空对象是没有网格、地形、精灵或粒子系统组件的对象。如果为这些对象类型之一启用了选择，系统将使用这个卷大小来定义对象的边界框，以确定对象是否被选中(通过单击或选择矩形)
<font color="red">XZ Grid Settings</font> 下面是对网格的设置
<font color="red">Is Visible</font>：允许您指定网格是否应该在场景中呈现
<font color="red">Line Color</font> ：网格线的颜色
<font color="red">Cell Size X</font>：X轴上的网格的单元格大小
<font color="red">Cell Size Z</font>：Z轴上的网格的单元格大小
<font color="red">Y Position</font>：网格在Y轴上的位置
<font color="red">Y Offset Scroll Sensitivity</font>：Y轴的偏移滚动灵敏度，当使用鼠标滚轮调整网格的高度时，此属性用于指定网格对鼠标滚轮的敏感度;注意:此属性仅适用于没有使用Snapping/Steping的情况
<font color="red">Y Step</font>：Y轴阶跃值，当网格的Y位置被修改时使用Snapping/Steping启用。
#### 7.2、(Singleton)RTEditor.EditorGizmoSystem
此对象的名称在首次创建时设置为(singleton) RTEditorEditorGizmoSystem，但如果您愿意，可以更改它。它表示管理所有gizmo对象的系统。它允许你在不同的gizmo类型之间切换，它计算gizmo对象的位置和方向，它还允许你改变gizmo变换空间和变换轴点
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190115142735911.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
<font color="red">Translation Gizmo</font>：这是Translation Gizmo，可以用来在场景中平移物体，这是初始化系统时自动设置的
<font color="red">Rotation Gizmo</font>：这是Rotation Gizmo，可以在场景中旋转物体，这是初始化系统时自动设置的
<font color="red">Scale Gizmo</font>：这是Scale Gizmo，可以在场景中缩放物体，这是初始化系统时自动设置的
<font color="red">Volume Scale Gizmo</font>：这是Volume Scale Gizmmo，可以在场景中以BoxCollider的形式修改物体，这是初始化系统时自动设置的
<font color="red">Active Gizmo Type</font>：初始化的时候，默认激活的Gizmo
<font color="red">Translation；Rotation；Scale VolumeScale</font>：打开或者关闭不同类型的Gizmo

组件下面是一些键映射，在前面都解释过了，就不再重复了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190115144357772.png)

#### 7.3、(Singleton)RTEditor.EditorObjectSelection
此对象的名称设置为(Singleton)RTEditor.EditorObjectSelection，这是系统初始化的时候设置的，可以更改。
这个系统可以去对象进行选择，该系统可以：

- 点击对象进行选择或者添加到当前选择中
- 在按下鼠标左键时移动鼠标，使用选择形状选择对象
- 使用一系列快捷键来控制对象是否被添加或者删除

注意:如果使用Unity Colliders在RuntimeEditorApplication Inspector中被选中，你将只能选择带有collider的对象。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019011514441585.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
<font color="red">Restrications</font> 限制条件
<font color="red">Can Select Terrain Objects</font>：可以选择地形对象
<font color="red">Can Select Light Objects</font>：可以选择灯光对象
<font color="red">Can Select Particle System Objects</font>：可以选择粒子系统对象
<font color="red">Can Select Sprite Objects</font>：可以选择Sprite对象
<font color="red">Can Select Empty Objects</font>：可以选择空对象。空对象是没有网格、光线、粒子系统、精灵或地形组件的对象;
<font color="red">Can Click-Select</font>：可以单选
<font color="red">Can Multi-Select</font>：可以多选
<font color="red">Selectable Layers</font>：可以被选择的层
<font color="red">Duplicatable Layers</font>：可以被复制的层
<font color="red">Selection Box Render Settings</font>对象选择之后的BoxRender设置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190115160231113.png)
<font color="red">Draw Selection Boxes</font>：画选择框，你可以取消这个如果你不想画的对象选择
<font color="red">Selection Box Style</font>：选择框样式
<font color="red">Selection Box Render Mode</font>：选择框的Render模式
<font color="red">Corner Line Percentage</font>：角线，比例只有当使用这个属性对象选择框风格将角线。它指定了一半的框宽/高/深度的百分比，可以用来渲染角线。例如，0.5的值表示角线将是盒子的一半大小。1的值将使边角线等于盒子的半维;
<font color="red">Selection Box Line Color</font>：选择框线段颜色
<font color="red">Selection Box Size Add</font>：选择框的大小增加，小偏移量添加到选择框大小呈现。必要的，以避免Z战斗。例如，当选择多维数据集时，假设没有使用额外的偏移量，则选择框行像素将与多维数据集像素发生冲突，在选择框行中将出现空白
<font color="red">Selection Rectangle Render Settings</font> 选择矩形渲染设置
<font color="red">Border Line Color</font>：边框线段颜色设置
<font color="red">Fill Color</font>：填充颜色
#### 7.4、(Singleton)RTEditor.EditorCamera
此对象的名称设置为(Singleton)RTEditor.EditorCamera，在系统初始化的时候就自动设置，可以修改这个名字。这个相机可以让你在运行的时候浏览场景，提供基本的旋转，平移和缩放功能。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190115163326862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
Zoom Settings 变焦设置
Is Zoom Enabled：是否启用摄像机变焦
Zoom Mode：摄像机变焦模式
Standard模式
Orthographic Standard Zoom Speed：直角平滑变焦速度，这允许你控制变焦速度时，变焦模式设置为平滑。此值仅在编辑器摄像机设置为正投影时应用。
Perspective Standard Zoom Speed：透视平滑变焦速度，与直角平滑值相同，但它适用于编辑器相机设置为透视
Smooth模式
Orthographic Smooth Value：直角平滑值，当缩放模式设置为平滑时，这允许您控制缩放速度达到0的速度。较小的值产生较长的缩放效果较大的值产生较短的缩放效果。此值仅适用于编辑器摄像机设置为
Perspective Smooth Value：透视平滑值，与正投影平滑值相同，但当Editor相机设置为透视时适用。
注意:当缩放模式设置为标准时，平滑值从检查器中消失，缩放速度属性被替换为标准缩放模式的缩放速度。由于相同的变焦速度在不同的相机类型和变焦模式下工作方式不同，因此根据相机类型和变焦模式设置不同的变焦速度非常有用。
Pan Settings 平移设置
Pan Mode：平移模式 
Standard标准模式
Standard Pan Speed：平移速度
Smooth平滑模式
Note:Time value is approximate.注:时间值为近似值。
Smooth Value：平滑值
Smooth Pan Speed：平滑速度
Invert X Axis：反转平移X轴
Invert Y Axis：反转平移Y轴
Focus Settings对焦设置
Focus Mode：对焦模式
Smooth 平滑模式，相机对焦速度会随着时间推迟缓慢下降
Smooth Focus Time：平滑焦点时间
Focus Distance Time：平滑焦点的间隔时间
Constant Speed 恒速，相机会恒定速度对焦
Constant Focus Speed：恒定速度
Focus Distance Scale：对焦距离刻度，当相机对焦时，将为相机计算一个位置，使其正好位于obiect选择的前面。此属性允许您缩放相机和对象选择之间的距离。较大的值会使相机远离焦点。最小可能的比例是1.0f
Instant 瞬间，相机会瞬间对焦到物体
Focus Distance Scale：对焦距离刻度，当相机对焦时，将为相机计算一个位置，使其正好位于obiect选择的前面。此属性允许您缩放相机和对象选择之间的距离。较大的值会使相机远离焦点。最小可能的比例是1.0f
Rotation Speed(Degrees)：旋转速度(度)，允许您指定的相机旋转速度度数。注意:这个值适用于所有类型的旋转:法线(环顾四周)和轨道。
Move Speed(units/second)：移动速度(单位/秒)，允许您控制相机移动速度,当相机使用WASD和QE键移动。
Background Settings 相机背景设置
Is Visible：设置背景是否可见
Top Color：顶部背景渐变色
Bottom Color：下部背景渐变色
Gradient Offset：渐变偏移,取[- 1,1]区间内的值，它允许您指定一种颜色应该比另一种颜色呈现得更多。例如，较小的值将呈现更多的顶部颜色，较大的值将呈现更多的底部颜色，值1将只呈现顶部颜色。值1将只呈现底部颜色


#### 7.5、(Singleton)RTEditor.EditorUndoRedoSystem
此对象的名称设置为(singleton) RTEditor.EditorUndoRedoSystem，名字可以更改。它负责允许执行撤销和重做
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190115144442869.png)
这里目前只有一个属性，这是一个标记为Action Limit的整数字段。此属性允许指定可撤消或重做的操作的最大数量。
#### 7.6、(Singleton)RTEditor.EditorMeshDatabase
储存网格数据库
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190115144448594.png)
#### 7.7、(Singleton)RTEditor.MessageListenerDatabase
消息侦听器数据库
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190115144454734.png)
#### 7.8、(Singleton)RTEditor.InputDevice
输入设备
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190115144500980.png)
#### 7.9、(Singleton)RTEditor.SceneGizmo
这个包包含一个场景小发明，非常类似于在Unity中实现的，除了一些小的例外:
- 它不支持平滑的透视图转换。透视切换是即时完成的:
- 当点击其中一个圆锥时，相机的位置不会受到影响。只有它的旋转将被调整，使摄像机的外观矢量与相应的轴对齐

 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114171744658.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190114171544650.png)
Corner：设置Scene Gizmo渲染的位置，值有ToplLeft, TopRight, BottomRight和BottomLeft
X,Y,Z Axis Color：设置轴的颜色
Negative Axis Color：负轴的颜色
Cube Color：连接小部件轴锥的立方体的颜色;
Hovered Componet Color：鼠标悬停组件颜色，当鼠标悬停在光标上时，gizmo组件(立方体、圆锥)的颜色;
Camera Look Aligh Duration (Seconds)：相机外观触发的持续时间，以秒为单位。当单击gizmo轴锥时，会发生这种对齐。

### 8、脚本
本章提供了构建应用程序时最需要的脚本信息。最重要的是学习如何注册到应用程序中发生的不同类型的事件(对象选择、gizmo操作等)，以便您的应用程序能够相应地进行操作。
#### 8.1 The IRTEditorEventListener interface
有时候，您可能希望侦听可以分别发送到各个方面的不同类型的事件。由于这个原因，系统公开了IRTEditorEventListener，它可以由所有必须侦听这些事件的 Monobheaviors 程序实现。

界面如下图所示:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116152100778.png)

现在让我们讨论接口公开的每个方法。注意:在讨论完所有方法之后，我们将在最后讨论ObjectSelectEventAres和ObiectDeselectEventArgs。

- Oncanbesselected
当实现接口的对象((Monobehaviour))即将被选中时，由对象选择模块调用。这为您提供了更多的控制，可以选择或不选择对象。如果对象可以被选中，该方法必须返回true，否则返回false:
- OnSelected
选择对象后，选择模块调用;
- Ondesselected
对象被取消选择后，选择模块调用:
- OnAlteredByTransformGizmo 
称为lby gizmo(移动，旋转或缩放)后，它被用来转换一个对象(改变位置，旋转或缩放)

现在让我们讨论ObjectSelectEventArgs和ObjectDeselectEventArgs参数。
下一个图像显示ObjectSelectEventArgs类公开的属性:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116152333758.png)

- Selectactiontype 
- 指定选择对象的方式

- Gizmotvpe 
选择对象时使用的gizmo类型

-  IsGizmoActive 
指定小部件是否活动。它可能被用户通过相应的热键。

下一个图像显示ObjectDeselectEventArgs类公开的属性:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116152810295.png)
在这种情况下，只有第一个属性不同。它被称为DeselectActionType，用于指定对象是如何被取消选择。
下面是一个你可以实现这个接口的例子:

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116152835361.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
因此，只需从Monobehaviour和IRTEditorEventListener派生并实现接口方法。当然，您需要将此脚本附加到所有必须侦听这些事件的对象。最有效的方法是将这个脚本附加到一个对象上，使该对象成为预置对象，并使用该预置实例化场景中的对象(在运行时或编辑器中)
##### 8.1.1 Useful GameObject Extensions
使用IRTEditorEventListener接口，您可以定制对象选择发生的方式。例如，您可能有一组网格对象，它们都附加到一个空的父对象对象。假设允许选择空的游戏对象(可以在编辑器对象选择检查器中选中空对象)，您可能希望将选择委托给这个对象，这样您就可以一次操作所有网格对象。这只是一个例子，但是有很多方法可以定制选择。

这个任务通常需要对所涉及的obiect层次结构进行某种处理。下面的清单显示了一些有用的GameObject extension方法和实用函数，可以帮助你在:

GameObject GetFirstEmptyParent(这个GameObject GameObject)方法将从指定的游戏对象开始向上移动，直到找到一个空的父对象。这可以是一个直接父对象的游戏对象，也可以是层次结构更上层的对象。

注意:当满足以下所有条件时，对象被认为是空的:
它没有网格:
它没有地形;
它没有轻分量:
无粒子系统组件:
无有效精灵渲染器;

#### 8.2 Gizmos
##### 8.2.1 Gizmo events
gizmo对象公开了一些您有时可能需要侦听的事件。它们是GizmoDragStart, GizmoDragUpdate和GizmoDragEnd。所有事件处理程序接受对触发事件的gizmo的引用:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116153556240.png)
因此，监听这些事件很简单，只需创建事件处理程序，然后将它们注册到gizmos即可。为了注册到gizmo事件，您需要访问gizmo对象，您可以这样做。如下所示:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116153611600.png)
在上面的图像中，所有gizmos的拖动开始事件都使用相同的处理程序，但是在这里要记住的重要一点是访问gizmos的方式。您可以订阅另一种类型的事件，但它属于EditorGizmoSystem类。它允许您检测 EditorGizmoSystem 何时发生了更改。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116153639644.png)
订阅此事件可以通过以下代码完成:EditorGizmoSvstem.Instance。ActiveGizmoTypeChanged + = MyHandler;MyHandler必须有一个与上面图像中的委托匹配的原型。处理程序必须接受的唯一参数是新类型的gizmo。
##### 8.2.2 Gizmo Object Masks
有时，指示小部件忽略某些对象或对象层是有用的。为例如，你可能想要移动gizmo忽略场景中的一些树对象。在这种情况下，您可以将树对象分配给move gizmo的掩码。或者，如果树对象已经被分配到一个专用层，那么您可以屏蔽整个对象层。

注意:在使用卷刻度小部件时不支持对象掩码。

下面是用于蒙版/取消蒙版对象和对象层的方法。这些“方法属于‘Gizmo’基类，所以它们可以用于任何Gizmo类型:
• public void MaskObject(GameObject gameObject);
• public void UnmaskObject(GameObject gameObject);
• public void MaskObjectCollection(IEnumerable<GameObject> collection);
• public void UnmaskObjectCollection(Ienumerable<GameObject> collection);
• public bool IsGameObjectMasked(GameObject gameObject);
• public void MaskObjectLayer(int objectLayer);
• public void UnmaskObjectLayer(int objectLayer);
• public void IsObjectLayerMasked(int objectLayer);
• public bool CanObjectBeManipulated(GameObject gameObject)
TranslationGizmo trGizmo = EditorGizmoSystem.Instance.TranslationGizmo;
trGizmo.MaskObject(myObject);
trGizmo.MaskObjectLayer(objectLayer);
trGizmo.MaskObjectCollection(myObjectCollection);
trGizmo.UnmaskObjectCollection(myObjectCollection);
##### 8.2.3 Axes Masks
所有的小发明(除了体积缩放小发明)支持轴掩码和这些掩码应用
对象。例如，你可以告诉移动的gizmo它不能沿着Z轴移动某个物体
轴。axis蒙版函数调用属于Gizmo基类，因此它们可以与所有3个Gizmo一起使用
类型。注意:仅当使用gizmo轴或
multi-axes。这里是一个简短的总结，gizmo组件是受口罩的影响
move gizmo 
rotation gizmo
scale gizm
void SetObjectAxisMask(GameObject gameObj, bool[] axisMask) 
void SetObjectAxisMask(GameObject gameObj, int axisIndex, bool isMasked) 
TranslationGizmo trGizmo = EditorGizmoSystem.Instance.TranslationGizmo;
trGizmo.SetObjectAxisMask(gameObj, new bool[] {true, true, false};
RotatinoGizmo rotGizmo = EditorGizmoSystem.Instance.RotationGizmo;
rotGizmo.SetAxisMask(gameObj, 0, false);
rotGizmo.SetAxisMask(gameObj, 1, true);
rotGizmo.SetAxisMask(gameObj, 2, true)

#### 8.3  Actions
每个可以执行然后撤销或重做的操作(即撤销/重做)都驻留在它自己的类中，该类实现了两个接口:IUndoableAndRedoableAction和IAction。下图显示了派生的操作类型及其实现的接口之间的关系:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116154115236.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
这些接口驻留在以下位置:AssetslRuntime Transform GizmosIScripts Editor Undo Redo SystemlActions。例如，使用实现撤消、重做和执行方法的action类执行对象复制。所有可以撤消/重做的操作都必须包装在自定义操作类中。
##### 8.3.1 Defining An Action
在本章中，我们将看到一个示例，演示如何实现自己的编辑器应用程序可能需要的新操作。让我们假设您已经实现了一个图形用户界面，该界面允许用户更改不同的gizmo属性。我们还假设您已经决定让用户撤销并重做changihg a gizmo轴颜色的操作。注意:如果不需要撤销/重做，则不需要实现action类。
The Gizmo基类包含两个方法，可以帮助你做到这一点:
• public Color GetAxisColor(GizmoAxis axis);
• public void SetAxisColor(GizmoAxis axis, Color color
以下是你需要完成的步骤:
在EditorActions。定义一个新的动作类，它表示gizmo轴颜色更改动作。这个脚本位于以下位置:AssetsRuntime Transform Gizmos ScriptsEditor Undo Redo SystemlActions Editor Actions。这个类的实现可能如下图所示:
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019011615423799.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
现在，为了改变活动部件的X轴的颜色，你需要写
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116154254145.png)
这将改变活动的gizmo的X轴的颜色，它还将允许撤销/重做，如果不需要撤销/重做，就不需要实现action类。直接使用Gizmo属性/方法就可以了
#### 8.4  Scene Management
##### 8.4.1 Game Object Sphere Tree
为了让你独立于碰撞器与对象交互(当使用Unity碰撞器在运行时编辑器应用检查器中未选中时)，该工具使用树形结构来组织场景中的对象。此树必须执行的一个操作是确定对象转换何时发生更改。该系统可以在2架无人机上实现:
手动跟踪对象转换数据(默认)，它工作得非常好;使用变换。对象的属性进行更改，以确定是否更改了任何转换特定的数据。这是最快的，但默认情况下不被激活，因为工具将被设置。这个标记为false，您可能希望在代码的其他部分自己管理这个值。
如果您确定不需要在代码的任何部分手动设置此值，那么可以执行以下步骤来激活此功能:
1. 打开资产IRuntime转换GizmoslScripts场景管理边界卷层次树状球体对象树\GameObjectSphereTree。cs文件;
 2. 取消对文件顶部一行的注释(即:#define USE TRANSFORM已更改);从这个过程中获得的速度不是很显著，但是如果您愿意的话，您可能想要这样做。这两种方法都很好。2. 取消对文件顶部一行的注释(即:#define USE TRANSFORM已更改);从这个过程中获得的速度不是很显著，但是如果您愿意的话，您可能想要这样做。这两种方法都很好。
##### 8.4.2 Picking Mesh Objects
系统将需要访问网格数据，以便允许您选择网格对象。如果网格被标记为不可读，系统将执行以下步骤:
它将检查游戏对象是否附加了网格碰撞器，并使用它来执行拣选操作;
如果没有网格碰撞器，它将使用游戏对象的方块音量进行拾取。
#### 8.5  Object Selection Masks
对象选择机制是在编辑器对象选择中实现的。选择模块是一个单例类，这意味着您可以通过编写如下代码来访问它:EditorObjectSelection.cs 
(Assets\Runtime Transform Gizmos\Scripts\Editor Object Selection\).
##### 8.5.1 Object Selection Masks
您可能希望场景中的一些对象被对象选择模块忽略。如果是这种情况，您可以在运行时将对象分配给对象选择掩码。传送到选择掩码的对象将永远不会被选中。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116154541682.png)
注意:当您向选择掩码中添加对象时，如果这些对象中有任何一个被选中，它们将被自动取消选择同样，你也可以从选择蒙版中删除对象:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116154557259.png)
##### 8.5.2 Manually changing the object selection
大多数情况下，您将让对象选择模块完成它的工作，并使用鼠标通过单击或使用选择矩形来选择对象。然而，有时您可能希望这样做手动更改对象选择。例如，如果您希望实现CTRL+ a fiuncality来选择场景中的所有对象，那么您将需要一种方法来告诉选择模块选择什么对象为此，可以(在EditorObjectSelection类中)使用SetSelectedObjects方法。其类型如下:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116154632221.png)
简单地说，该方法接受您希望选择的obiects的集合和一个boolean参数，该参数允许您指定是否可以撤消/重做选择操作。注意:这个方法会考虑到任何指定的限制(对象选择蒙版，图层蒙版，可以选择网格对象，可以选择地形对象等等)。不通过此筛选器的对象将不会被选中。还有，之前选过的所有片段。如果v不在“selectedObiects”集合中，则将被取消选择。因此，此方法不附加到当前选择。它改变了选择。您还可以通过调用ClearSelection手动清除obiect选择:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116154652402.png)
该参数允许指定清除操作是否可以撤消/重做。
你可能会发现其他有用的方法有:
bool AddObiectToSelection(GameObiect gameObi
添加一个对象到当前选择。第一个参数表示必须添加到选择中的对象，第二个参数指定操作是否可以撤消/重做;
bool RemoveObjectFromSelection((GameObject gameObj, bool allowUndoRedo)
第二个参数指定操作是否可以撤消/重做。
##### 8.5.3 Listening to Selection Changed events
通过在选择模块中注册一个偶处理器，可以监听选择更改事件，如下图所示:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116154759630.png)
处理程序必须具有以下原型:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116154811906.png)
处理程序必须接受的唯一参数是的实例

ObjectSelectionChangedEventArgs. 
现在让我们讨论存储在这个对象中的属性/informatibn:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116154851165.png)
第一个属性是一个名为ObjectSelectActionType的枚举类型，如下所示:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116155013415.png)
它告诉客户端代码，如果有对象被选中，这就是它们被选中的方式。
现在我们来讨论可能的值
• Click – 对象是通过单击鼠标选择的;
• ClickAppend – 对象是通过单击选中的，但是带有选择热键的追加活动的，它导致将对象追加/添加到当前选择项中;
• MultiSelect – 对象是通过选择矩形选择的;
• Undo – 这些对象被选择为撤销操作的一部分。当您更改对象选择(以任何方式、形状或形式)，然后撤销时，就会发生这种情况，这将使对象选择恢复到以前的状态。因此，撤消机制将重新选择以前选择的对象;
• Redo – 与撤销相同，但它适用于重做操作;
• AddObjectToSelectionCall – 对象是通过调用来选择的EditorObjectSelection。实例.AddObjectToSelection;
• SetSelectedObjectsCall – 对象是通过调用来选择的实例是EditorObjectSelection.Instance.SetSelectedObjects
• None – 在某些场景中用于指示没有选择对象

第二个属性是一个集合，它包含所选择的所有对象。一起
选择action type属性，这2个属性可以用来找出哪些对象已经被选中，以及在哪些对象中被选中
的方式。注意:如果SelectActionType属性设置为None，这意味着没有选择对象
在这种情况下，对象集合为空


接下来的两个属性与前两个几乎相同，但是它们提供了关于哪些对象的信息
以什么方式被取消选举。让我们看看ObjectDeselectActionType是什么样的:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190116155311335.png)
• ClearSelectionCall – 对象是通过调用来取消选择的EditorObjectSelection.Instance.ClearSelection;
• SetSelectedObjectsCall – 对象是通过调用来取消选择的
EditorObjectSelection.Instance.SetSelectedObjects。这是因为这个方法根据指定的集合更改对象选择，因此以前当选的，将被取消选举;
• RemoveObjectFromSelectionCall – 通过调用，将对象从选择项中删除
EditorObjectSelection.Instance.RemoveObjectFromSelection; 
• ClearClickAir – 当用户在空中单击时，将取消选择对象。这原因
要被选择模块清除的对象选择;
• ClickAlreadySelected – 当用户单击带有append to的对象时就会发生这种情况
选择热键激活，对象已经被选中。在这种情况下，模块将取消选择它;
• MultiDeselect – 当multideschosen热键处于活动状态时，使用选择矩形取消选择对象;
• Undo – 对象作为撤销操作的一部分被取消选择;
• Redo – 作为重做操作的一部分，对象被取消选择;
• DeselectInactive – 在选择对象之后，有时可能会发生某些情况
变得不活跃。该模块将检查每一帧中不活动的对象，并将它们从选择中删除。当从选择项中删除这些对象时，将使用这种类型的取消选择操作;
• None – 在某些情况下，指示没有取消选择对象是有用的

回到ObjectSelectionChangedEventArgs, DeselectAction'lype和DeselectedObjects告诉你哪些对象以什么方式被取消了。如果DeselectActionType设置为None，这意味着没有对象被取消选择，DeselectedObjects是空的。接下来，我们有一个Gizmolype属性，它告诉您在选择更改时使用的gizmo的类型。最后一个属性IsGizmoActive指定当用户通过相应的热键关闭gizmo时，gizmo是否处于活动状态。
