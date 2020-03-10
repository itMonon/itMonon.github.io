---
layout: post
category: Unity3D-Daily
title: 【Unity3D】发布错误：could not allocate memery：system out of memery！
tagline: by 恬静的小魔龙
tag: Unity3D
---


<p style="margin-top:5px; margin-bottom:5px; font-family:微软雅黑; font-size:14px; line-height:21px; widows:1">
<span style="font-family:'font-size:16px;'; background-color:inherit"><img src="https://img-blog.csdn.net/20170925233610351?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcTc2NDQyNDU2Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""><br>
</span></p>
<p style="margin-top:5px; margin-bottom:5px; font-family:微软雅黑; font-size:14px; line-height:21px; widows:1">
<span style="font-family:'font-size:16px;'; background-color:inherit"><br>
</span></p>
<p style="margin-top:5px; margin-bottom:5px; font-family:微软雅黑; font-size:14px; line-height:21px; widows:1">
<span style="font-family:'font-size:16px;'; background-color:inherit">可能出现的原因：</span></p>
<div style="font-family:微软雅黑; font-size:14px; line-height:21px; widows:1"><span style="font-family:'font-size:16px;'; background-color:inherit">1、项目太大了</span><br style="background-color:inherit">
<span style="font-family:'font-size:16px;'; background-color:inherit">2、项目坏了</span><br style="background-color:inherit">
<span style="font-family:'font-size:16px;'; background-color:inherit">3、资源坏了</span><br style="background-color:inherit">
<span style="font-family:'font-size:16px;'; background-color:inherit">4、单个资源定点数超了e。</span></div>
<div style="font-family:微软雅黑; font-size:14px; line-height:21px; widows:1"><span style="font-family:'font-size:16px;'; background-color:inherit">&nbsp;</span></div>
<div style="font-family:微软雅黑; font-size:14px; line-height:21px; widows:1"><span style="background-color:inherit"><span style="font-family:'font-size:16px;'; background-color:inherit">解决办法：删除了一些模型。是模型太大，面数、顶点数太多的原因。</span></span></div>
<div style="font-family:微软雅黑; font-size:14px; line-height:21px; widows:1">&nbsp;</div>
<div style="font-family:微软雅黑; font-size:14px; line-height:21px; widows:1"><span style="font-family:'font-size:medium;'; background-color:inherit"><span style="line-height:1.5; background-color:inherit"><span style="background-color:inherit">Unity3d里查看模型的顶点数</span></span></span></div>
<div style="font-family:微软雅黑; font-size:14px; line-height:21px; widows:1"><span style="font-family:'font-size:medium;'; background-color:inherit"><span style="line-height:1.5; background-color:inherit"><span style="background-color:inherit">展开fbx模型，点击里面的mesh文件，就能看到顶点数。比如下面的：1052
 verts，1471 tris，uv，skin。</span></span></span></div>
<div style="font-family:微软雅黑; font-size:14px; line-height:21px; widows:1"><span style="font-family:'font-size:medium;'; background-color:inherit"><span style="line-height:1.5; background-color:inherit"><span style="background-color:inherit"><img src="https://img-blog.csdn.net/20170925233624383?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcTc2NDQyNDU2Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt=""><br>
</span></span></span></div>
<div style="font-family:微软雅黑; font-size:14px; line-height:21px; widows:1"><span style="font-family:'font-size:medium;'; background-color:inherit"><span style="line-height:1.5; background-color:inherit"><span style="background-color:inherit"><img src="file:///C:/Users/Administrator/AppData/Local/YNote/data/qq765464B75A4CF4308749BC1DF7EEB220/ab0664d84ea6445da7c85a3b82c9d3be/00576378476.jpeg" alt="" style="display:inline-block; margin-top:8px; max-width:800px; height:auto!important; background-color:inherit"></span></span></span></div>
