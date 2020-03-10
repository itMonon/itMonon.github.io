---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Editor自定义窗口、自定义组件、Inspector、菜单等等
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
这是我看到的一片比较完整的讲自定义窗口，自定义组件的教程，讲的比较详细，特意转过来给大家分享一下
## 二、原文
原文地址：https://mp.weixin.qq.com/s/4kporY-PCScRAESy4WSpmA
原文作者：克森
原文出处：微信公众号克森空间
## 三、目标
利用学到的东西制作自己的工具（自定义的窗口、Inspector、菜单等等）。
## 四、正文
### 1、Unity Editor 基础篇（一）：Build-In Attribute
关于 Unity 内置属性可以从到官方文档中查询，本篇文章只介绍一些常用的内置属性，如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704091602282?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接下来进行项目设置，创建一个空的 Unity 工程，名字由你来定，文件夹的层级关系如下：
![这里写图片描述](https://img-blog.csdn.net/20180704091616469?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
目前还不需要Editor文件夹，但是先创建，往后的教程中会用到。然后再 Scripts 文件夹中创建一个新的 C# 脚本，命名为“People”，双击打开该脚本。

####  AddComponentMenu

AddComponentMenu 属性允许将一个脚本添加到 Component 菜单中，然后你便可以通过 Component ->（你设置的名字）为一个选中的游戏对象创建该脚本，如下所示：
![这里写图片描述](https://img-blog.csdn.net/20180704091736264?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704091747396?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
#### RequireComponent
RequireComponent()属性会自动帮你添加你需要的组件，如果已经存在则不再重复添加，且不能移除，如下所示：
![这里写图片描述](https://img-blog.csdn.net/20180704091832443?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704091841679?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
提示：经过测试，我发现一个问题，如果脚本已经挂在物体身上，然后再修改脚本，为添加 RequireComponent 属性的话，完全不起作用，因此建议大家在用此属性的时候要注意。
#### ContextMenu
ContextMenu()属性允许添加一个命令到该组件上，你可以通过右键或者点击设置图标来调用到它（一般用于函数），且是在非运行状态下执行该函数，如下所示：
![这里写图片描述](https://img-blog.csdn.net/20180704091911695?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704091920721?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
#### HelpURL

HelpURL()提供一个自定义的文档链接，点击组件上的文档图标既能打开到你指定的链接，如下所示：
![这里写图片描述](https://img-blog.csdn.net/20180704091947417?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704091959351?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
提示：填写链接时，一定要写上 http:// 或者 https://，否则将无任何反应。
#### Range()、Multiline()、header()
-  Range()属性
用于将一个值指定在一定的范围内，并在Inspector面板中为其添加滑块；
- Multiline()属性
用于给 string 类型添加多行输入；
- header()属性
用于添加属性的标题

具体操作如下所示：
![这里写图片描述](https://img-blog.csdn.net/20180704092128529?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704092141372?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

简单的分解一下：
   1.第9行，我们使用了 [Header("BaseInfo")] 为其设置了标题（为“BaseInfo”），如上图所示。
   
2.第10行，我们使用了 [Multiline(5)] 为其 name 属性添加了5行输入，如上图所示，明显输入框变大了。

  3.第12行，我们使用了 [Range(-2,2)] 为其 age 属性指定了一个（-2，2）的范围，并且为其添加了一个滑块，如上图所示。


#### Tooltip()、Space()
Tooptip（）属性用于在 Inspector 面板中，当鼠标停留在设置了Tooptip（）的属性添加指定的提示；Space（）用于为在 Inspector 面板两属性之间添加指定的距离，如下所示：
![这里写图片描述](https://img-blog.csdn.net/201807040922554?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704092301922?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


----------


### Unity Editor 基础篇（二）：自定义 Inspector 面板

#### 最终效果

![这里写图片描述](https://img-blog.csdn.net/20180704092357835?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 准备工作
还是使用上一篇的 Unity 工程，然后在 Scripts 文件夹里创建一个新的 C# 脚本，命名为“Player”，然后双击打开脚本，然后为其添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704092438215?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Player 类记录了 Player 的一些基础信息，例如：ID、名字、背景故事、生命值、伤害等等。

自定义 Inspector 属性面板的一些基础知识，和注意事项如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704092445383?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

传送门：        http://www.ceeger.com/Script/Editor/Editor.html

接下来开始制作的我们自己的 Inpector，对于自定义 Inpector 面板可参考下图的API：
![这里写图片描述](https://img-blog.csdn.net/20180704092505798?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
传送门：http://www.ceeger.com/Script/EditorGUILayout/EditorGUILayout.BeginVertical.html

#### 常用的自定义 Inspector 界面布局属性
现在，请你在 Editor 文件夹中创建一个新的 C# 脚本，双击就打开该脚本，并为其添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704092718724?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704092724987?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704092731360?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704092737769?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Okey，接下来一一分析一下

##### 1、Vertical - 垂直布局
默认的界面布局就是垂直布局，但是为了节目效果，我们还是把它写上比较好，设置元素为垂直布局需使用这对兄弟来声明：

```csharp
EditorGUILayout.BeginVertical();
EditorGUILayout.EndVertical();
```
在这对兄弟里面做的布局都是以垂直方向来排列的。
![这里写图片描述](https://img-blog.csdn.net/20180704092840395?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
如上图所示，整个页面元素都是以垂直方向来布局的。
##### 2、Horizontal - 水平布局
设置元素为水平布局需使用这对兄弟来声明：

```csharp
EditorGUILayout.BeginHorizontal();
EditorGUILayout.EndHorizontal();
```
在这对兄弟里面做的布局都是以水平方向来排列的。
![这里写图片描述](https://img-blog.csdn.net/20180704092923581?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
由于我们在上图圈选的地方使用了 Horizontal 这对兄弟，因此在这对兄弟里的元素全是以水平方向排列。

##### 3、Space - 空间（空行）
使用 EditorGUILayout.Space() 可在两个元素之间空出一行。
![这里写图片描述](https://img-blog.csdn.net/20180704092949390?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### 4、绘制各种类型字段
绘制字段用到以下几个方法：

```csharp
EditorGUILayout.LabelField（）标签字段
EditorGUILayout.IntField（） 整数字段
EditorGUILayout.FloatField（） 浮点数字段
EditorGUILayout.TextField（） 文本字段
EditorGUILayout.Vector2Field（） 二维向量字段
EditorGUILayout.Vector3Field（） 三维向量字段
EditorGUILayout.Vector4Field（） 四维向量字段
```
它们的规律就是方法名都是以 Field 结尾，大伙们可以根据绘制的类型选择相对应的方法。

一般括号里面的参数，第一个为绘制该字段的名字（string 类型），第二个为绘制该字段的值，如下所示：
![这里写图片描述](https://img-blog.csdn.net/2018070409304027?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704093048575?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
1 为标签字段
2 为整形字段
3 为文本字段
4.为文本区域

##### 5、滑块、进度条
滑块：EditorGUILayout.Slider（）
![这里写图片描述](https://img-blog.csdn.net/20180704093124410?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704093130525?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
EditorGUILayout.Slider（）用于绘制一个滑块，从上可知：

第一个参数是滑块的名字
第二个参数是滑块要改变的值
第三和第四个参数是滑块的范围

效果如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704093151936?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
进度条：EditorGUI.ProgressBar()
![这里写图片描述](https://img-blog.csdn.net/20180704093201752?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704093238400?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704093249766?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
EditorGUI.ProgressBar（）用于绘制一个进度条，从上可知：

第一个参数是设置进度条的大小，类型是一个 Rect。
第二个参数是设置显示的值，
第三个参数是设置进度条的名字

提示：
    1.第一个参数，我们使用了 GUILayoutUtility.GetRect（） 工具类的 GetRect（）方法返回一个设置好的矩形框，在案例里我们设置了一个 50*50 大小的矩形框。

   2.第二个参数，我们使用 player.health / 100.0f。那是因为进度条的最大值为1，如果不除100的话，当滑块的值为1时，进度条便填满了，因此我们想让值与进度条的比例同步，那就除100吧（语文不好，不知道解释得如何）。

效果图：
![这里写图片描述](https://img-blog.csdn.net/20180704093315674?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


##### 6、帮助框
帮助框：EditorGUILayout.HelpBox（）
![这里写图片描述](https://img-blog.csdn.net/20180704093504895?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/2018070409351113?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
EditorGUILayout.HelpBox（）用于绘制一个盒子（也可以看作矩形框），然后再盒子的里面显示提示信息，从上图可知：

第一个参数是传入提示信息
第二个参数是提示信息的类型


效果图：
![这里写图片描述](https://img-blog.csdn.net/20180704093533136?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
错误类型
![这里写图片描述](https://img-blog.csdn.net/20180704093542711?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
正常类型
![这里写图片描述](https://img-blog.csdn.net/20180704093555455?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
警告类型


----------

### 基础篇（三）：自定义窗口
#### 最终效果
![这里写图片描述](https://img-blog.csdn.net/20180704093659312?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
#### 准备工作
在之前的项目中，找到 Editor 文件夹，然后创建一个新的 C# 脚本，命名为“MyFirstWindow”，然后双击打开脚本，添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/2018070409373573?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704093743445?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704093749709?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704093759237?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
#### 常用的自定义窗口属性
![这里写图片描述](https://img-blog.csdn.net/20180704093831639?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
传送门：http://www.ceeger.com/Script/EditorWindow/EditorWindow.html
![这里写图片描述](https://img-blog.csdn.net/20180704093844650?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/201807040939016?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
传送门：http://www.ceeger.com/Script/GUILayout/GUILayout.html

以上是我们这个案例中主要用到的几个类。
#### 代码分析
属性
![这里写图片描述](https://img-blog.csdn.net/20180704093929314?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
首先声明了三个变量：

 - 1.bugReporterName 用于储存记录Bug人的名字
 - 2.description 用于描述Bug信息
 - 3.buggyGameObject 用于储存 Bug 对象

设置窗口的名字
![这里写图片描述](https://img-blog.csdn.net/20180704094005582?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
如代码注释所示，利用构造函数来设置窗口的名字。比较陌生的是 titleContent属性 和 GUIContent 类，简单了解如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704094022383?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
GUIContent 界面内容类
![这里写图片描述](https://img-blog.csdn.net/2018070409411067?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
titleContent 属性
这个构造函数所产生的作用如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704094127586?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
设置窗口的名字

##### 添加菜单栏选项 - 打开窗口
![这里写图片描述](https://img-blog.csdn.net/20180704094154313?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这个函数用于在菜单栏上添加一个打开该窗口的的菜单选项。比较陌生的是 [MenuItem()] 属性 和 GetWindow（）函数，简单了解如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704094200908?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
[MenuItem()] 属性需要注意的两点（上图也有提示）：
    1.必须是放在 Assets / Editor 文件夹下的类，且使用了 using UnityEditor
    2.调用的函数必须是静态函数（static）
    ![这里写图片描述](https://img-blog.csdn.net/20180704094221213?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
该函数就是用于返回一个窗口对象（就是打开一个窗口）。
##### 绘制窗口

 绘制窗口元素需要在 OnGUI() 函数里面设计，接下来我们一一分解。
![这里写图片描述](https://img-blog.csdn.net/20180704094257415?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
步骤：
    1.GUILayout.Space(10)，这个有说过，让两个元素之间空十个像素之间的距离
    2.GUI.skin.label.fontSize 、GUI.skin.label.alignment 用于设置标题的字体大小和对齐格式，具体从下图中了解：
![这里写图片描述](https://img-blog.csdn.net/20180704094306352?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
对于 GUI.skin API 里面没有列出一些关于皮肤的属性，但是大伙们可以通过在Assets 菜单下右键 Create => GUI skin，如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704094329705?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
想对应的属性全在里面

 3.利用 GUILayout.Label() 来绘制标题

整个代码的效果如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704094339683?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### 绘制文本
![这里写图片描述](https://img-blog.csdn.net/20180704094412707?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好吧，似曾相识，因此不在赘述，效果如下所示：
![这里写图片描述](https://img-blog.csdn.net/20180704094426716?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
##### 显示当前正在编辑的场景
![这里写图片描述](https://img-blog.csdn.net/20180704094441399?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
在这段代码中，比较陌生的也就是 EditorSceneManager.GetActiveScen().name，我们先看下图进行简单的了解：

![这里写图片描述](https://img-blog.csdn.net/20180704094452835?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
其实就是返回当前编辑的场景信息（也就是返回 Scene 类型参数），然后利用 name 属性获取场景的名字，效果如下：

![这里写图片描述](https://img-blog.csdn.net/20180704094503243?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
##### 显示当前时间
![这里写图片描述](https://img-blog.csdn.net/20180704094535120?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这段代码主要就是利用 System.DateTime.Now 获取当前时间，然后通过 GUILayout.Label（） 把当前时间显示出来，对于 System.DateTime.Now 可从下面链接去了解：

https://msdn.microsoft.com/zh-cn/library/system.datetime.now(v=vs.110).aspx

效果图如下：
![这里写图片描述](https://img-blog.csdn.net/20180704094542999?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#####绘制对象槽
![这里写图片描述](https://img-blog.csdn.net/2018070409460836?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

我们先看看API：

![这里写图片描述](https://img-blog.csdn.net/20180704094614917?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

从上图可知：
    1.第一个参数用于设置卡槽的标题名字
    2.第二个参数用于设置字段显示的物体
    3.第三个参数用于设置显示的类型
    4.第四个参数用于设置是否允许指定场景中的物件

效果如下：
![这里写图片描述](https://img-blog.csdn.net/20180704094632789?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### 绘制描述文本区域
![这里写图片描述](https://img-blog.csdn.net/20180704094659430?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好的，这段代码我们也介绍过了，直接上效果图：
![这里写图片描述](https://img-blog.csdn.net/20180704094705524?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### 绘制按钮
![这里写图片描述](https://img-blog.csdn.net/20180704094741521?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704094748108?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704094753718?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
其实很简单，不外乎就是添加一个按钮呗。在我们的代码中，用了一个 if 判断语句来判断，当我们点击该按钮时所触发的事件（该函数的返回值是一个 bolol 类型），在代码中克森也上好备注了，因此也没有什么难的，直接上效果图：
![这里写图片描述](https://img-blog.csdn.net/2018070409480274?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
#####SaveBug（） 函数
![这里写图片描述](https://img-blog.csdn.net/20180704094821633?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
其实这个函数所做的事情也很简单，就是把我们设置好的一些参数保存到一个文本文件（.txt文件）上，仅此而已。

步骤如下：
    1.第一行，利用 Directory 类创建一个目录
    2.创建一个写入流类（StreamWriter）
    3.然后把设置好的各个参数写入文件中

还不了解 C# 文件操作的朋友，是时候返回去补补了，API 链接如下：

https://msdn.microsoft.com/zh-cn/library/system.io.directory(v=vs.110).aspx
https://msdn.microsoft.com/zh-cn/library/system.io.streamwriter(v=vs.110).aspx

具体操作如下所示：
![这里写图片描述](https://img-blog.csdn.net/20180704094848995?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后你便能在 Assets 文件夹看到如下图所示信息：
![这里写图片描述](https://img-blog.csdn.net/2018070409485979?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### SaveBugWithScreenshot（） 函数
![这里写图片描述](https://img-blog.csdn.net/20180704094920363?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
其实这个函数所做的事情跟上面那个函数做的事情一样一样的，不外乎就对了一行代码，而这行代码便是获得游戏屏幕截图，仅此而已，通过下面 API 去了解：
![这里写图片描述](https://img-blog.csdn.net/20180704094927944?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
具体操作和上面一样一样的，只不过多了一样图片，仅此而已，如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704094942327?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好吧，至此我们的这篇文章就结束了。

----------

### Unity Editor 基础篇（四）：Handles
#### 最终效果
![这里写图片描述](https://img-blog.csdn.net/20180704095100360?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
#### 准备工作
由于太久不更新，之前的项目不知道跑哪儿去了。让我们重新创建一个新的项目，命名为“MyHandles”。然后创建三个文件夹，如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704095121637?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接下来在Scripts文件夹中，创建一个C#脚本，并命名为“MyHandles”；然后在Editor文件夹中再创建一个C#脚本，命名为“HandlesInspector”；然后在将下面的小图标保存到Img文件夹中：
![这里写图片描述](https://img-blog.csdn.net/20180704095134601?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好了准备工作就绪，开始码了个码。
#### 码前预习
在这篇教程中，我们主要用到 Handles 这个类，一下是该类的基本介绍，克森会挑出几个比较常用的属性和方法来制作一下简单的东西，其它属性和方法大伙们可以自行去尝试尝试：
![这里写图片描述](https://img-blog.csdn.net/20180704095205596?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
API传送门：http://www.ceeger.com/Script/Handles/Handles.html  
#### 绘制半径操作柄
首先打开我们的 MyHandles.cs 脚本，为其添加一个变量：
![这里写图片描述](https://img-blog.csdn.net/20180704095244163?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后打开 HandlesInspector.cs 脚本，添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704095252679?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704095300513?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
将这两个脚本保存，回到Unity中创建一个空物体，并为其添加 MyHandles.cs 脚本：
![这里写图片描述](https://img-blog.csdn.net/20180704095316511?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
此时我们观察场景，除了场景中出了　“MyHandles”　几个字外，似乎啥事儿也没发生，不急，让我们来调整调整 Area Radius 参数的值，便能看到如下效果：
![这里写图片描述](https://img-blog.csdn.net/20180704095327835?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
代码很简单，注释也给大家弄上了，相信大家都能看懂吧。还是不太懂的可以多看看API：
![这里写图片描述](https://img-blog.csdn.net/20180704095356752?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704095402767?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
作用：这个东西是不是有点类型于碰撞体的那个框框啊，这玩意多用于制作AI，用于判断和指定UI影响范围用的。

#### 绘制缩放操作柄
打开 MyHandles.cs 脚本，添加如下变量：
![这里写图片描述](https://img-blog.csdn.net/201807040954374?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后为 HandlesInspector.cs 脚本添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704095443472?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这段代码呢也不难于理解，就是参数多了点
![这里写图片描述](https://img-blog.csdn.net/20180704095512602?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704095519199?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
PS：由于中文版的介绍不全，所以补了一张官方的API。

作用：多用于绘制一些自定义的操作，比如Unity的粒子系统就用到了好多自定义的操作柄，比如粒子系统的Shape参数就用到了该函数的第五个参数来绘制：

![这里写图片描述](https://img-blog.csdn.net/20180704095532533?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
#### 绘制位置操作柄
打开 MyHandles.cs 脚本，添加如下变量：
![这里写图片描述](https://img-blog.csdn.net/201807040956077?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后为 HandlesInspector.cs 脚本添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704095614252?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
回到场景中，此时大伙们会碰到这样的问题：
![这里写图片描述](https://img-blog.csdn.net/20180704095636955?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
别担心，那是因为你没有设置 nodePoints 属性，所有该函数访问到一个空的数组，因此便报出了老司机错误。如下图所示便OK：
![这里写图片描述](https://img-blog.csdn.net/20180704095646143?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这段代码简单了吧，也就两个参数，如果还是不清楚的小伙伴可以多尝试尝试。
![这里写图片描述](https://img-blog.csdn.net/20180704095711460?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
作用：这个可以用在AI上面，然后为每一个AI添加一个位置操作柄，这样好像看上去方便不少吧？

#### 绘制旋转操作柄
打开 MyHandles.cs 脚本，添加如下变量：
![这里写图片描述](https://img-blog.csdn.net/20180704095751554?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后为 HandlesInspector.cs 脚本添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704095758934?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

同样的，会出现上面类似的错误，甚至更加严重，Scene视图直接白屏！！使用同样的解决方法即可，如下所示：
![这里写图片描述](https://img-blog.csdn.net/2018070409582682?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
PS：nodePointsRotation的个数要与nodePoints的个数要相等或者小于nodePoints。

这段代码同样很简单了吧，也是两个参数，如果还是不清楚的小伙伴可以多尝试尝试。
![这里写图片描述](https://img-blog.csdn.net/20180704095833141?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
做一个操作，看看大伙们能不能看懂我想表达的意思：
![这里写图片描述](https://img-blog.csdn.net/20180704095855526?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
是不是感觉像是静止一般，一动不动的呢？下面修改一下代码：

![这里写图片描述](https://img-blog.csdn.net/20180704095945774?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
此时，你会看到如下的错误，不要慌张，我们只有越到错误，解决多了，那以后越到错误就不是什么可怕的事情了：
![这里写图片描述](https://img-blog.csdn.net/20180704100001447?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
相信很多人都知道这个是什么错误吧，那是因为由三维向量转为四元素W的值不能为0，因此我们只要把W设置为1即可，如下所示：
![这里写图片描述](https://img-blog.csdn.net/20180704100016858?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这个操作呢，主要是帮大家找出一些开发过程中容易遗漏的错误，还有一个目的就是让坐标轴跟随着旋转而旋转（因为第二个参数是位置操作柄的旋转方向嘛，我把它改为了我们设置好的旋转方向，因此位置操作柄便能跟随着我们的旋转而旋转了）。
#### 连接操作柄
为 HandlesInspector.cs 脚本添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704100116773?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
此时回到场景中便能看到如下所示：
![这里写图片描述](https://img-blog.csdn.net/20180704100136675?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这段代码呢，也很简单，可能有点不明白的也就是 Mathf.Repeat()函数，这个简单，看下图便能明白：
![这里写图片描述](https://img-blog.csdn.net/20180704100146130?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
其实这样做的原因大伙们都知道，就是为了防止下标越界。
#### 打开和关闭操作柄
打开 MyHandles.cs 脚本，添加如下变量：
![这里写图片描述](https://img-blog.csdn.net/20180704100216457?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后为 HandlesInspector.cs 脚本添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704100222727?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
其实也很容易理解，就是当showNodeHandles为false时便不执行 if 代码块里的代码，因此便无法绘制出位置操作柄和旋转操作柄咯，最终的效果如下：
![这里写图片描述](https://img-blog.csdn.net/20180704100240291?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
#### 绘制场景GUI
为 HandlesInspector.cs 脚本添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704100304858?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
回到场景便能看到如下图所示的界面：
![这里写图片描述](https://img-blog.csdn.net/20180704100311638?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这段代码呢，其实也很简单，不过是运用了两对函数。

第一对为：Handles.BeginGUI() 和 Handles.EndGUI()。这对函数表名你想要在Scene视图下绘制东西。

第二对为：GUILayout.BeginHorizontal() 和 GUILayout.EndHorizontal()。这对函数表明你想要以水平方向绘制东西。相信第二对函数大伙们都不陌生吧，记得在《Unity Editor 基础篇（三）：Editor Window》中有介绍过。

里面的逻辑代码也很简单，那就是绘制一个按钮，当我点击时让 MyHandles.shoNodeHandles的值取反（也就是原来为true，点击后取反，便为false）。

补充：在第一对函数里得操作和自定义窗口里得操作几乎相同，大家可以参考下面得API去尝试尝试：

http://www.ceeger.com/Script/GUILayout/GUILayout.html
这个插件就用到了今天我们学到的东西制作而成的：
![这里写图片描述](https://img-blog.csdn.net/20180704100336584?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704100342152?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好了，差不多就介绍到这里吧


----------
### Unity Editor 基础篇（五）：Gizmos
#### 最终效果
![这里写图片描述](https://img-blog.csdn.net/20180704100432324?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
#### 准备工作
在之前的项目或者新建的项目中创建如下目录结构：

![这里写图片描述](https://img-blog.csdn.net/2018070410045010?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
如果是新的项目，只需创建Scripts和Gizmos就好。

该文章用的到API：
![这里写图片描述](https://img-blog.csdn.net/20180704100509286?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
传送门：http://www.ceeger.com/Script/Gizmos/Gizmos.html


#### 基础知识
![这里写图片描述](https://img-blog.csdn.net/20180704100548244?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
由上图可知，Gizmos是用于在场景视图可视化调试或辅助设置用的。

需要注意的是所以Gizmos的绘制必须在脚本的OnDrawGizmos或OnDrawGizmosSelected里编写，因此我们的第一步便是在脚本中添加这两个函数。
#### 码了个码
在Scripts文件夹中创建一个C#脚本，命名为：“MyGizmos”，双击打开脚本，码入如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704100629753?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
让我们来测试一下：
![这里写图片描述](https://img-blog.csdn.net/20180704100642695?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
咦，感觉不对啊，感觉不是每一帧都在调用啊！克森做了个测试，如果你在Scene视图下不做任何错误（鼠标滑动也不能调用这两个函数），这两个函数都没有调用（看来官方文档说得不完全啊！！）。

不管了，总之大伙们知道是这么一回事儿就行了。

PS：必须于Scene视图下，于Game视图下不起作用。

接下来为“MyGizmos.cs”脚本添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704100651609?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好，现在回到场景视图下，如下图所示操作：
![这里写图片描述](https://img-blog.csdn.net/20180704100707988?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
哦豁，我们的线框球体便出来了，是不是很简单啊。

代码分析：
![这里写图片描述](https://img-blog.csdn.net/20180704100727633?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
该函数的第一个参数是该线框球体的中心点位置，它是一个Vector3类型。

第二个参数是该线框球体的半径大小，它是一个float类型。

接下为我们的脚本添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704100734269?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好，现在回到场景视图下，如下图所示操作：
![这里写图片描述](https://img-blog.csdn.net/20180704100752436?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
哦豁，我们的线条便出来了，是不是很简单啊。

代码分析：
![这里写图片描述](https://img-blog.csdn.net/20180704100800306?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
上图已经解释得非常清楚，两个参数表示：从from起点到to位置绘制一条线。

因此第一个参数就是起点的位置，第二个参数就是指定的位置。
![这里写图片描述](https://img-blog.csdn.net/20180704100819265?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
上面代码的意思就是当前的位置朝 Z-轴 正方向根据 size 的值扩大。

接下来为我们的脚本“MyGizmos.cs”添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/2018070410084093?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好，现在回到场景视图下，如下图所示操作：
![这里写图片描述](https://img-blog.csdn.net/20180704100852682?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
如上图所示，我们通过for循环调用Gizmos.绘制出了5个实心球体。

代码分析：
![这里写图片描述](https://img-blog.csdn.net/20180704100912317?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
由上图可知。第一个参数是绘制该球体的中心点的位置，第二个参数是该球体的半径。

因此在我们的代码中，利用for循环依据nodePoints参数创建多个球体，在上面的案例中克森创建了5个球体，设置它们的半径为0.5（大伙们也可以添加一个参数，进行动态操作半径值）。

由于绘制的东西都是一个色，不好辨别，大伙们可以添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704100937343?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
添加后的效果：
![这里写图片描述](https://img-blog.csdn.net/20180704100949430?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
我们的球体变成了蓝色，我们的线条编程了红色，未设置的线框球体还是默认的白色。

PS：Gizmos.color = Color.blue，如果后续没有重新指定绘制的颜色，则使用最后一次设置的颜色。

接下来为我们的脚本“MyGizmos.cs”添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704101002637?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
回到场景中看看有什么效果：
![这里写图片描述](https://img-blog.csdn.net/20180704101034539?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好了，从上图中大家也知道我们添加的这段代码的意思了吧。很简单，就是将这些球体给连接起来。

![这里写图片描述](https://img-blog.csdn.net/20180704101042973?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
尴尬了，发现最后要将的东西已经在前面暴露出来了。。。就是下图的这个东西：
![这里写图片描述](https://img-blog.csdn.net/20180704101127407?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
相信这个东西大伙们都知道怎么设置吧，不过为了该文章的完整性，我们还是来操作一遍：

![这里写图片描述](https://img-blog.csdn.net/20180704101143846?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好了，这个是手动设置的，那么代码中又是如何设置的呢？

请大家添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704101201489?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
回到场景中，看看有什么效果发生。

咦，没有什么事情发生啊！！！
![这里写图片描述](https://img-blog.csdn.net/20180704101209715?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
哦，原来是找不到图片资源。如上图所示，这就是为什么文章的开头让大伙们创建 Gizmos 文件夹的原因。现在将一张你喜欢的图标，命名为：“icon.jpg”放入Gizmos文件夹中。

PS：图片的命名一定要与代码中的第二个参数的名字一样。


----------
### Unity Editor 基础篇（六）：Property Drawers
#### 最终效果
![这里写图片描述](https://img-blog.csdn.net/20180704101311701?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
####准备工作
创建一个新的工程或者用上一篇的工程都可以（克森是新建的工程），然后在Scripts文件夹中创建两个C#脚本，分别命名为：“Persion.cs”和“ShowPersionInfo.cs”，如下图所示：

![这里写图片描述](https://img-blog.csdn.net/20180704101344620?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后在Editor文件夹中创建一个名为“PersionPropertiesDrawer.cs”的脚本，具体如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704101400366?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
#### 码了个码
首先，打开我们得“Persion.cs”脚本，为其添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704101419974?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这段代码不用解释了吧，就是一个普通得类和枚举。接下来为我们的“ShowPersionInfo.cs”脚本添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704101431298?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
为什么要这样呢？相信大伙们都知道，要想给一个游戏对象挂上脚本，那么该脚本就必须继承自 MonoBehaviour 。大伙们可以将“Persion.cs”挂到游戏对象上，便会出现如下图所示：

![这里写图片描述](https://img-blog.csdn.net/20180704101442214?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
所以“ShowPersionInfo.cs”仅仅就是一个辅助类，作用就是将我们的“Persion.cs”能挂到游戏对象上。

好了，接下来让我们创建一个空的游戏对象，并且命名为“Persion”，然后为其添加“ShowPersionInfo.cs”脚本：
![这里写图片描述](https://img-blog.csdn.net/20180704101454312?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这什么都没有啊！！原来，我们漏掉了一段代码，接下来让我们为其补上：
![这里写图片描述](https://img-blog.csdn.net/20180704101518555?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
原来呀，要想将一个普通的类里的属性在Inspector面板中显示出来，那么必须将这个普通的类序列化。

好了，让我们回到 Unity 中，看看发生了什么变化。
![这里写图片描述](https://img-blog.csdn.net/20180704101531241?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Perfect！Persion类中的属性成功的显示在了Inspector面板上。

好，让我们简单的了解一下，什么是序列化，如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704101621832?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
（图片来源于百度百科）
简单的理解就是，序列化类的时候是从属性读取值以某种格式保存下来，将其传输到另一个地方去。那么呢，这个过程是交给Unity引擎来实现的，简单的了解就行了（也就是会用就行了）。

接下来就是本篇教程的核心了！！！

首先打开我们的“PersionPropertiesDrawer.cs”脚本，为其添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704101700814?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
让我们的“PersionPropertiesDrawer.cs”继承自PropertyDrawer类，然后重写 OnGUI 和 GetProperties 函数。

添加 [CustomPropertyDrawer(typeof(Persion))]，指定该类是用于自定义属性的绘制。

接下来让我们来测试一下这些方法传入的参数都是做什么的，为我们的脚本添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/2018070410170982?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好，现在回到Unity看看测试的数据：
![这里写图片描述](https://img-blog.csdn.net/20180704101723205?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
从上面的数据可以看出如下几点：
    1.OnGUI 和 GetPropertyHeight 里的 property 参数是同一个参数。该参数里存放的是 Persion 里的属性信息。

   2.OnGUI 和 GetPropertyHeight 里的 Label 参数也是同一个参数，该参数里存放的是 Persion 类的类名。

   3.position参数指的是需要在Inspector面板中绘制的区域信息，可以从下面两张图中简单的了解一下：
    
![这里写图片描述](https://img-blog.csdn.net/20180704101746749?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704101758854?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
PS：为了测试，注释了一些代码，并且添加了一个2D刚体组件。

对了还有一个地方遗漏掉了，那就是在Inspector面板中的一行高度为 16 。我们可以从下图中得知。
![这里写图片描述](https://img-blog.csdn.net/20180704101823288?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好了，接下来就让我们来绘制我们Persion的属性吧。我们的目标如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704101840412?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
下面看一看我们的分析图：
![这里写图片描述](https://img-blog.csdn.net/20180704101858220?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好了，接下来就开始码我们的代码，打开“PersionPropertiesDrawer.cs”，为其添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704101930136?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704101937172?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
上面的代码呢，都有分析过了。也许会有一些小伙伴在两个地方上头晕，也就是【获取对应的序列化属性】和【绘制属性】这两个地方上弄不明白。其实很简单，从下面两张图中便可理解：
![这里写图片描述](https://img-blog.csdn.net/20180704101945299?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180704101951993?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好了，让我们回到Unity中，看看我们的效果实现了没：
![这里写图片描述](https://img-blog.csdn.net/20180704102017944?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好的，非常完美的实现了。

至此，该篇文章就已经弄完了。


----------
### Unity Editor 基础篇（七）：Property Attributes

#### 最终效果
![这里写图片描述](https://img-blog.csdn.net/20180704102127315?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
#### 准备工作
大伙们还记得《Unity Editor 基础篇（一）：Build-In Attribute》里所说的东西吗？如下图所示：


![这里写图片描述](https://img-blog.csdn.net/20180704102139785?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

创建一个新的工程或者用上一篇的工程都可以（克森用的是原来的工程），然后在Scripts文件夹中创建两个C#脚本，分别命名为：“ReadOnlyAttribute.cs”和“Test.cs”，如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704102154486?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后在Editor文件夹中创建一个名为“ReadOnlyAttributeDrawer.cs”的脚本，具体如下图所示：


![这里写图片描述](https://img-blog.csdn.net/20180704102205233?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 码了个码

首先，打开我们得“ReadOnlyAttribute.cs”脚本，为其添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704102230654?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这段代码很简单，就是让我们的“ReadOnlyAttribute”类继承自“PropertyAttribute”类，该类的解释如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704102244664?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
所以呢，由上图便能知道我们接下来要做的事情了吧，那就是让我们的“ReadOnlyAttributeDrawer”类继承自PropertyDrawer类，然后重写OnGUI和GetPropertyHeight方法，如下图所示：
![这里写图片描述](https://img-blog.csdn.net/2018070410231052?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

上图的代码在上一篇都有讲解过，因此这里不再做过多的赘述。

好的，接下来继续为我们的“ReadOnlyAttributeDrawer.cs”的OnGUI方法添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704102319105?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
在上面的代码中，我们使用到了一个名为“SerializedPropertyType”的枚举，它存放的是序列化属性的类型，它包含的类型很多，但是在该篇文章中我们只使用到了这几个，感兴趣的同学看可以去尝试其它类型。

我们使用该枚举为value获取相对应类型的值，然后使用一个Label在Inspector面板中绘制出来（\t为制表符，为了美化显示）。

好了，接下来打开我们的“Test.cs”脚本，添加如下代码：

![这里写图片描述](https://img-blog.csdn.net/20180704102343289?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
现在，让我们回到Unity中查看一下效果：
![这里写图片描述](https://img-blog.csdn.net/20180704102355438?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这个。。。是不是很简单呀。接下来让我们制作一个带有参数的。

让我们打开我们的“ReadOnlyAttribute.cs”，添加如下代码：

![这里写图片描述](https://img-blog.csdn.net/20180704102417153?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
再让我们为“ReadOnlyAttributeDrawer.cs”的OnGUI函数添加如下方法：
![这里写图片描述](https://img-blog.csdn.net/20180704102424694?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
上面的代码相信大伙们都能看懂，唯一有点迷惑的可能就是 attribute 属性了，其实它就是我们通过 [CustomPropertyDrawer(typeof(ReadOnlyAttribute))] 传过来的ReadOnlyAttribute类，因此克森Debug了它的类型名字，接下来让我们看看打印出来的信息：
![这里写图片描述](https://img-blog.csdn.net/20180704102436298?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好的，就是我们的 ReadOnlyAttribute 类。对了克森打印了 myAttribute.textColor 的值，是为了测试值是否正确的传入。

好了，接下来开始测试，让我们为我们的“Test.cs”脚本添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704102456664?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
从上图可知，其实[ReadOnly(参数)] 传入的参数对应的就是我们ReadOnlyAttribute类的构造函数需要传入的参数。

好了，接下来让我们回到Unity中查看一下效果：
![这里写图片描述](https://img-blog.csdn.net/20180704102508277?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好了，大伙们可以看到，值已经完美的传入了，效果并不是完美期望的那样。但是我们的 My Int 的 Apha 值起作用了。

克森对于这个Bug弄了一个晚上，最后发现原来是Unity5.x出的BUG，总之克森今晚把Unity5.x版本都试得差不多了，还是一个鸟样，最后Unity4.6版本妥妥得实现了我们想要得效果，下面有两张图，一张是克森试过得版本，一张是Unity4.6实现得效果图：
![这里写图片描述](https://img-blog.csdn.net/20180704102534743?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
（试过的版本）
![这里写图片描述](https://img-blog.csdn.net/20180704102548533?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
（Unity4.6版本的效果）

好了，今天的教程就到这里吧


----------
### Unity Editor 基础篇（八）：Decorator Drawers

#### 最终效果
![这里写图片描述](https://img-blog.csdn.net/20180704102640394?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
#### 准备工作
创建一个新的工程或者用上一篇的工程都可以（克森用的是原来的工程，因为这一篇的内容和上一篇的内容很类似），然后在Scripts文件夹中创建两个C#脚本，分别命名为：“DrawerImageAttribute.cs”和“Test.cs”，如下图所示：
![这里写图片描述](https://img-blog.csdn.net/2018070410270223?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后在Editor文件夹中创建一个名为“DrawerImageAttributeDrawer.cs”的脚本，具体如下图所示：
![这里写图片描述](https://img-blog.csdn.net/20180704102712772?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
#### 码了个码
首先，打开我们得“DrawerImageAttribute.cs”脚本，为其添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704102729643?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这段代码很简单，就是让我们的“DrawerImageAttribute”类继承自“PropertyAttribute”类，上一篇已经讲过，不了解的同学可以去看《Unity Editor 基础篇（七）：Property Attributes》

接下来，打开我们的“DrawerImageAttributeDrawer.cs”脚本，为其添加如下脚本：


![这里写图片描述](https://img-blog.csdn.net/20180704102741840?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
上图的代码除了红色框框里的东西，其它的在上一篇都有讲解过，因此这里不再做过多的赘述。

好了，让我们来分析分析 DecoratorDrawer 类是个什么东西，首先从字面上的意思就是：装饰绘制者。意味着它是用于装饰的。接下来让我们看看它的源码：

![这里写图片描述](https://img-blog.csdn.net/20180704102756565?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
从源码中我们知道，它其实和上一篇的PropertyDrawer类差不多，都是继承自 GUIDrawer。只不过他的 OnGUI 方法的参数比 PropertyDrawer 的 OnGUI 方法的参数好了两个，仅此而已。

接下来让我们为“DrawerImageAttributeDrawer.cs”添加如下代码：

![这里写图片描述](https://img-blog.csdn.net/20180704102807372?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
上面的代码应该不难理解吧，就是判断图片是否存在，如果不存在就去Resources文件夹里读取对应的图片，然后调用 GUI.DrawTexture(position, image); 在Inspector面板中绘制该图片。

因此，接下来的操作相信大伙们都知道了吧。那就是创建Resources文件夹，然后将图片放入该文件夹中，修改相对应的名字，搞定！
![这里写图片描述](https://img-blog.csdn.net/20180704102818796?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接下来，让我们回到Unity中查看效果：
![这里写图片描述](https://img-blog.csdn.net/20180704102829738?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
咦，怎么回事儿，怎么那么难看？看到这里，相信看过上一篇文教的伙计们应该知道怎么做了吧？那就是修改我们 GetHeight() 方法的返回值就行了呀。

让我们回到我们的“DrawerImageAttribute.cs”脚本中，为其添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704102840413?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接着回到“DrawerImageAttributeDrawer.cs”中，添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704102908418?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好了，接下来打开我们的“Test.cs”脚本，添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704102914971?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
现在，让我们回到Unity中查看一下效果：

![这里写图片描述](https://img-blog.csdn.net/2018070410292545?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这...你坑我？不急不急，克森是故意这么做的，错误见多了那就不是错误了。

好，让我们来解决这个错误。接下来为我们的“DrawerImageAttributeDrawer.cs”脚本添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704102954114?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后回到Unity中，看看测试的数据，分析出错原因：
![这里写图片描述](https://img-blog.csdn.net/20180704103000533?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
从上图中，我们可以看出，该脚本先调用的是 GetHeight() 方法，因此当我们在 GetHeight() 方法中使用 _attribute.height 的时候便会报空指针的错误，因为此时的 _attribute 还没有初始化，因此让我们添加如下代码：
![这里写图片描述](https://img-blog.csdn.net/20180704103011709?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好了，接下来回到Unity中查看效果：
![这里写图片描述](https://img-blog.csdn.net/20180704103020745?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Perfect，漂亮的完成了。


好了，《Unity Editor 基础篇》系列结束了！！！太棒了
