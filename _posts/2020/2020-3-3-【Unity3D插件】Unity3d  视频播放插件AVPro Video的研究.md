---
layout: post
category: Unity3D-Plugin
title: 【Unity3D插件】Unity3d  视频播放插件AVPro Video的研究
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
Avpro Video是一款由RenderHeads出品的可以在Unity上安装使用的万能多平台视频播放插件,Avpro Video支持Windows,linux,ios,mac,Android等多平台万能播放。不仅可以实现基础的播放功能，还能实现进度条拖放和速率调整，播放4K视频，360度全景视频等，并对不同的平台进行了优化。
![这里写图片描述](https://img-blog.csdn.net/20180614163735895?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 二、参考资料
http://www.onlinedown.net/soft/987730.htm
https://blog.csdn.net/yiwei151/article/details/78415771
https://blog.csdn.net/dark00800/article/details/56015917

## 三、下载链接
CSDN下载链接
https://download.csdn.net/download/q764424567/10561111
GitHub下载链接
https://github.com/764424567/Unity-plugin/blob/master/AVPro%20Video.unitypackage

## 四、如何安装使用
**基于UGUI的视频播放功能**

1.  将下载的unitypackage导入工程，可以看到以下几个文件夹
![这里写图片描述](https://img-blog.csdn.net/20180614164531992?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

2. 创建Media Player对象，在Hierarchy视图右击或者选择菜单栏的GameObject菜单，然后选择AVPro Video->Media Player
![这里写图片描述](https://img-blog.csdn.net/201806141646592?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180614165240660?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

Media Player对象包括基础设置
- Media Properties（视频的图像和音频设置）
- Global Settings（全局设置）
- Preview（预览，只在Play时有效）
- Events（回调事件）
- Platform（多平台重写）
- About（插件信息）
其中我们一般需要进行设置的只有基础设置和Events。

![这里写图片描述](https://img-blog.csdn.net/20180614171411163?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

-  Absolute Path Or URL :绝对路径或者URL，path写服务器的路径，但是记得写视频的后缀名，这个时候就可以在线播放视频了
- Relative To Project Folder:相对于项目文件夹的路径，这个因为打包之后项目的相对路径就变了，需要不同平台，设置不同参数
- Relative To StreamingAssets Folder:这是视频文件的最佳和最常见的位置。这个文件夹位于。“Assets/streamingassets/”，如果它不存在，你必须创建它。复制到这个文件夹的文件不会被Unity导入或处理，但是它们会自动复制到构建中。
- Relative To Data Folder:数据文件夹是由Unity指定的
- Relative To Peristent Data Folder:持久数据文件夹由Unity指定


3. 设置Medie Player的参数
![这里写图片描述](https://img-blog.csdn.net/20180614171707277?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180614171359601?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- 注意一下的是Video Location这个选项
- 推荐使用StreamingAssets文件夹
- 需要在项目的Assets目录下新建一个StreamingAssets文件夹，然后将视频拖进去，StreamingAssets中的文件不会被打包到程序资源包中，而是作为一个独立的目录自动生成在输出的程序目录的Data目录下
- Recent选项可以快速选择之前选择过的文件
- Browse选项可以快速选择电脑中的文件

4.创建AVPro Video对象，在Hierarchy视图右击或者选择菜单栏的GameObject菜单，然后选择UI->AVPro Video
![这里写图片描述](https://img-blog.csdn.net/20180614171456610?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
参数这么设置就行
![这里写图片描述](https://img-blog.csdn.net/20180614171638325?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

OK 视频就可以播放了


## 五、如何在手机上在线播放视频

同样的步骤添加Media Player组件之后，设置Media Player组件的参数
![这里写图片描述](https://img-blog.csdn.net/20180614172309683?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
将VideoLocation改为URL，VideoPath改为你服务器的路径，但记得写视频的后缀。这个时候就可以在PC上播放在线视频了


**安卓设置**

当然如果我们想要在安卓上播放的还需要更改一些设置；
![这里写图片描述](https://img-blog.csdn.net/20180614172437309?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
在这个切换为安卓平台的，同时需要在PlayerSetting中设置一些参数。

因为这个插件支持的最低Level为16，所以需要改为16，同时修改下面的参数
　　![这里写图片描述](https://img-blog.csdn.net/20180614172449138?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这样我们就可以在安卓上播放在线视频了。
## 六、插件的官方文档翻译
将下载的unitypackage导入工程，导入后会看到几个demo和插件的说明文档AVProVideo-UserManual
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020011311053810.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
详细的插件用法包括视频格式的支持和API在说明文档中都有。
文档下载链接：
https://github.com/764424567/Unity-plugin/blob/master/AVProVideo-UserManual.pdf
下面就是文档的内容，因为是全英文的，就给翻译了一下

**内容**
 1. 介绍
	 1.1  产品特点
	 1.2 试用版
	 1.3 版权说明
 2. 系统要求
	 2.1 平台不支持
 3. 安装
	 3.1 平台的说明
	 3.2  试用版和水印
	 3.3 视频文件位置
	 3.4 流文件
	 3.5 虚拟现实的说明
	 3.6 Hap编解码器的说明
	 3.7 幻灯片的说明
 4. 快速入门示例
	 4.1 快速启动从Unity开始
	 4.2 使用Prefabs快速启动全屏幕视频播放器
	 4.3 使用组件快速启动3D网格视频演示
 5. 使用
	5.1 使用指南
	5.2 不受支持的平台
	5.3 组件
			  &nbsp; &nbsp; &nbsp; &nbsp;Ⅰ Media Player 组件
			  &nbsp; &nbsp; &nbsp; &nbsp;Ⅱ Display IMGUI 组件 
			  &nbsp; &nbsp; &nbsp; &nbsp;Ⅲ Display uGUI 组件
			  &nbsp; &nbsp; &nbsp; &nbsp;Ⅳ Apply To Mesh / Material 组件
	5.4 脚本
		      &nbsp; &nbsp; &nbsp; &nbsp;Ⅰ Namespace
		      &nbsp; &nbsp; &nbsp; &nbsp;Ⅱ Media Player Scripting
		      &nbsp; &nbsp; &nbsp; &nbsp;Ⅲ Events
 6. 资源文件
	6.1 Demos
	6.2 Prefabs
	6.3 Scripts
 7. 支持的媒体格式
	7.1 Android
	7.2 iOS,tvOS, and OS X
	7.3 Windows
 8. 脚本参考
 9. 支持
 10. 关于 RenderHeads Ltd
	附录A - FAQ
	附录B - 版本历史
	附录C - 路线图
	
	
### 1.介绍
AVPro视频是来自渲染头的最新视频插件，它之前开发过。AVPro QuickTime和AVPro Windows媒体插件用于Unity。在新一代的ugins中，我们的目标是创建一个易于使用的跨平台视频播放系统，该系统使用每个平台的特性。

#### 1.1 产品特点
- Version for iOS,tvOS,OS X,Android,Windows,Windows Phone  and UMP
- One API for video playback on all supported platforms
- 4K Video support (on supported hardware)
- VR support (equirectangular hardware)
- Fast fiexible video playback
- Unity Pro 4.6.9 and 5.x supproted
- Free watermaked trial version avaiable(download here)
- Fast native Direct3D,OpedGL and Metal texture updates
- Linear and Gamma colour spaces supported
- Graceful fallback in editor
- Unity 4.5 uGUI support
- Easy to use ,drag and drop components
- Streaming video from URL (when supported by platform)

#### 1.2 试用版

我们提供了一个无限试用版的AVPro视频，可以从我们的网站http://renderheads.com/product/av和亲视频中下载。试用版没有遗漏的特性或时间限制，但它确实对渲染的输出应用了水印。水印确实有很小的性能影响，这在高分辨率的视频中是非常明显的。在Windows中，如果在没有显示水印的情况下使用GPU解码路径，那么每隔几秒，视频的大小就会缩小。

#### 1.3 媒体版权

BigBuckBunny-360p30.mp4-（c）版权2008，Blender基金会

/www.bigbuckbunny.org。


BigBuckBunny_720p30.mp4-（c）版权2008，Blender基金会

/www.bigbuckbunny.org

SampleSphere.mp4-（c）版权Daniel Arnett，

https://vimeo.com/97887646

### 2.系统要求

- Unity 5.x or Unity Pro 4.6 and above 
- iOS 8.1 and above
- tvOS 9.0(Apple TV 4th Gen) and above
- OS X 10.7 and above,64-bit only
- Android 4.1 (Jelly Bean ,API level 16) and above(ARM7 and x86)
- Windows XP and above(32-bit and 64-bit)
- Windows 8.0 and above(32-bit and 64-bit)
- Windows Phone UMP 8.1(32-bit and 64-bit)
- Window Desktop UMP 8.1(32-bit ,64-bit and ARM)
- Universal Windows Platform 10(32-bit,64-bit and ARM)

#### 2.1不支持的平台
- WebGL*
- WebPlayer
- Linux desktop
- Game COnsoles(XBox,PS4 etc)




### 3. 安装
	1.打开一个新的Unity会话（清除任何锁定的插件文件）
	2.将unitypackage文件导入到Unity项目中。如果提示升级一些脚本，请单击Yes。

#### 3.1 平台说明

##### 3.1.1 Android
	-  这个插件需要API的等级最低16
	-  使用的是MediaPlayer API
	-  如果你想支持流媒体，别忘了设置“互联网接入”选项。播放器设置“需要”
	-  对于渲染，我们支持OpenGL ES 2.0和OpenGL ES 3.0
	-  支持多线程渲染

##### 3.1.2 iOS / tvOS / Mac OS X
-  在引擎盖下，我们使用的是AVFoundation API
- 如果您想要支持流媒体，您需要显式地启用HTTP下载。对于iOS来说，这是新版本Unity的一个选项。但是对于Mac OS X和旧版本的Unity，你必须通过编辑plist文件来显式地做到这一点。下面是关于如何做到这一点的注释
- 对于Mac OS X的渲染我们支持OpenGL Legacy和OpenGL核心
- 对于iOS和tvOS的渲染我们支持OpenGL ES 2.0，OpenGL es3.0和金属
- 支持多线程渲染

##### 3.1.3 Windows
- 在引擎盖下，我们使用媒体基金会和DirectShow APl。内侧Foundation用于Windows 8和其他版本，而DirectShow则用作Windows 7和下面的回退。
- 对于渲染我们支持Direct3D 9，Direct3D 11和OpenGL Legacy
- 支持多线程渲染

##### 3.1.4 Windows Store / UMP / Hololens
- 对于GPU解码，将这一行添加到app.cs：
  m_AppCallbacks.AddCommandLienArg("-force-d3d11-no-snglethreaded");

#### 3.2 试用版和水印说明

##### 3.2.1 水印
   如果你使用的是这个插件的试用版，你会看到一个水印在视频中显示出来。水印的形式是“渲染头”的标志，它在屏幕上显示，或者是在屏幕上移动的厚水平条。AVPro视频的完整版本在任何平台上都没有水印。
    如果你使用一个特定于平台的软件包（比如iOS的AVPro视频，或者Windows的AVPro视频），那么你将不会看到你购买的平台上的水印，但是你会看到其他平台上的水印。例如，如果你为iOS购买了AVPro视频，那么你仍然可以看到Unity编辑器中的水印，因为这是在windows/os X上运行的，但是当你部署到iOS设备时播放的视频将是无水的。

##### 3.2.2 从试用版更新
   
   如果您正在从试用版升级，请确保删除旧/资产/plugins文件夹，因为它包含试用插件，并且可能会发生冲突。您可能需要先关闭Unity，然后手动删除文件，然后重新启动Unity并重新导入软件包（因为Unity在加载后就会锁定本地插件文件）。
      您可以通过在场景中添加一个MediaPlayer组件来检查您安装了哪个版本，并单击该组件的检查员的“关于”按钮。版本号显示在这个框中。

##### 3.2.3 安装多个AVPro平台包

如果你已经安装了iOS包，那么它也会包含所有其他的插件。平台，但启用了水印。这意味着，如果您试图安装另一个AVPro包，它可能不会正确地覆盖插件。下面是如何使用iOS和Android包来解决这个问题：

	1.开始一个Unity新项目
	2.导入iOS的包
	3.删除Plugins/Android 文件夹
	   a.如果你已经安装了其他的Android插件，那么你就不能删除整个文件夹，并且必须特别删除AVPro文件。查看一下AVPro原生nlugin的“helow”列表
    4.导入Android包
     类似操作适用于其他包
     
本地的pluain文件列表：
- Plugins/Android/AVProVideo.jar
- Plugins/Android/libs/armeabi-v7a/libAVProLocal.so
- Plugins/Android/libs/x86/libAVProLocal.so
- Plugins/AVProVideo.bundle
- Plugins/iOS/libAVProVideoiOs.a
- Plugins/tvOS/libAVProVideotvOS.a
- Plugins/WSA/PhoneSDK81/ARM/AVProVideo.dll
- Plugins/WSA/PhoneSDK81/x86/AVProVideo.dll
- Plugins/WSA/SDK81/ARM/AVProVideo.dll
- Plugins/WSA/SDK81/x86/AVProVideo.dll
- Plugins/WSA/SDK81/x86_64/AVProVideo.dll
- Plugins/WSA/UMP/ARM/AVProVideo.dll
- Plugins/WSA/UMP/x86/AVProVideo.dll
- Plugins/WSA/UMP/x86_64/AVProVideo.dll
- Plugins/x86/AVProVideo.dll
- Plugins/x86_64/AVProVideo.dll


#### 3.3 Video本地文件夹

视频文件几乎可以在任何位置播放，但是我们建议在Unity项目中放置视频文件，因为这是最容易开始的文件夹。StreamingAssets是一个特殊的文件夹，在没有处理的情况下，统一复制到构建。在其他地方复制的文件将需要手动复制到构建位置。MediaPlayer组件允许您浏览视频文件，并将它们与父文件夹相关联：
![这里写图片描述](https://img-blog.csdn.net/20180615092747530?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
视频定位字段指定视频文件的主位置，而视频路径。•菲尔德指定在何处查找与位置有关的文件。

例如，如果您的文件存储在“Assets/StreamingAssets/Video.mp4”中你会设置。定位到“相对于流媒体资产文件夹”，并将视频路径设置为“vedio.mp4”
子文件夹也支持一个视频“Assets/ StreamingAssets / myfolder /video.mp4“会有它的视频路径设置。“myfolder / video.mp4”。
您还可以指定相对于其他位置的绝对路径、url或路径：

##### 3.3.1 相对于资源文件夹
这是视频文件的最佳和最常见的位置。这个文件夹位于。“Assets/StreamingAssets/”，如果它不存在，你必须创建它。复制到这个文件夹的文件不会被Unity导入或处理，但是它们会自动复制到构建中。

##### 3.3.2 绝对路径或者URL
。在这里，您可以为视频文件指定一个完整的URL或绝对路径。一个URL可以是“http://myserver.com/myvideo.mp4”或“rtsp：//myserver.com:8080/mystream.rtsp”，这取决于所使用的平台支持和流媒体服务。


一条绝对路径看起来是这样的:
- C:/MyFolder/AnotherFolder/MyVido.mp4(Windows)
- /Users/Mike/downloads/MyVideo.mp4(Mac/Linus)
- /Storage/SD/Videos/MyVideo.mp4(Android external SDCARD)
- /Storage/emulated/0/MyFolder/MyVideo.mp4(android local file system)

……使用绝对路径对于测试来说是很有用的，但是在部署到其他没有相同文件结构的机器时是没有用的。

##### 3.3.3 相对于项目文件夹
项目文件夹是您的Unity项目的文件夹，所以包含资产的文件夹。图书馆和项目设置子文件夹。当vou不想在Unity资产文件夹中包含视频文件时，指定与项目文件夹相关的文件是很有用的，但是希望将它们保存在项目文件夹结构中。经常做一个叫做“视频”的子文件夹是有用的。在这个位置上的一个可能的问题是，当makina构建vour视频文件不会自动复制到构建目的地时，所以它们需要手动复制。


对于构建这个文件夹应该位于：

- Windows - 在与你的EXE相同的级别
- Mac -与应用程序包中的内容文件夹相同
- iOS 和AppName的级别相同。应用/数据文件夹
- Android - 除非你手动构建APK，否则无法访问APK

##### 3.3.4 相对于资源文件夹

数据文件夹是由Unity指定的：

http://docs.unity3d.com/ScriptReferencelApplication-dataPath.html

把视频文件直接放到这个文件夹里是没有用的，因为它们会被统一处理成电影材质，并且会使你的项目规模膨胀。如果你想停止Unity的处理，视频文件只是简单地将扩展名重命名为Unity不理解的东西，所以“myvideo”。mp4“可以重命名为myvideo.mp4.bin”。数据文件夹中的文件（编辑器中的资产文件夹）不会自动复制到构建中，因此您必须手动复制它们。

##### 3.3.5 相对于持续化数据的文件夹

持久数据文件夹是Unity设置的
http://docs.unity3d.com/ScriptReference/Application-persistenDataPath.html


#### 3.4 流文件说明
AVPro视频支持多个基于平台的流媒体协议：
![这里写图片描述](https://img-blog.csdn.net/20180615094130105?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 3.5 HTTP流

在为流媒体播放MP4视频时，确保它们在文件开始时使用视频头数据进行编码。你通常通过选择“快速启动”来做到这一点。QuickTime编码器，或者在FFMPEG中使用“-移动标志快速启动”，其他编码器将有类似的选项。要使用FFMPEG来准备一个MP4，您可以使用以下命令：
	
	ffmped -i %1 -acodec copy -vcodec copy -movflags faststart %1 -streaming.mp4

如果你是流媒体视频在URL中"mp4"部分特别有用如果你使用HLS流媒体播放从VIMEO作为MP4，你应该注意到你可以替换vith。m3u8“让它变成一个HLS流。这可能是为苹果应用商店开发应用程序，因为你需要认证（截止到2016年4月）。

##### 3.4.1 OS X,iOS and tvOS 流文件

这个平台支持HLS流的流，通常以m3u或m3u8扩展结束。

如果vou有一个HTTPS URL，它应该可以正常工作，因为苹果信任安全连接。

如果你只能使用HTTP那么你的应用就必须有一个特殊的标志来让它使用HTTP

连接（这是苹果的安全问题）。

这个设置在iOS和tvOS的Unity播放器设置中被曝光：

![这里写图片描述](https://img-blog.csdn.net/20180615094453214?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

该设置也在脚本API中公开：
http://docs.unity3d.com/ScriptReference/PlayerSettings.iOS-allowHTTPDownload.html

如果出于某种原因，你的Unity版本没有公开这一点，那么你将不得不手动添加它。在Unity编辑器中，你需要编辑“Unity.app/content/info”。在您的构建应用程序中，您需要编辑“您的.app/content/info.plist”。这些文件需要添加这些kevs：
<key>NSAppTransportSecurity</key>
<dict>
<key>NSAllowsArbitraryLoads</key>
</dict>
你可以在这里找到更多的信息：

http://ste.vn/2015/06/10/configuring-app-transport-security-ios-9-osx-10-11/

我们还包括一个名为“PostProcessBuild”的post过程构建脚本。cs”项目•对plist进行编辑并添加该属性。目前，它只在iOS平台上设置，但你也可以在顶部编辑定义，让Mac OS X也可以。

##### 3.4.2 Android流文件
要求将internet访问设置(在播放器设置中)设置为Required
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113111250232.png)
##### 3.4.3 测试数据流
我们发现这些流方便测试(不保证他们仍然工作):

- Streaming Mp4
http://downloads.renderheads.com/2016/BigBuckBunny_360p30_Streaming. mp4
- HLS
○ http://184.72.239.149/vod/mp4:BigBuckBunny_115k.mov/playlist.m3u8 
○ http://qthttp.apple.com.edgesuite.net/1010qwoeiuryfg/sl.m3u8
- RTSP
rtsp://rtmp.infomaniak.ch/livecast/latele

#### 3.5 VR/AR说明
到目前为止，我们已经在下面平台测试AVPro视频:
-  Oculus Rift 
-  Google Cardboard 
-  Gear VR 
-  HTC Vive 
-  Microsoft Hololens

VR仍然是非常新的，你应该总是检查最新的建议安装步骤时，创建你的项目。我们在网上找到很多过时的安装说明。AVPro视频支持4K MP4播放，创造360度体验。立体声4K视频在顶部底部和并排的格式都是alsol支持的有关实现VR高分辨率视频回放的提示，请参阅FAQ。降低编码视频的复杂性将使解码引擎更容易，并可能导致更高的帧率和更低的CPU/GPU使用率。可能的编码调整包括:

- 使用尽可能低的配置
- 文件级别不要使用太多的参照系
- 不要使用太多的框架
- 禁用CABAC

##### 3.5.1 VR立体声
AVPro视频支持上下左右格式的立体视频。
你可以在媒体属性面板中设置视频的立体包装格式:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113111828940.png)
现在，当使用在一个网格上的球内着色器，它会自动映射正确的par的视频到每个前夕。查看“Demo_360SphereVideo”场景，了解它是如何工作的。
包含的着色器“InsideSphere.shader“允许你轻松设置什么格式的视频是通过一个下拉式菜单选择的材料:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113111953314.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
选择“立体声调试着色”来着色左边和右边不同的颜色，这样你可以确保立体声工作注意:在使用Unity 5.3或更低版本或Unity 5.4或更高版本时，在不启用单通道VR选项的情况下，一定要在场景中添加“UpdateStereoMaterial”组件脚本。

通常立体VR需要2个摄像头，每个摄像头设置一个不同的图层蒙版，2个球体也设置一个不同的蒙版。AVPro视频不需要这个，只需要使用普通的单摄像头和单球体。

##### 3.5.2 VR 音频
一些VR系统，如Oculus Rift，有自己的音频输出设备，AVPro视频有一个选项“强制音频输出设备”的Windows(目前只在DirectShow播放模式)，允许你指定这个音频设备的名称:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113112658983.png)
闹剧音频输出i要使用的设备名称可以从VR API中检索或硬编码。Oculus Rift的名字通常是“Rift音频”，而HTC Vive则是“HTC Vive USB音频”。

#### 3.6 Hap编解码器
Hap编解码器是本地支持的，对于超高分辨率的视频，它的CPU使用率极低。AVI和MOV容器都可以使用。Hap仅支持Windows和Mac OS X平台。

##### 3.6.1 Windows 支持
目前，只有Hap和Hap Alpha变种的Hap被支持用于带有HapQ的Windows，而未来带有Alpha的HapQ也将得到支持。Hap目前要求选择“Force DirectShow”选项:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113112829385.png)
##### 3.6.2 MAC OS X 支持
支持Hap, Hap Alpha和Hap Q

#### 3.7 透明度说明
没有多少视频编解码器支持透明/ alpha通道。AVPro视频部分平台支持的格式为:
- Hap Alpha
在Windows和Mac OS x上的强大支持。快速和低开销的fomat，尽管文件大小可以变得很大取决于内容。目前这是我们为透明视频推荐的ormat
- Uncompressed RGBA
未压缩不是理想的文件大小或磁盘带宽，但仍然可以作为备份使用。
- Uncompressed YUVA
未压缩不是理想的文件大小或磁盘带宽，但仍然可以作为备份使用。
- ProRes 4444
最好的支持是在Mac OS x上。
- VP6
遗留格式。我们只支持它通过第三方DirectShow插件的Windows(如LAV过滤器)

### 4.快速入门示例
#### 4.1 快速启动：在Unity专家版本的快速启动
1. 将视频文件放到StreamingAssets文件夹中
2.  使用MediaPlayer脚本播放视频(将视频路径设置为视频文件的文件名)
3. 使用其中一个显示脚本显示您的视频(如DisplayIMGUI, DisplayUGUI。ApplytoMaterial)
#### 4.2 快速启动 : 全屏视频播放器，采用预制件
AVPro视频包括许多示例预制块，您可以使用它们轻松地将视频回放添加到您的项目中。
以下步骤将创建一个应用程序，播放全屏视频:
1. 创建一个新的Unity项目
2.  导入AVProVideo包
3. 从项目窗口的AVPro/Prefabs文件夹，draq的全屏视频预制到你的层次结构窗口

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113113359856.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
4. 在您的项目窗口中创建一个名为StreamingAssets的文件夹，并将您的文件(比如MP4文件)复制到该文件夹中
5. 在MediaPlayer组件的视频路径字段中输入文件名(包括扩展名)
6. 构建和部署您的应用程序，视频将displaved fullscreer6Displayl MGUI组件脚本只是用于显示视频的组件之一。


Displayl MGUI组件脚本只是用于显示视频的组件之一。它使用传统的Unity IMGUI系统，它总是呈现在所有其他东西之上。如果你不想让你的视频在顶部，尝试使用DisplayBackground或DisplayUGUI组件来获得更多的控制。

#### 4.3 快速启动:3D Mesh视频播放器使用组件
AVPro视频包括许多易于使用的脚本组件，你可以添加到你的场景。在这个例子中，我们展示了如何使用组件在材质上播放视频，材质被应用到场景中的3D模型上。

1. 创建一个新的统一程序
2.  导入AVProVideo包
3. .创建一个新的游戏对象从“游戏对象> AVPro视频>媒体播放器”菜单命令点击“添加组件”按钮
4. 添加“AVPro Video > Apply To Mesh”
5.  媒体播放器脚本在应用到网格脚本的“媒体”字段，这告诉应用到网格脚本的媒体播放器使用
6. 通过"游戏对象 + 3D 对象 + 球体"命令菜单创建球体
7. 拖动网格渲染器组件到“网格”字段在应用到网格脚本，这告诉应用到网格脚本使用哪个网格
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113113655448.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
8. 在项目窗口中创建一个名为StreamingAssets的文件夹，并将视频文件(比如MP4文件)复制到该文件夹
9.  在MediaPlayer组件的视频路径字段中输入文件名(包括扩展名)
10. 构建和部署您的应用程序，视频将显示在vour 3D sphere

### 5. 使用
#### 5.1 开始
最简单的入门方法是查看包含的演示，并查看使用了哪些脚本组件。对于视频回放，你需要在你的场景中做三件事:
1. 播放的视频文件:在项目窗口中创建一个“StreamingAssets”文件夹复制您的视频文件(通常是MP4文件，但请参考以下平台支持的格式列表)到StreamingAssets文件夹
2.  加载并播放视频的媒体脚本创建一个GameObject并将MediaPlayer脚本添加到它将视频路径字段设置为视频文件的名称(e.q. myvideo.mp4)
3. 显示视频的脚本:决定你想要你的视频文件如何和在哪里出现。针对不同的使用场景，包含了许多不同的显示组件脚本。如果你想在你的场景中显示视频，只需将DisplaylMGUI脚本添加到场景中的游戏界面中，并设置媒体播放器组件。其他显示组件的工作方式也类似。

#### 5.2 不支持的平台备份
AVPro视频被设计成即使在没有本地支持的平台上也能正常工作，而不是显示实际的视频。所有的视频控制仍然可以工作。例如，如果您在Linux中运行编辑器，则虚拟视频播放器将出现在编辑器中，而真正的视频将在部署到受支持的平台时出现。如果部署到不受支持的平台，如三星电视，还会看到虚拟视频播放器。该代码易于扩展，可以为任何不受支持的平台添加自定义视频播放器。
#### 5.3 组件
为了使这个资产易于使用，包含了许多componentb。组件位于AVProVideo/Scripts/ components文件夹中，也可以从组件菜单中添加:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113114030493.png)
##### 5.3.1 Media Player组件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113114041974.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
这是播放媒体的核心组件。此组件只处理媒体的加载和回放，而不处理如何显示它。使用显示脚本组件控制视频的显示方式和位置。字段是：

 - Video Location 
在哪里查找下面的视频路径中指定的文件。这可以是一个绝对路径/URL，或者相对于一个Unity文件夹。StreamingAssets文件夹是最容易使用的。选项是：
1.Absolute or URL 
绝对路径
2.Relative to Project Folder 
相对于项目文件夹
3.Relative to Streaming Assets Folder 
相对于StreamingAssets文件夹
4.Relative to Data Folder 
相对于数据文件夹
5.Relative to Persistent Data Folder 
相对于持有数据路径文件夹
 -  Video Path 
StreamingAssets文件夹中视频的文件路径(例如:myvideo.mp4或者AndroidVideos/myvideo.mp4(如果你想使用子文件夹)
 - Auto Open
是否在启用/启动此组件时打开文件
 - Auto Start
视频打开后是否播放视频
 - Loop
是否循环播放视频
 - Playback Rate 
设置一个乘数，影响视频播放速度
不支持android
 - Volume
0 . .1范围的音频音量
 - Muted
音频是否消音 
 - Persistent 
将DontDestroyOnLoad应用到obiect上，这样它就能承受场景/level的加载
 - Debug Gui 
是否显示对调试有用的视频回放统计信息的叠加
  - Events
此事件可以连接到脚本函数，当非循环视频完成回放时将调用脚本函数。有关更多细节和脚本示例，请参见下面的Events部分
 - Platform overrides 
这允许您为每个平台设置不同的文件。

##### 5.3.2 显示IMGUI组件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113115252876.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
这是显示视频最基本的组件。它使用传统的Unity IMGUI系统在屏幕上显示视频。IMGUI总是被渲染在场景中其他所有东西的上面，所以如果你需要你的视频被渲染在3D空间或作为uGUl系统的一部分，最好使用其他组件。字段:

- Media Player
要显示的媒体播放器
- Display In Editor 
显示在编辑器
- Scale Mode
是否在编辑器中显示矩形，对调试缩放模式有用
- Color
如何适应屏幕的屏幕颜色
- Alpha Blend
Alpha混合
- Depth
IMGUI深度显示顺序，使用这个来改变其他IMGUI脚本的渲染顺序
- Full Screen
- 全屏
- X
指定的X位置
- Y
指定的Y位置
- Width
指定的宽
- Height
指定的高

##### 5.3.3 显示UGUI组件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113115750533.png)
这个组件是用来显示一个视频使用Unity的uGUI系统。字段：

- Media Player
要显示媒体播放器
- Default Texture
视频不播放时显示的纹理（例如在缓冲中）
- No Default Display 
不会显示任何东西，直到有帧可用
- Color
颜色着色器，包括Alpha透明度
- Material
标准UGUI材质
- UV Rect
标准UGUI的UV
- Set Native Size
当视频加载将调整大小的RectTransform的像素尺寸的视频
- Keep Aspect Ratio
是否保持正确的长宽比

##### 5.3.4 适用于网格组件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113120211592.png)
该组件将媒体播放器组件生成的纹理分配到3D网格上的材质纹理槽中。这对于在3D网格上播放视频非常有用。字段：

- Mesh
网格（渲染器）应用纹理
- Media
媒体播放器
- Default Texture
当视频不播放时显示一个纹理
##### 5.3.5 适用于材质组件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113120234915.png)

- Material
应用纹理的材质
- Texture Property Name
纹理属性名（可选）：默认情况下，这个脚本分配给主纹理，但如果你想分配给另一个槽，你可以把名字放在这里
- Media
媒体播放器
- Default Texture(optional)
在视频不播放时显示的纹理

#### 5.4 脚本
##### 5.4.1 命名空间
所有脚本都使用名称空间RenderHeads.Media。所以一定要添加“使用RenderHeads.Media”。AVProVideo”到源文件的顶部。

##### 5.4.2 Media Player 脚本
大多数脚本很可能以MediaPlayer .cs脚本为中心。这个脚本处理视频的浏览、回放和更新。该脚本公开了许多与不同用例相关的接口，可以在interface .cs中找到它们
MediaPlayer公开3个主要接口:
- 信息接口：
IMedialnfo接口由Info属性公开，该接口用于访问有关媒体的信息，例如

```csharp
MediaPlayer mp;
mp.Info.GetVideolidth ();
```

- 控制接口：
IMediaControl接口是公开的控制属性，这个接口是用来控制播放，
例如:

```csharp
MediaPlayer mp;
mp.Control.Pause ();
```

- TextureProducer接口：
IMediaProducer接口由TextureProducer属性公开，该接口用于获取关于如何显示当前纹理的信息，并由显示组件使用，例如:

```csharp
MediaPlayer mp;
videoTexture = mp.TextureProducer.GetTexture();
```

MediaPlayer脚本也有许多控制媒体加载的方法:

- OpenVideoFromFile()
加载指定的视频。有用的，如果你需要手动控制何时视频加载
- CloseVideo()
关闭视频，释放内存

##### 5.4.3 事件
MediaPlayer目前有以下事件:

- MataDataReady  	
当宽度、高度、持续时间等数据可用时调用
- ReadyToPlay 		
在加载视频并准备播放时调用开始
- Started   		
播放开始时调用
- FirstFrameReady 	
第一帧已被渲染结束播放时调用
- FinishedPlaying	
当非循环视频播放完毕时调用

脚本例子：

```csharp
//Addtheeventlistener(canalsodothisviatheeditorGUI) 
MediaPlayermp; 
mp.Events.AddListener(OnVideoEvent); 

//Callbackfunctiontohandleevents 
publicvoidOnVideoEvent(MediaPlayermp,MediaPlayerEvent.EventTypeet) 
{ 
	switch(et)
 	{ 
		caseMediaPlayerEvent.EventType.ReadyToPlay: 
		mp.Control.Play(); 
		break; 
		caseMediaPlayerEvent.EventType.FirstFrameReady: 
		Debug.Log("Firstframeready"); 
		break; 
		caseMediaPlayerEvent.EventType.FinishedPlaying: 
		mp.Control.Rewind(); 
		break;
 	}
	Debug.Log("Event:"+et.ToString()); 
}
```

### 6.资源文件

#### 6.1 Demos
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113142531130.png)
- Demo_360SphereVideo.unity
1.Demo包含一个视频播放器，播放360度视频使用等矩形(latlong)映射
2.视频被应用到一个球体上，球体里面是主摄像头如果目标设备有一个陀螺仪，然后移动设备，旋转摄像头，从不同角度观看视频。
3.对于没有陀螺仪的平台，鼠标/触摸可以用来四处查看。
4.一个特殊的着色器和脚本是用来允许一个单一的摄像机在VR头盔上呈现立体声。
5.点击材质，设置视频是单屏显示、上下点选显示还是左右立体显示。
- Demo_360CubeVideo.unity
与上面的球体演示相同，但是使用的是Cubemap 3x2布局源视频。
- Demo_BackgroundShader.unity
基本演示，播放一个视频使用的背景材料，让视频梨背后的所有内容。
- Demo_FrameExtract.unity
显示去读取帧的视频保存到磁盘(jpg/png)或访问像素数据。
- Demo_imGui.unity
1.基本的演示，播放一个视频，并使用传统的IMGUI显示组件，以绘制到屏幕上的视频
2.也有一个音频剪辑，以显示音频媒体播放。
3.还有3个不同的流媒体url来演示流媒体。
4.IMGUI是在所有其他可视组件之上绘制的。
- Demo_Mapping3D.unity
1.Demo包含一个视频播放器和一个3D场景
2.有些3D模型的影像是透过pply ToMaterial手写板制作的
- Demo_Multiple.unity
这个演示允许你以编程方式加载多个视频，并测试多个视频同时播放。显示是通过AVPro的视频uGUI组件
- Demo_uGui.unity
1.这个演示演示了如何在uGUI系统中显示视频。它使用画布层次结构中的DisplayUGUI组件。
2.它也使用一个自定义着色器来覆盖
- Demo_VideoControl.unity
- 这个演示演示了如何查询视频状态和控制回放

#### 6.2 Prefabs
360Spherevideo.prefab
预置的视频播放器和映射到一个球体。适用于360度等角度视频的回放预制件包含一个视频播放器和一个四轴模型与一个特殊的背景材料应用。
BackgroundVideo.prefab
这种材料使四方得到绘制之前的一切，所以它出现在背景。
FullscreenVideo.prefab
预置控制一个视频播放器和IMGUI显示组件非常简单的基本视频playback创建
#### 6.3 Scripts
**组件**
- ApplyToMaterial.cs
将MediaPlayer组件生成的纹理应用到unity材质纹理槽中
- ApplyToMesh.cs
通过设置所有材质的mainTexture字段，将MediaPlayer组件生成的纹理应用到一个不整洁的网格中(通过MeshRenderer)
- DisplayBackground.cs
显示MediaPlayer组件在所有其他内容之后生成的纹理(与SkyBox不兼容)。
- DisplavlMGUl.cs
显示MediaPlayer组件使用Unity的遗留IMGUl系统生成的纹理DisplayuGul.cs显示MediaPlaver组件使用Unity的新uGUI svstem生成的纹理的
- MediaPlayer.cs
用于加载和控制视频回放实例的主脚本
- UpdateStereoMaterial.cs
我的一个助手脚本为VR立体渲染更新摄像机在一个球形材料的位置变量，以帮助找出哪个眼来渲染
- ApplyToTextureWidgetNGUI.cs
将MediaPlayer组件生成的纹理应用到NGUI纹理widqet纹理槽中
**Editor**
- DisplayUGUIEditor.cs
控制如何在检查器中呈现DisplayUGUI组件的编辑器脚本的
- MediaPlaverEditor.cs
控制MediaPlaver组件的编辑器脚本在检查器中呈现
**内部**
- AndroidMediaPlayer.cs
Android媒体播放器的BaseMediaPlaver.cs所有平台媒体播放器的公共基类的
- Interfaces.cs
接口和事件NullMediaPlayer.cs不支持的平台OSXMediaPlayer.cs的备份假媒体播放器iOS和OSX特定的媒体播放器的WindowsMediaPlaver.csWindows专用媒体播放器
### 7.脚本参考
AVPro视频desiąned主要与使用提供的组件拖放到但总有时候的脚本是必要的。资产包括样本场景，给出了一些例子如何使用脚本控制视频回放，应用视频纹理到材料等，这是有用的学习。完整的课程参考资料可在此下载:
http://downloads.renderheads.com/docs/AVProVideoClassReference/

**MediaPlayer class**
MediaPlayer类是视频播放的主类，是指定和控制视频文件的地方。这个类主要通过Unity Inspector Ul来控制，并通过它所公开的接口属性来编写脚本。

**属性**
- 事件
返回MediaPlaverEvent类信息
- 信息
返回IMedialnfo接口控件
- 控制
返回IMediaControl
- 纹理控制
接口的TextureProducero返回IMediaProducer接口

**方法**
所有这些方法都使用上面公开的接口，都是方便的快捷方式
- void OpenVideoFromFile(FileLocation location, string path, bool autoPlay)
打开指定的视频空白
- void CloseVideo ()
关闭当前视频并释放分配的内存空间
- void Play()
开始播放视频空白暂停()停顿的二无效
- void Stop()
停顿的无效
- void Rewind(bool pause)
使用一个选项来回放视频，同时暂停它的Texture2D
- Texture2D ExtractFrame(Texture2D target,float timeSeconds,int timeoutMs)
从当前视频的指定时间提取帧作为可读的Texture2D。这可以用来保存像素数据。纹理必须被用户破坏。纹理可以通过“target”参数再次传递来重用它

**IMedialnfo接口**
该接口用于查询视频的属性
**方法**
- float GetDurationMs ();
返回视频的持续时间，以毫秒为单位;
- int GetVideoWidth();
返回视频宽度的像素
- int GetVideoHeight();
返回视频的高度(以像素为单位)

**IMediaControl接口**
**方法**
这个接口用于控制视频的加载和回放
- bool OpenVideoFromFile (string路径)
开始从指定的路径或URL加载文件。如果遇到任何错误，则返回false。这个函数是异步的，所以视频属性不会立即可用。这个函数不应该使用，而是使用MediaPlaver OpenVideoFromFile函数
- void CloseVideo()
关闭视频和任何分配的资源
- void SetLooping(bool looping)
设置播放是否应该循环。这可以在视频播放时更改。
- bool CanPlay()
返回视频是否处于播放状态。有时视频在播放前可能需要几帧。
- void Play()
开始播放
- void Pause();
暂停
- bool Stop()
停止视频(本质上与Pause' bool IsPlaying相同);
- bool IsPlayint()
返回视频当前是否正在播放
- bool lsPaused():
返回当前视频是否暂停，
- bool IsFinished();
返回视频是否已完成回放
- bool IsBuffering()
返回流媒体视频是否已停止并正在缓冲。在下载了足够的数据后，缓冲视频将恢复。
void Rewind()
将当前时间设置为视频
- void Seek(float timeMS)
开始时间(浮动时间);
- void SeekFast (float timeMs);
将当前时间设置为一个指定的值(以毫秒为单位)，但牺牲精度以换取速度。如果你只是想在视频中向前/向后跳，但你不关心准确性，这是很有用的。
- bool IsSeeking()
返回视频当前是否正在寻找。在寻找过程中没有产生新的框架。
- float GetCurrentTimeMs ()
返回当前时间(播放位置)，以毫秒为单位;设置当前播放速率。1。0f是正常的速率。并不是所有平台都支持负利率
- float GetPlaybackRate ()
返回当前播放速率
- void MuteAudio(bool mute)
设置音频静音或不
- void SetVolume(float volume)
设置0.0到1.0之间的卷
- float GetVolume()
返回0.0到1.0之间的音量级别

**IMediaProducer接口**
**方法**
- Texture GetTexture ();
如果有纹理可用，返回一个Unity纹理对象，否则返回null。
- int GetTextureFrameCount ();
 返回插件更新纹理的次数。这对于了解每次更新纹理时的值wil增量是很有用的。
- bool RequiresVerticalFlip ()
有些纹理是上下颠倒解码的，需要在显示时垂直翻转。此方法返回显示期间是否需要翻转纹理。

### 8.支持的媒体格式
一般来说，支持的最常见格式是带有H.264编码的MP4文件，用于视频和AAC编码的音频。所有平台都支持这种格式，但不一定支持所有比特率和配置文件。
容器支持:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113150455512.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
音频编解码器支持:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113150736106.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
视频编解码器支持:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113150643420.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
VP8NP9 Windows媒体基金会的编解码器可在以下网址下载http://www.webmproject.oraliel
显然苹果很快就会发布支持
**要求勾选“Force DirectShow”选项

带有“?”的细胞是我们不确定的细胞。我们将做更多的测试，并继续更新这个表。有关哪种编解码器和哪种编码类型最好的详细信息，请参阅下面的每个平台详细信息。


#### 8.1 Android
Android支持多种媒体格式。要查看完整的列表，请查看以下Android
文档:http://developer.android.com/intl/ko/quide/appendix/media-formats.html
下表显示了一些Android设备的功能，可以作为查看支持的视频格式的指南。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113150922448.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
#### 8.2 IOS，tvOS和 OS X
iOS支持多种媒体格式，包括H.264。要查看完整的列表，请点击这里的ios
文档:https://developer.apple.com/librarv/ios/documentation/Miscellaneous/Conceptual/iPhoneQs TechOverview /MediaLayer/ MediaLaver.html
下表显示了一些iOS设备的功能，可以作为查看支持的视频格式的指南
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113151038767.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
在OS X中，支持ProRes 422和ProRes 4444
OS X及以上，支持以下附加格式:
/
未压缩的R10k
未压缩的y210
未压缩的2 yuy

#### 8.3 Windoes
支持的格式的完整列表可以在这里找到:
https://msdn.microsoft.com/en­us/library/windows/desktop/dd757927(v=vs.85).aspx https://msdn.microsoft.com/en­us/windows/uwp/audio­video­camera/supported­codecs
解码器支持高达配置文件L5.1:
https://msdn.microsoft.com/en­us/library/windows/desktop/dd797815(v=vs.85).aspx

Windows 10 添加以下格式
H.265 /HEVC
MKV
FLAC

#### 8.4 Windoes Phone
这个平台还不支持，但该平台支持的媒体的详细信息可以点击这里:

https://msdn.microsoft.com/library/windows/apps/ff462087(v=vs.105).aspx https://msdn.microsoft.com/en­us/windows/uwp/audio­video­camera/supported­codecs

### 9.支持
如果您需要支持或对该产品有任何意见/建议，请与我们联系。
电子邮件:unitysupport@renderheads.com
网站:http://renderheads.com/product/avpro-videa/
Unity 论坛:http://forum.unity3d.com/threads/released-avpro-video-complete-video-playback-solution.3856111

#### 9.1 Bug 提交
如果你正在报告一个错误，请包括任何相关的文件和细节，以便我们可以尽快修复问题。

**必要的细节:**
- 错误消息
确切的错误消息
如果可能，控制台/输出日志
如果它是一个Android构建，然后一个“adb logcat”捕获
- 硬件
手机/平板/设备类型和操作系统版本
- 开发环境
Unity的版本开发
操作系统版本
AVPro视频插件版本
- 视频详细信息
决议
编解码器
帧率
更好的是，包括一个视频文件的链接

更好的是，寄给我们一个完整的或缩小的副本，您的统一项目


### 10. 关于RenderHeads有限公司
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113151848354.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113151901301.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113151953451.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113152003373.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113152014349.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113152023875.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113152031506.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020011315205167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113152103957.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113152115892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113152118499.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113152136327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
