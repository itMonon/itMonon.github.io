---
layout: post
category: Unity3D-Plugin
title: 【KGFMapSystem插件】Unity3d KGFMapSystem插件使用
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
在我们开发游戏或者虚拟现实中，一般都会用到小地图，如果要我们去写小地图，应该会花费一点时间的吧，如何加快我们的开发速度呢，其实在unity 3d中就有一个“小”插件，是专门用开开发小地图用的，这个插件就是KGFMapSystem。这个是它的官网。http://www.kolmich.at/documentation/（KGF里面不只是有一个这样的插件，它里面有很多插件，有兴趣的朋友可以研究一番）

## 二、正文
1.首先我们倒入这个插件包，打开kolmich/KGFMapSystem/demo/scenes/quickstart_demo
我们就能看见这个了
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlcy5jbml0YmxvZy5jb20vYmxvZy80ODc3MDEvMjAxMzA1LzEyMTEwMDUzLTE4YjcwZjgzZWQ2ZTQyYzliMGMzODVjODczNzdlNzM3LnBuZw?x-oss-process=image/format,png)
你看，我们现在能看见一个红色的标志的警号提示了，意思就是叫我们新建一个层（layer），取名为mapsystem(必须得是这个名字才能有用).
选中我们摄像头，去掉我们刚才建的mapsystem这个layer.如图:
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlcy5jbml0YmxvZy5jb20vYmxvZy80ODc3MDEvMjAxMzA1LzEyMTEwMTExLWYwNTY2ODk1YWIyZTRiZGE4M2YwODI3MDg0Mjk1NzQxLnBuZw?x-oss-process=image/format,png)
现在我们来运行一下，你看在右上角就能看见我们梦寐以求的小地图了 是不是？？
![在这里插入图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlcy5jbml0YmxvZy5jb20vYmxvZy80ODc3MDEvMjAxMzA1LzEyMTEwMTI0LWQ0M2VjMjZhMzk5OTQwNjRhMjQzZWQ2ZjZlYjQ4YzQ1LnBuZw?x-oss-process=image/format,png)
从以上步骤来看，用这个插件是不是很容易的就开发了自己的小地图呢?.有的人就会说，这个是它这个场景自带好吧，如果我们在实际的项目中来发呢。现在我来教大家如何在自己的项目中开发自己的小地图。

我们打开quickstart_try_yourself这个场景，运行后发现是不是什么都没有呢，这个就像我们自己原始的项目。
我们找到kolmich/KGFMapSystem/prefabs里面中的KGFMapSystem这个预设，直接拖进我们的工程，如图：
![<img src="http://images.cnitblog.com/blog/487701/201305/12110157-27c9d0db7ce44c70aee31314c6d35f4c.png" alt="" style="margin:0px; padding:0px; border:0px" />](https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlcy5jbml0YmxvZy5jb20vYmxvZy80ODc3MDEvMjAxMzA1LzEyMTEwMTU3LTI3YzlkMGRiN2NlNDRjNzBhZWUzMTMxNGM2ZDM1ZjRjLnBuZw?x-oss-process=image/format,png)
我们看看右下角的那个提示（我用蓝色线圈圈住的的）。我相信大家都能读懂他是什么意思吧。我们找到我们的人物，再直接附上给Its Target.如图:
![<img src="http://images.cnitblog.com/blog/487701/201305/12110221-f46b21a36b494654b08dbdc3a1bd8a46.png" alt="" style="margin:0px; padding:0px; border:0px" />](https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlcy5jbml0YmxvZy5jb20vYmxvZy80ODc3MDEvMjAxMzA1LzEyMTEwMjIxLWY0NmIyMWEzNmI0OTQ2NTRiMDhkYmRjM2ExYmQ4YTQ2LnBuZw?x-oss-process=image/format,png)
我们再在kolmich/KGFMapSystem/prefabs/mapicons_samples中找到KGFMapIcon_player这个预设，拖进我们人物中。（让它成为我们的子物体，其实这个就相当于在地图中表示自己的那个标记）可别忘记了我们在刚开始建一个layer的那些步骤哦，运行如图:
![<img src="http://images.cnitblog.com/blog/487701/201305/12110243-1a8cc466a87c4e0da0a4574b0c85c14d.png" alt="" style="margin:0px; padding:0px; border:0px" />](https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlcy5jbml0YmxvZy5jb20vYmxvZy80ODc3MDEvMjAxMzA1LzEyMTEwMjQzLTFhOGNjNDY2YTg3YzRlMGRhMGE0NTc0YjBjODVjMTRkLnBuZw?x-oss-process=image/format,png)
5.我们看见自己的小地图了吧，中间的那个黄色箭头就是我们自己。

6.细心的人就会发现小地图旁边有4个按钮一样的东西，没错 他就是按钮，有放大地图……这些功能。里面还有很多设置，你可以慢慢去试着研究吧。自己动手丰衣足食!重要的部分我都说了，其他的就看自己了。

我将其应用到我的工程

![<img src="http://img.blog.csdn.net/20140122202730015?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZGluZ3hpYW93ZWkyMDEz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" />](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTQwMTIyMjAyNzMwMDE1?x-oss-process=image/format,png)
仔细一看其实还是有问题的，就是建筑物显示出来了，但地面没有显示出来！怎么办呢，只能再继续琢磨，在同事萍萍的共同探讨下，发现了问题，就是层的问题，打开地面的下面的子节点一看，原来他是在自定义的maylayer层上，这个层是留给插件本身用的，其他我们自己的model是不可以在这个层上的，所以要代码修改或者是自己做预设，将物体都默认自定义在default层。
解决问题之后应该是这样的
![\[外链图片转存失败,源站可能有防盗<img src="http://链机制,建议传(i.blMg.sdn.net/20140124oJcwB145548046?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZGluZ3hpYW93ZWkyMDEz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="" />http://img.blog.csdn.net/20140124145548046?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZGluZ3hpYW93ZWkyMDEz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)\]](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTQwMTI0MTQ1NTQ4MDQ2?x-oss-process=image/format,png)
![<img src="http://img.blog.csdn.net/20140207121942734" alt="" />](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTQwMjA3MTIxOTQyNzM0?x-oss-process=image/format,png)工程源文件下载：<a target=_blank target="_blank" href="http://download.csdn.net/detail/s10141303/6880779">http://download.csdn.net/detail/s10141303/6880779</a>
