---
layout: post
category: Unity3D-Plugin
title: 【SteamVR】Unity SteamVR插件集成
tagline: by 恬静的小魔龙
tag: Unity3D
---

Unity SteamVR插件集成

**重要组件**
<div class="postText">
<div id="cnblogs_post_body">
<h4>SteamVR_Camera</h4>
<p>VR摄像机，主要功能是将Unity摄像机的画面进行变化，形成Vive中的成像画面</p>
<p>使用方法：</p>
<p>l 在任一个摄像机上增加脚本</p>
<p>l 点击Expand按钮</p>
<p><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191917600-1111937397.png"><img title="image001" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191917991-1918718458.png" alt="image001" width="244" height="241" border="0" /></a><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191918413-160257055.png"><img title="image002" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191918803-1917742481.png" alt="image002" width="244" height="240" border="0" /></a></p>
<p>完成以上操作后，原本的摄像机会变成如下结构</p>
<p><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191919131-1323191127.png"><img title="image003" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191919522-1847228351.png" alt="image003" width="188" height="78" border="0" /></a></p>
<p>l Origin：位置</p>
<p>l Head：头部</p>
<p>l Eye：眼睛</p>
<p>l Ears：耳朵</p>
<p>至此，游戏中Vive中可以看到游戏画面，360度旋转查看游戏世界，在游戏世界中移动等</p>
<p><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191919850-24462207.png"><img title="image007" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191920194-1822351401.png" alt="image007" width="244" height="115" border="0" /></a><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191920491-450238300.png"><img title="image006" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191920725-572668482.png" alt="image006" width="244" height="195" border="0" /></a></p>
<p><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191921006-831336875.png"><img title="image005" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191921600-409554863.png" alt="image005" width="178" height="501" border="0" /></a><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191922006-1388808705.png"><img title="image004" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191922334-1331688012.png" alt="image004" width="226" height="498" border="0" /></a></p>
<h4>SteamVR_ControllerManager和SteamVR_TrackedObject</h4>
<p>控制器，主要用于设置和检测Vive控制器。</p>
<p>Vive控制器由菜单键(ApplicationMenu)，触摸板(Touchpad)，系统键/电源键(System)，扳机键(Trigger)，侧柄键(Grip)，组成</p>
<p><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191922741-2008581832.png"><img title="image008" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191923038-9682255.png" alt="image008" width="244" height="200" border="0" /></a></p>
<p>1 Menu button（菜单键）</p>
<p>2 Trackpad（触摸板）</p>
<p>3 System button（系统键/电源键）</p>
<p>4 Status light</p>
<p>5 Micro-USB port</p>
<p>6 Tracking sensor</p>
<p>7 Trigger（扳机键）</p>
<p>8 Grip button（侧柄键）</p>
<p>使用方法</p>
<p>在Origin物体上添加2个子物体代表Vive的2个手柄，增加SteamVR_TrackedObject，Index设置为None</p>
<p>在Origin物体上添加SteamVR_ControllerManager，设置左右手柄</p>
<p>至此就完成了手柄的集成。</p>
<p><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191923366-698276083.png"><img title="image010" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191923756-826140605.png" alt="image010" width="244" height="87" border="0" /></a><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191924053-551270519.png"><img title="image011" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191924444-1197451997.png" alt="image011" width="244" height="104" border="0" /></a><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191924725-484107593.png"><img title="image009" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191925178-963236009.png" alt="image009" width="159" height="74" border="0" /></a></p>
<h5>获取手柄状态</h5>
<p>通过代码</p>
<p>var device = SteamVR_Controller.Input(uint);</p>
<p>device.GetTouchDown(SteamVR_Controller.ButtonMask)</p>
<p>就可以获取到某个按键的状态</p>
<p>或者使用</p>
<p>var system = OpenVR.System;</p>
<p>system.GetControllerState(uint, ref VRControllerState_t))</p>
<p>获取当前所有的按键状态</p>
<h5>手柄震动</h5>
<p>public void TriggerHapticPulse(ushort durationMicroSec = 500, EVRButtonId buttonId = EVRButtonId.k_EButton_SteamVR_Touchpad)</p>
<p>{</p>
<p>var system = OpenVR.System;</p>
<p>if (system != null)</p>
<p>{</p>
<p>var axisId = (uint)buttonId - (uint)EVRButtonId.k_EButton_Axis0;</p>
<p>system.TriggerHapticPulse(ControllerIndex, axisId, (char)durationMicroSec);</p>
<p>}</p>
<p>}</p>
<p>或者</p>
<p>var device = SteamVR_Controller.Input(uint);</p>
<p>device. TriggerHapticPulse();</p>
<h4>SteamVR_RenderModel</h4>
<p>该组件用于渲染手柄的模型，并且跟踪手柄的位置</p>
<p>使用方法</p>
<p>在左右手柄的物体下创建一个子物体，子物体上添加SteamVR_RenderModel脚本，Shader可以根据需求设置，比如设置为Standard</p>
<p><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191925491-384927569.png"><img title="image012" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191925772-1574875115.png" alt="image012" width="244" height="138" border="0" /></a></p>
<p>至此，游戏中可以看到手柄模型和手柄位置同步</p>
<h4>SteamVR_PlayArea</h4>
<p>用于显示游玩区域。</p>
<p>使用方法，在Origin物体上添加该脚本即可</p>
<p><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191926116-2004129891.png"><img title="image013" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191926459-1139466450.png" alt="image013" width="202" height="244" border="0" /></a></p>
<p>可以看到游戏场景中多了一个显示区域</p>
<p><a href="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191926819-1056136440.png"><img title="image014" src="http://images2015.cnblogs.com/blog/289969/201605/289969-20160524191927163-1773337013.png" alt="image014" width="244" height="186" border="0" /></a></p>

**注意事项**

制作UI的时候需要使用世界坐标，通过不同相机的Depth复合，目前还不支持（2016/5/23）
注意ControlIndex的值，默认情况下都会自动设置，如果手动设置错误将导致错误的表现


