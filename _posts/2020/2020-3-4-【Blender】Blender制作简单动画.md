---
layout: post
category: web
title: 【Blender】Blender制作简单动画
tagline: by 恬静的小魔龙
tag: Other
---

## 一、前文
&ensp;&ensp;&ensp;&ensp;今天给大家介绍一款3D动画制作软件，Blender 是一款开源的跨平台全能三维动画制作软件，提供从建模、动画、材质、渲染、到音频处理、视频剪辑等一系列动画短片制作解决方案。
&ensp;&ensp;&ensp;&ensp;Blender 拥有方便在不同工作下使用的多种用户界面，内置绿屏抠像、摄像机反向跟踪、遮罩处理、后期结点合成等高级影视解决方案。同时还内置有卡通描边（FreeStyle）和基于 GPU 技术 Cycles 渲染器。以 Python 为内建脚本，支持多种第三方渲染器。
&ensp;&ensp;&ensp;&ensp;Blender 为全世界的媒体工作者和艺术家而设计，可以被用来进行 3D 可视化，同时也可以创作广播和电影级品质的视频，另外内置的实时 3D 游戏引擎，让制作独立回放的 3D 互动内容成为可能。
有了 Blender 后，喜欢 3D 绘图的玩家们不用花大钱，也可以制作出自己喜爱的 3D 模型了。它不仅支持各种多边形建模，也能做出动画！

## 二、参考文章

【百度百科】https://baike.baidu.com/item/Blender/222123?fr=aladdin
【游戏制作之路（3）Blender制作极简动画】https://blog.csdn.net/caimouse/article/details/82144723
【Blender 2.7.8.0 中文版】http://www.onlinedown.net/soft/43975.htm
感谢以上文章作者提供的资料与教程

## 三、正文

### 1、下载地址
【网络资源】http://www.onlinedown.net/soft/43975.htm
【百度网盘】https://pan.baidu.com/s/1KaGS801tgcb3BcGNVwXB9g
【官方下载】https://builder.blender.org/download

### 2、安装
解压之后双击安装
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109153158496.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109153207998.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
设置路径
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109153217311.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
安装完成
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109153519852.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

### 3、汉化
&ensp;&ensp;&ensp;&ensp;对于英文不是太好的小伙伴来说英文界面是很不友好的，那怎么汉化呢。。（话说，我用英文软件的第一想法就是汉化，英文不好，汗。。。）
File->UserPreferences
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109153607157.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
切换到System界面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109153644620.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
勾选Use internationnal fonts
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109153801274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
切换语言
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109153958690.png)

最后一步，保存用户设置
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190109154046484.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 4、导入模型

打开我们的blender窗口。
![在这里插入图片描述](http://src.onlinedown.net/d/file/p/2018-04-28/a63ffe1ed64b9fde0291853fdb201976.png)
选择工具菜单中的“File”。
![在这里插入图片描述](http://src.onlinedown.net/d/file/p/2018-04-28/90d41531c32783cb17525b36573fb22b.png)
选择“Import”。
![在这里插入图片描述](http://src.onlinedown.net/d/file/p/2018-04-28/cc40f02871e8bfac10374c4857e0ddcc.png)
然后选择我们模型的格式，这里就是blender默认支持的一些模型格式，例如我我们导入obj格式的模型。
![在这里插入图片描述](http://src.onlinedown.net/d/file/p/2018-04-28/14ab72e073c5cf9d1a26ffa7dacf47a1.png)
　找到我们的obj文件所在路径并选中，然后“Import Obj”导入模型。
　![在这里插入图片描述](http://src.onlinedown.net/d/file/p/2018-04-28/ca80a5950697d984d69f6bab1ed07b3f.png)
　这样我们的obj模型就导入了，其他格式的模型也一样。
　![在这里插入图片描述](http://src.onlinedown.net/d/file/p/2018-04-28/2f45d784d7fca34250762aca5d712b4c.png)


### 5、快捷键 			
<table>
<tr>
<td>Shift + A
</td>
<td>调出创建菜单
</td>
</tr>
<tr>
<td>R键
</td>
<td>模型进行旋转
</td>
</tr>
<tr>
<td>G键
</td>
<td>模型进行移动
</td>
</tr>
<tr>
<td>S键
</td>
<td>模型进行缩放
</td>
</tr>
<tr>
<td>Shift + D
</td>
<td>复制功能
</td>
</tr>
<tr>
<td>Shift + D
</td>
<td>复制功能
</td>
</tr>
<tr>
<td>Z键
</td>
<td>切换到线框模式，再按一次切换回来
</td>
</tr>
<tr>
<td>Tab键
</td>
<td>切换到“编辑模式”，以及回到当前模式
</td>
</tr>
<tr>
<td>编辑模式 Ctrl+Tab
</td>
<td>切换到“点线面”选择模式
</td>
</tr>
<tr>
<td>选择模型部分后 E键
</td>
<td>将选择的部分模型挤出
</td>
</tr>
<tr>
<td>X键
</td>
<td>可以调出删除菜单，选择要删除的数据
</td>
</tr>
<table>

### 6、软件界面
![在这里插入图片描述](https://img-blog.csdn.net/20180828111531769?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NhaW1vdXNl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
通过这个界面介绍，就基本了解这个软件的基本功能分布了，现在就来进入创建动画的过程。

### 7、动画制作
第一步，先来到底部的时间轴这个区域，设置第一帧的动画是什么样的如下：
![在这里插入图片描述](https://img-blog.csdn.net/20180828112028321?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NhaW1vdXNl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
找到设置帧的位置了，接着下来，就是要把物体改变位置，改变大小，或者改变转动方向，做了这三个动作之后，就要插入关键帧了，那么怎么样来插入关键帧呢？这是通过快捷键的方式：按下i键，就会弹出如下菜单：
![在这里插入图片描述](https://img-blog.csdn.net/20180828112456153?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NhaW1vdXNl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
当你选择菜单里的一个选项之后，就可以插入关键帧了，这样就定义了动画的起始状态，相当于小孩子出生长得怎么样，后面动画，就看他长得怎么样了。blender这时就会记住场景里所有信息，接着下来所有变化，都会记录到动画的变化里。接着下来，我们来把这个四方体进行变大变小地变化，看看动画是否也跟着改变。因此，接着下来就创建第二个关键帧，这样软件就可以计算从第一个状态到第二个状态，它需要怎么样进行插值。第二个关键如下操作：
![在这里插入图片描述](https://img-blog.csdn.net/20180828113420474?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NhaW1vdXNl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
第二个操作是一样的，只是在中间添加了一个缩放物体的动作，这个缩放就是实现动画的功能：变大变小。当然，你也可以进行其它操作，比如位移，旋转，或者变成其它物体。接着下来创建第三个关键帧，同样把时间轴设置为100帧位置，可以在50的位置进行手动输入，也可以使用鼠标进行拖动绿色的滑块：

![在这里插入图片描述](https://img-blog.csdn.net/20180828114034742?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NhaW1vdXNl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
最后一步跟前面是一样的，只是按下s键把物体变小了，再按i键插入关键帧，最后把动画总帧数限制为100帧，这样动画就创建完成了。接着下来，就进观察这个动画是否符合我们设计的效果，按下底部播放键：
![在这里插入图片描述](https://img-blog.csdn.net/20180828114416923?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NhaW1vdXNl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这样就可以预览动画的效果，可见制作动画就是这么简单的一个过程。不过，这个查看是没有使用摄像头观看的，如果要那样查看，需要从菜单里选择这个：
![在这里插入图片描述](https://img-blog.csdn.net/20180828115055133?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NhaW1vdXNl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
当然，你也可以按下快捷键：Ctrl + F11,就会显示如下窗口：


![在这里插入图片描述](https://img-blog.csdn.net/201808281153491?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NhaW1vdXNl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
可以看到动画的结果了，这样就完成动画的制作过程了。接着下来是保存文件，如下图：
![在这里插入图片描述](https://img-blog.csdn.net/20180828115526394?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NhaW1vdXNl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
最后一步，也是最关键的一步，就是导出动画给游戏开发人员使用，有很多文件格式可以导出，最常用的使用是fbx格式，采用下面的菜单来导出：
![在这里插入图片描述](https://img-blog.csdn.net/2018082811574346?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NhaW1vdXNl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
好吧，这样就可以拿到动画数据了，我们来把这个动画保存名称为Test007.fbx，以便后面使用。

![在这里插入图片描述](https://img-blog.csdn.net/20180828120032821?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NhaW1vdXNl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
可见，通过blender制作一个动画是非常简单，快捷，导出功能也强大。


