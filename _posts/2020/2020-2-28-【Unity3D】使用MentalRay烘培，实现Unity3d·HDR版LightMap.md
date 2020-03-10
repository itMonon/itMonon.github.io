---
layout: post
category: Unity3D-Daily
title: 【Unity3D】使用MentalRay烘培，实现Unity3d·HDR版LightMap
tagline: by 恬静的小魔龙
tag: Unity3D
---


<h1>虚拟现实，房产精装间，使用MentalRay烘培，实现Unity3d·HDR版LightMap</h1>                                                          
<br />更新一个百度网盘的下载地址，包含所有的rar文件。迅雷地址不使用。
<br /><a href="http://pan.baidu.com/s/1i3Dylj3 " target="_blank">http://pan.baidu.com/s/1i3Dylj3</a>
<br />以下为单独的ShaderAndScript.rar，之前下过的同学可能没有含有这个rar。
<br /><a href="http://pan.baidu.com/s/1hqpdJBe " target="_blank">http://pan.baidu.com/s/1hqpdJBe</a>
<br />//2013.07.25 
应大伙需求再分享一些资源，方便学习和研究。
ShaderAndScript.rar &nbsp; 

压缩包里含有4个Shader和1个镜面脚本，2张贴图，1张CubeMap，分别为：
<br />1.MGKJ_Base_1.02.shader，基本材质，场景默认所有物体都可以使用这个材质。
<br />2.MGKJ_Cubemap_1.00.shader，带CubeMap反射的材质（假反射），应用在有反射的物体，如圆形的罐子等曲面物体，画框玻璃。
<br />3.MGKJ_Glass_Base_1.00.shader,玻璃材质，表现窗户玻璃。
<br />4.MGKJ_Mirror_Base_1.00.shader，镜面反射（真实反射），可以使用一张Normal图来表现粗糙的反射面。
<br />5.MirrorReflection.cs，镜面反射脚本。有平面物体A，将MirrorReflection.cs挂载到A物体，给A物体MGKJ_Mirror_Base_1.00.shader，脚本能自动生成镜面反射贴图到材质球上。
<br />6.Normalmap_512.tga,一张表现粗糙的Normap图（在Unity中将图片格式设置为Normap）
<br />7.Nromal_map_flat.tga，一张平的Normap图（在Unity中将图片格式设置为Normap）
<br />8.MyCubeMap.cubemap，一张室内的CubeMap；

<br />Unity3D Demo。.rar
<br />3dsMax 到出的FBX和烘培好的LightMap,EXR 版 .rar
<br />3dsMax 原文件及贴图 .rar
<br />虚拟现实精装流程（MentalRay商业化烘培流程）.pdf &nbsp;

<br />置顶的4个链接都本教程需要用的资源和最终的DEMO
<br />1.最终的Demo，需运行在Diretx11下 ，可以通过 “-”和“+”调节亮度，通过&quot;[&quot;和“]” 调整对比度
<br />2.包含了Unity 场景中的FBX模型文件，DiffuseMap 和烘培好的LightMap(Exr)
<br />3.3dsMax原工程文件
4. 一个MentalRay烘培流程，主要介绍Mentalray使用技巧和Max烘培场景整理，已商业化应用，应用实践已有两年。
PS: 用到软件版本：#3dsmax2012 &nbsp;#unity3d 4.1 #PhotoShop CS5

<br />从事房产虚拟现实有几年了，在烘培和光影上有些心得，拿出来和大家分享下，促进行业的发展。

<br />这里主要想接受怎么实现在unity3d LinnerSpace下HDR版LightMap的烘培，为了实现现在看到的效果走了好多弯路也查阅了许多资料，先看看最后能呈现的效果

<br /><span id="J_att_10915"><span id="td_att_10915" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：maxrender.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/1306/thread/7_1_7710f1c0c6b506a.png" border="0" onload="if(this.offsetWidth>
<br />上图为3dsMax渲染效果，未添加光泽效果
<br />
<br /><span id="J_att_10907"><span id="td_att_10907" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：Default.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/thumb/1306/thread/7_1_7af4adb5ea3b219.png" border="0" onload="if(this.offsetWidth>
<br />上图为Unity3d 最终的效果
<br />
<br /><span id="J_att_10916"><span id="td_att_10916" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：Unity_01.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/thumb/1306/thread/7_1_b6269fac7a96c8f.png" border="0" onload="if(this.offsetWidth>
<br />上图为Unity3d 实时改变亮度和对比度——01
<br />
<br /><span id="J_att_10917"><span id="td_att_10917" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：Unity_02.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/thumb/1306/thread/7_1_8ab9ee5ae3eb66c.png" border="0" onload="if(this.offsetWidth>
<br />上图为Unity3d 实时改变亮度和对比度——02
<br />
<br /><span id="J_att_10918"><span id="td_att_10918" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：Unity_03.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/thumb/1306/thread/7_1_bb50dc3f105bcd6.png" border="0" onload="if(this.offsetWidth>
<br />上图为Unity3d 实时改变亮度和对比度——03
<br />
<br /><span id="J_att_10919"><span id="td_att_10919" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：Unity_04.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/thumb/1306/thread/7_1_ab88c00f7c55c35.png" border="0" onload="if(this.offsetWidth>
<br />上图为Unity3d 实时改变亮度和对比度——04
<br />
<br /><span id="J_att_10920"><span id="td_att_10920" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：Unity_05.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/thumb/1306/thread/7_1_1d8122b2567ea09.png" border="0" onload="if(this.offsetWidth>
<br />上图为Unity3d 实时改变亮度和对比度——05
/>+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
<br />以下为具体流程，因为内容比较多，介绍最重要的几步，有问题可提问。
<br />
<br />
<br />
<br /><span id="J_att_10911"><span id="td_att_10911" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：HDR_Line.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/1306/thread/7_1_b83e883dbba46c7.png" border="0" onload="if(this.offsetWidth>
<br />上图：先看图理解下为什么需要HDR才能实现真确的DiffuseMap×Lightmap。因为如果不使用HDR普通的图片最高亮度只能为1，所以在普通烘培贴图的时候超过1的全部被裁切到1.
<br />
<br />
<br />
<br /><span id="J_att_10912"><span id="td_att_10912" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：Max00.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/1306/thread/7_1_d0c313a6f3b5933.png" border="0" onload="if(this.offsetWidth>
<br />上图：指定渲染器为Mental ray 渲染器
<br />
<br />
<br />
<br /><span id="J_att_10913"><span id="td_att_10913" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：Max01.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/thumb/1306/thread/7_1_12eba63b7022f4f.png" border="0" onload="if(this.offsetWidth>
<br />
<br />上图 ：指定Gamma校正，Gamma校正是为了在Unit3yd 中使用线性空间渲染 （Linear Color Space）
<br />
<br />
<br /><span id="J_att_10914"><span id="td_att_10914" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：Max02.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/thumb/1306/thread/7_1_daed8fac7487375.png" border="0" onload="if(this.offsetWidth>
<br />
<br />上图： 指定烘培的图为.exr，指定为全浮点格式
<br />
<br />
<br />上述几步设定正确后烘培出来的exr才是HDR版本。
<br />
<br />
<br />++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
<br />以下为Unity3d 中设置，以使烘培的LightMap能正确的显示
<br /><span id="J_att_10908"><span id="td_att_10908" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：Editor01.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/1306/thread/7_1_0c1d11205e73ad0.png" border="0" onload="if(this.offsetWidth>
<br />
<br />上图：设置颜色空间，Dircct3d 11 这个勾选因为自己使用到了Dircct3d 11的shader ，没使用可以不勾选
<br />
<br />
<br /><span id="J_att_10909"><span id="td_att_10909" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：Editor02.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/1306/thread/7_1_ba8f64e8813d4ac.png" border="0" onload="if(this.offsetWidth>
<br />
<br />
<br />上图：将exr版的贴图的Texture Type 选为 Lightmap，必须选，不然不会转换为HDR（Unity3d 目前支持的最大亮度值为8）
<br />
<br /><span id="J_att_10910"><span id="td_att_10910" class="J_attach_img_wrap single_img"><div class="img_info J_img_info"><p>图片：Editor03.png</p></div><img class="J_post_img" src="http://www.ceeger.com/forum/attachment/thumb/1306/thread/7_1_d7f2bd4d8e14e0d.png" border="0" onload="if(this.offsetWidth>
<br />
<br />
<br />上图：配合特殊Shader实现正确的现实。（原理和Unity3d 中自带的LightMap工作方式是一样的）
<br />
<br />
<br />目前为止就能正确的实现HDR的LihgtMap。但是要想达到理想的效果Max中的模型制作和灯光测试，还有uinty3d最后的效果调节都需要用心制作才能达到好的效果。</div>



        

   
