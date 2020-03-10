---
layout: post
category: web
title: 【VR】用抛物线实现HTC Vive瞬移系统
tagline: by 恬静的小魔龙
tag: Other
---

<h1><a>用抛物线的点实现HTC Vive瞬移系统        </a></h1>
<div id="article_content" class="article_content tracking-ad" data-mod=popu_307  data-dsm = "post" >

<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">这个是针对HTC Vive和引擎简单易用的瞬移系统。这个系统是模仿Valve的Vive游戏“The Lab”而作，即玩家可以穿过比游戏区域大的VR环境。下图可以看到对于系统的演示：</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><br style="word-wrap:break-word">
</span></span></div>
<div align="center" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><img id="aimg_104668" src="http://img.manew.com/data/attachment/forum/201606/16/211806vkkkwiswnfydzs4i.gif" class="zoom" width="500" alt="" style="word-wrap:break-word">&nbsp;<br style="word-wrap:break-word">
</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><br style="word-wrap:break-word">
</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">提供的这个系统解决了以下<a target="_blank" href="http://www.manew.com/forum-ask-1.html" target="_blank" class="relatedlink" style="word-wrap:break-word; color:rgb(49,177,145); border-bottom-width:1px; border-bottom-style:solid; border-bottom-color:blue">问题</a>：</span></span></div>
<br style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><span style="word-wrap:break-word; font-weight:700">1&nbsp; &nbsp;&nbsp;&nbsp;</span><span style="word-wrap:break-word; font-weight:700">计算可导航空间：</span>我们显然不希望玩家可以传送出边界，或者在不透明物体内部。为了解决这个问题，这个系统使用了Unity生成的导航网&#26684;作为玩家可以传送的边界。因为这个过程包括了Unity的工作，所以它很稳定并且可以放心的应用到大多数工程中。为了预加载这个数据，只需要在场景的任何位置添加一个&quot;Vive
 Nav Mesh&quot;组件，并且在检视面板点击&quot;UpdateNavmesh Data&quot;按钮。这样无论什么时候更新场景都可以用新的NavMesh烘焙来更新Vive Nav Mesh。上边的过程说明如下图：</span></span></div>
<div align="center" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><img id="aimg_104664" src="http://img.manew.com/data/attachment/forum/201606/16/210217xbwehi6ir6wnotkw.gif" class="zoom" width="500" alt="" style="word-wrap:break-word">&nbsp;<br style="word-wrap:break-word">
</span></span></div>
<br style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><br style="word-wrap:break-word">
</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">2&nbsp; &nbsp;&nbsp;<span style="word-wrap:break-word; font-weight:700">选择瞬移目的地：</span>这个系统通过简单的运动学方程使用了直观的抛物线选择机制。再次说明，这是受到了Valve的“The Lab”启发。用户将控制器举到更高的角度时，选择点会生成的更远一些。如果用户将控制器举过45度（抛物线的最大距离），角度将会锁定在那个距离。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">3&nbsp; &nbsp;&nbsp;<span style="word-wrap:break-word; font-weight:700">表现游戏区域：</span>很有必要知道传送后防护边界在哪里。因此系统围绕防护边界划定了一个盒子。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">4&nbsp; &nbsp;&nbsp;<span style="word-wrap:break-word; font-weight:700">减少不适感：</span>瞬移（显示为“眨&#30524;”）时屏幕的淡入淡出可降低用户的疲劳和眩晕感。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><br style="word-wrap:break-word">
</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">这个Unity工程（版本5.3.4p6）提供了两个简单场景：一个直接集成了SteamVR，另一个适用于没有HTC Vive的小伙伴来演示系统功能。资源代码已经做了文档和注释，或者可以跟随MIT Licence（参照LICENSE.txt）使用.</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="word-wrap:break-word; font-weight:700"><span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><br style="word-wrap:break-word">
</span></span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="word-wrap:break-word; font-weight:700"><span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">开始吧</span></span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">为了使瞬移设置运行需要使用三个组件：</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><span style="word-wrap:break-word; font-weight:700">Vive Nav Mesh</span>组件控制Unity的NavMesh系统到可渲染网&#26684;的转换。它还会计算NavMesh的边界，所以当玩家选择传送位置时可以被显示出来。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><span style="word-wrap:break-word; font-weight:700">Parabolic Pointer</span>组件生成/显示一个指示网&#26684;并从<span style="word-wrap:break-word">Vive Nav Mesh</span>进行采样。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><span style="word-wrap:break-word; font-weight:700">Vive Teleporter</span>组件控制实际传送机制。它从<span style="word-wrap:break-word; font-weight:700">Parabolic Pointer</span>找出指示数据这样就知道要传送到哪里。当用户决定传送时它还会平稳的淡入淡出屏幕防止带来的不适感。它还可以和SteamVR配合来控制按钮点击事件、控制器管理、触觉反馈，当选择传送位置后还可以显示传送区域边界。</span></span></div>
<br style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">注：<span style="word-wrap:break-word; font-weight:700">Teleport Vive</span>和<span style="word-wrap:break-word; font-weight:700">Parabolic Pointer</span>组件都会自动添加一个<span style="word-wrap:break-word; font-weight:700">Border
 Renderer</span>组件。<span style="word-wrap:break-word; font-weight:700">Border Renderer</span>仅仅生成并渲染出显示<span style="word-wrap:break-word; font-weight:700">ViveNav Mesh</span>边界的网&#26684;和SteamVR游戏区域。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><br style="word-wrap:break-word">
</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="word-wrap:break-word; font-weight:700"><span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">第一步：配置Vive NavMesh</span></span></span></div>
<div align="center" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><img id="aimg_104665" src="http://img.manew.com/data/attachment/forum/201606/16/210217tkxdpnfdmdggtpm4.png.thumb.jpg" alt="" style="word-wrap:break-word">&nbsp;<br style="word-wrap:break-word">
</span></span></div>
<br style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">从添加<span style="word-wrap:break-word">Vive Nav Mesh</span>对象开始，可以在Assets文件夹中的<span style="word-wrap:break-word">Vive-Teleporter/Prefabs/Navmesh.prefab</span>路径下找到一个预配置的Vive
 NavMesh。可以将这个对象放在场景层级面板的任何地方和场景中的任何位置。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">接下来需要在Unity中烘焙一个导航网&#26684;(&quot;Navmesh&quot;)。这个可以在Navigation窗口中完成(Window &gt; Navigation)。</span></span></div>
<br style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">这里有几点需要考虑：</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><span style="word-wrap:break-word; font-weight:700">系统自动剔除斜坡导航网&#26684;三角形</span>。这意味着任何没有直接面向上的部分的导航网&#26684;都会被传送系统忽略。这个在VR中是合理的，因为玩家不能走上斜坡！</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><br style="word-wrap:break-word">
</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><span style="word-wrap:break-word; font-weight:700">必须在所以可传送表面使用物理碰撞器。</span>抛物线的点（见下边第二步）使用物理射线来确定玩家指向。因此所有可传送表面必须有碰撞器（包括像墙这种不可传送的表面并且要阻止指示）。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><br style="word-wrap:break-word">
</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">为不可传送区域<span style="word-wrap:break-word; font-weight:700">分配不同的导航区域</span>也是个不错的主意。这个对于优化（因此当玩家选择传送位置时系统不需要渲染巨大的预览网&#26684;）和游戏平衡（这样玩家就不会传送到地图以外了）很有帮助。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><br style="word-wrap:break-word">
</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">烘焙完导航网&#26684;之后（使用Navigation窗口底部的“Bake”按钮），回到之前创建的<span style="word-wrap:break-word">Vive Nav Mesh</span>&nbsp;对象。如果决定专用的导航区域（见上方），可以通过<span style="word-wrap:break-word">Area
 Mask</span>属性选择那些区域是可传送的。然后点击检视面板中的“Update Navmesh Data”按钮，就会看到导航网&#26684;显示在场景视图中。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><br style="word-wrap:break-word">
</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="word-wrap:break-word; font-weight:700"><span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">第二步：配置ParabolicPointer</span></span></span></div>
<div align="center" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="word-wrap:break-word; font-weight:700"><span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><img id="aimg_104666" src="http://img.manew.com/data/attachment/forum/201606/16/210217oi7x171h4fz5y51i.png.thumb.jpg" alt="" style="word-wrap:break-word">&nbsp;<br style="word-wrap:break-word">
</span></span></span></div>
<br style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<br style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">接下来添加<span style="word-wrap:break-word">Parabolic Pointer</span>&nbsp;对象。可以在Assets文件夹中&nbsp;<span style="word-wrap:break-word">Vive-Teleporter/Prefabs/Pointer.prefab路径下找到一个预配置的指针。可以将它放在场景层级面板的任何地方和场景中的任何位置。</span></span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">你当然可以修改Parabolic Pointer脚本中的任何设置了，不过只允许设置其中的一个：配置从第一步的<span style="word-wrap:break-word">ViveNav Mesh对象</span>到指针的“Nav Mesh”。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><br style="word-wrap:break-word">
</span></span></div>
<span style="font-family:微软雅黑; word-wrap:break-word; color:rgb(105,105,105); font-size:16px; line-height:25px"><span style="font-size:14px; word-wrap:break-word"><span style="word-wrap:break-word; font-weight:700"><span style="color:#333333; word-wrap:break-word">第三步：配置Vive
 Teleporter</span></span></span></span>
<div align="center" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="color:#333333; word-wrap:break-word"><span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><img id="aimg_104667" src="http://img.manew.com/data/attachment/forum/201606/16/210218a8nnocsnnnkagnns.png.thumb.jpg" alt="" style="word-wrap:break-word">&nbsp;<br style="word-wrap:break-word">
</span></span></span></div>
<br style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">最后需要为<span style="word-wrap:break-word; font-weight:700">SteamVR Camera</span>添加一个<span style="word-wrap:break-word">Vive Teleporter</span>&nbsp;（Component &gt;
 Vive Teleporter &gt; Vive Teleporter）组件。这是用来渲染Vive显示的摄像机。如果是使用了SteamVR 插件中的<span style="word-wrap:break-word">[CameraRig]</span>预制件则应该将<span style="word-wrap:break-word">Vive Teleporter</span>&nbsp;添加给那个预制件中的<span style="word-wrap:break-word">Camera (eye)</span>&nbsp;对象。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><br style="word-wrap:break-word">
</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word">接下来为组件属性配置以下&#20540;：</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><span style="word-wrap:break-word; font-weight:700"><span style="word-wrap:break-word">Pointer</span></span><span style="word-wrap:break-word; font-weight:700"><span style="word-wrap:break-word">：</span></span>将这个设置为第二步创建的<span style="word-wrap:break-word">Parabolic
 Pointer</span>&nbsp;对象。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><span style="word-wrap:break-word; font-weight:700"><span style="word-wrap:break-word">OriginTransform</span></span><span style="word-wrap:break-word; font-weight:700"><span style="word-wrap:break-word">：</span></span>将这个设置为追踪空间的起点。如果使用了SteamVR插件，这个就是<span style="word-wrap:break-word">[CameraRig]</span>&nbsp;游戏对象。当玩家传送时这个对象是实际移动的。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><span style="word-wrap:break-word; font-weight:700"><span style="word-wrap:break-word">HeadTransform</span></span><span style="word-wrap:break-word; font-weight:700"><span style="word-wrap:break-word">：</span></span>将这个设置为玩家的头部。这个应该是<span style="word-wrap:break-word">Origin
 Transform</span>的子集。如果使用了SteamVR插件，这个是<span style="word-wrap:break-word">Camera (head)</span>&nbsp;游戏对象。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><span style="word-wrap:break-word; font-weight:700"><span style="word-wrap:break-word">Navmesh Animator</span></span><span style="word-wrap:break-word; font-weight:700"><span style="word-wrap:break-word">：</span></span>将这个设置为第一步创建的<span style="word-wrap:break-word">Vive
 Nav Mesh</span>&nbsp;对象的动画。</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><span style="word-wrap:break-word; font-weight:700"><span style="word-wrap:break-word">Fade Material</span></span><span style="word-wrap:break-word; font-weight:700"><span style="word-wrap:break-word">：</span></span>将这个设置为<span style="word-wrap:break-word">Vive-Teleporter/Art/Materials/FadeBlack.mat</span>中的材质</span></span></div>
<div align="left" style="word-wrap:break-word; color:rgb(105,105,105); font-family:'microsoft yahei'; font-size:16px; line-height:25px">
<span style="font-family:微软雅黑; word-wrap:break-word"><span style="font-size:14px; word-wrap:break-word"><span style="word-wrap:break-word"><span style="word-wrap:break-word; font-weight:700">Controllers</span></span><span style="word-wrap:break-word; font-weight:700">：</span>将SteamVR控制器对象填到这里。如果使用了SteamVR的&nbsp;<span style="word-wrap:break-word">[CameraRig]</span>&nbsp;预制件，则应该将<span style="word-wrap:break-word">Controller(left)</span>&nbsp;和<span style="word-wrap:break-word">Controller(right)</span>&nbsp;两个对象填到这里。</span></span></div>
   
</div>

<h1>这是上面要用到的插件：</h1>
<h2>SteamVR</h2>
<h2>Vive-Teleporter</h2>
http://pan.baidu.com/s/1bo1ii3X

<!-- Baidu Button BEGIN -->




