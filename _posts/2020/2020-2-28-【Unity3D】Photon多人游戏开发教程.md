---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Photon多人游戏开发教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

##一、前言
Photon Unity Networking（首字母缩写PUN）是一个Unity多人游戏插件包。它提供了身份验证选项、匹配，以及快速、可靠的通过我们的Photon后端实现的游戏内通信。

##二、原文
原文地址：http://bbs.gameres.com/forum.php?mod=viewthread&tid=797805&extra=page=1&filter=sortid&sortid=6 
原文作者：liefeng603 
原文出处：GameRes游资网
PS：图文来源于网络，如有侵权请联系作者删除

##三、正文
Photon多人游戏开发教程 
![这里写图片描述](https://img-blog.csdn.net/20180723172505332?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
##<font color="red">PUN介绍</red>
###入门
Photon Unity Networking（首字母缩写PUN）是一个Unity多人游戏插件包。它提供了身份验证选项、匹配，以及快速、可靠的通过我们的Photon后端实现的游戏内通信。

PUN输出几乎所有Unity支持的平台，且有两种选项：
![这里写图片描述](http://di.gameres.com/attachment/forum/201803/09/120118k8sdrmee5d5hhmxs.png)
注意：对于Unity 5，两个PUN插件包都含相同的文件。你可以买PUN+ 来获得60个月的100 CCU，但客户端上仍使用PUN Free。
<font color="red">PUN、PUN+和UNet的对比</font>
![这里写图片描述](http://di.gameres.com/attachment/forum/201803/09/120157zfzqqqnqqyv5aayx.jpg)
<font color="red">一些必须的代码</font>
要充分使用PUN，你将需要写一些脚本。本页向你展示入门的最重要部分。

你也应该花一些时间来通过Marco Polo Tutorial。
###连接
C#代码示例：

```
PhotonNetwork.ConnectUsingSettings("v4.2");
```
上面的代码是你需要连接并开始使用Photon功能的所有代码。

<font color="red">ConnectUsingSettings </font>设置你的客户端的游戏版本并使用一个由PUN设置向导写入的配置文件，该配置文件保存在<font color="red">PhotonServerSettings</font>里面。
###匹配
接下来，你想加入现有的房间或创建自己的。下面的代码显示了启动或加入游戏的可能方法调用。

```
//加入名为"someRoom"的房间
PhotonNetwork.JoinRoom("someRoom");
//如果没有开放的游戏就会失败。错误回调: OnPhotonJoinRoomFailed
  
//尝试加入任何随机游戏：
PhotonNetwork.JoinRandomRoom();
//如果没有开放的游戏就会失败。错误回调: OnPhotonRandomJoinFailed
  
//创建名为"MyMatch"的房间。
PhotonNetwork.CreateRoom("MyMatch");
//如果名为"MyMatch"的房间已存在就会失败并调用:OnPhotonCreateRoomFailed
```

好朋友常常想要一起玩游戏。如果他们可以交流(例如 使用Photon Chat, Facebook), 他们可以瞎编一个房间名并使用JoinOrCreateRoom方法。因为他们知道房间的名字，他们可以创建为他人不可见，像这样：

C#代码示例：

```
 RoomOptions roomOptions = new RoomOptions() { isVisible = false, maxPlayers = 4 };
 PhotonNetwork.JoinOrCreateRoom(nameEveryFriendKnows, roomOptions, TypedLobby.Default);
```
使用<font color="red"> JoinOrCreateRoom </font>方法，如果房间不存在就会创建该房间。如果房间满了， <font color="red">OnPhotonJoinRoomFailed </font>会被调用 (如果你在某个地方实现了这个回调函数)。
###游戏
GameObjects可以被实例化为"networked GameObjects"。它们会有一个可以被识别的<font color="red">PhotonView</font>组件和一个所有者（或控制者）。所有者会更新其他人。持续更新可以通过拖拽一个脚本到一个<font color="red">PhotonView的 Observed</font>字段被发送。需要更新的脚本必须实现<font color="red">OnPhotonSerializeView</font>像这样:

```
// 在一个"observed"[ "observed"，被观察的。] 脚本里:
public void OnPhotonSerializeView(PhotonStream stream, PhotonMessageInfo info)
{
      if (stream.isWriting)
      {
          Vector3 pos = transform.localPosition;          stream.Serialize(ref pos);
      }
      else
      {
          Vector3 pos = Vector3.zero;
          stream.Serialize(ref pos);  // pos被填充。必须在某个地方使用
      }
}
客户端可以为不见用的操作执行Remote Procedure Calls:
// 定义一个可以被其他客户端调用的方法：
[PunRPC]
public void OnAwakeRPC(byte myParameter)
{
       //Debug.Log("RPC: 'OnAwakeRPC' Parameter: " + myParameter + " PhotonView: " + this.photonView);
}
// [...]
// 在别的某个地方调用该RPC
photonView.RPC("OnAwakeRPC", PhotonTargets.All, (byte)1);
```
独立于GameObjects, 你也可以发送你自己的事件:

```
PhotonNetwork.RaiseEvent((byte)eventCode, (object)eventContent,(bool)sendReliable, (RaiseEventOptions)options)
```
##<font color="red">初始设置</font>
Photon Unity Networking (PUN)真的很容易设置。把PUN导入到一个新的项目中，然后PUN设置向导就会弹出来，如图0-1所示。通过输入一个邮箱地址来注册一个新的(免费) Photon Cloud帐号，或者复制粘贴一个已有的<font color="red">AppId</font>到该字段里。打完收工。

如果你想要自己托管一个Photon服务器，点击"skip"，然后像如下描述的那样编辑 <font color="red">PhotonServerSettings</font>。
![这里写图片描述](http://di.gameres.com/attachment/forum/201803/09/131243a4tz6ns6v3tsstun.png)
要连接,你只需在你的代码中调用 <font color="red">PhotonNetwork.ConnectUsingSettings()</font>。如果你需要更多的控制，详见下面的 <font color="red"> Connect Manually</font>。
###Photon服务器设置
设置向导会添加一个 <font color="red">PhotonServerSettings</font>文件到你的项目，用来保存配置。如图0-2所示，这也是去编辑服务器设置的地方。
![这里写图片描述](http://di.gameres.com/attachment/forum/201803/09/131243o43sqlxhfqjq5op5.png)
你可以设置AppId、Photon Cloud Region和更多的。你的客户端的Game Version是在代码里被设置的。

要选择的最重要的选项是托管类型。
###托管类型
通过<font color="red">Hosting Type</font>你选择处理你游戏的服务器和其他配置。

<font color="red">Photon Cloud</font>和<font color="red">Best Region</font>都涉及到我们管理的云服务。您可以选择特定区域，也可以让客户选择最佳ping区域。

如果你想在别的地方运行Photon服务器，选择<font color="red">Self Hosted</font>。安装程序如下。

或者，你的客户可以在脱机模式。

最佳托管区域

最佳区域模式将在应用首次启动的时候ping所有已知区域。由于这需要一点时间，结果被存储在PlayerPrefs。这会加快连接时间。

你可以设置哪些区域可以忽略。在更少的区域分发客户端会导致剩余区域的玩家更多。这在游戏流行之前是有益的。

使用<font color="red">PhotonNetwork.OverrideBestCloudServer()</font>来定义要使用的另一个区域。
###自托管
<font color="red">如果你为iOS开发游戏可以考虑阅读 PUN and IPv6</font>和how to setup Photon Server for IPv6。

如果你要自己托管一个Photon服务器，你应在PhotonServerSettings里面设置好它的地址和端口。当这些都被正确设置了，你可以在你的代码里调用<font color="red">PhotonNetwork.ConnectUsingSettings()</font>。

确保您的客户端可以到达输入的地址。它可以是一个公共的、静态的IP地址、主机名或在你的客户端也使用的网络中的任何地址。

端口取决于所选协议，所以请确保这两个字段匹配。清除该字段会将其重置为默认端口。

###协议
这里默认是（可靠的）UDP，但Photon还支持使用TCP以及将允许一个可靠的HTTP协议。

我们建议你坚持UDP。PUN+不支持TCP。WebGL导出只能使用WebSockets。
###客户端设置
客户端设置部分包含了每个项目应设置的几个选项。

当你勾选<font color="red">Auto-Join Lobby</font>时，PUN将在连接（或离开房间）时自动加入默认大厅。Photon的大厅提供当前房间的列表，这样玩家可以选择一个加入。这个默认是关闭的，因为更好的选择是使用随机匹配，就像所有的演示案例中使用的那样。

启用<font color="red">Enable Lobby Stats</font>来从服务器获取大厅统计信息。如果游戏使用多个大厅，并且你想要向玩家展示每一个活动，则这个统计信息会很有用。每个大厅，你都可以获取这些属性: name、type、room和playercount。详见<font color="red">PhotonNetworking.LobbyStatistics</font>!

这些设置在PUN v1.60版本引入。

远程过程调用列表

<font color="red">Remote Procedure Calls</font>使你可以在一个房间里调用所有客户端上的方法。PUN 将这些方法的列表保存在PhotonServerSettings。对于最初的设置，这是不相关的。详见Remote Procedure Calls。
###手动连接
作为替代自动连接的<font color="red">PhotonNetwork.ConnectUsingSettings()</font>方法你可以通过<font color="red">PhotonNetwork.ConnectToMaster()</font>方法来手动连接你自己的Photon服务器。当你托管付费Photon服务器时这是有用的。

对于<font color="red">ConnectToMaster(),你需要提供一个<font color="red">masterServerAddress</font>和一个<font color="red">port</font>参数。地址可以是你的On-Premises DNS名称或一个IP。它可以包括冒号后的端口（然后传递0作为端口）或您可以单独通过端口。

<font color="red">ConnectToMaster()</font>方法有更多的另外两个参数 : "appID"和"gameVersion"。两者都只与Photon Cloud有关，并且当你自己托管Photon服务器时，可以设置为任何值。

对于Photon Cloud, 使用<font color="red">ConnectUsingSettings()</font>方法。它涉及到我们的Name Server自动找到一个区域的主服务器。
###<font color="red">功能概述</font>
###内容提要
- PUN插件
- 连接和主服务器
- 版本控制
- 创建和加入游戏
- MonoBehaviour回调函数
- 在游戏房间里发送消息
- Photon视图组件
- 观察Transform
- 观察MonoBehaviour
- 远程过程调用
- RPCs和加载关卡的时机
###PUN
当你导入PUN时，设置向导窗口会弹出来。如何设置请看导入PUN与设置小节。

PUN由相当多的文件组成, 然而只有一个是真正重要的: <font color="red">PhotonNetwork</font>。这个类包含所有需要的函数和变量.。如果您有自定义要求，可以随时修改源文件。

<font color="red">要从UnityScript中使用PUN,你需要把 "PhotonNetwork"和"UtilityScripts" 文件夹移动到AssetsPlugins文件夹。</font>

为了告诉你这个API如何工作，这里有几个例子。
###连接
PhotonNetwork始终使用主服务器和一个或多个游戏服务器。主服务器管理当前可用的游戏并进行匹配。一旦房间被发现或创建，实际的游戏是在游戏服务器上完成的。

所有的服务器都运行在专用的机器上，没有所谓的玩家托管的服务器。你不必费心记住该服务器组织，PUN会为你处理它。

C#代码示例

```
PhotonNetwork.ConnectUsingSettings("v1.0");
```
上面的代码是你需要连接并开始使用Photon功能的所有代码。<font color="red">ConnectUsingSettings</font> 设置你的客户端的游戏版本并使用一个由PUN设置向导写入的配置文件，该配置文件保存在<font color="red">PhotonServerSettings</font>里面。你也可以修改文件PhotonServerSettings 属性来连接到你自己的服务器。或者，使用Connect()方法来忽略该PhotonServerSettings 文件。
###版本控制
Photon的负载均衡逻辑使用你的AppId来区分你的和他人的游戏。<font color="red">玩家也会被游戏版本分开,ConnectUsingSettings</font>的参数(见上文)。通过这种方式，您可以发布新功能的客户端，而不破坏旧版本的游戏。

由于我们不能保证不同PUN的版本之间相互兼容，PUN把它自己的版本号添加到你的游戏里。更新PUN可能会从旧的版本中分离出新的客户端，但不会打破老客户端。

##<font color="red">创建和加入游戏</font>
接下来，你想加入或创建一个房间。下面的代码展示了一些必要的函数：

```
//加入一个房间
PhotonNetwork.JoinRoom(roomName);  
  
//创建这个房间。
PhotonNetwork.CreateRoom(roomName);  
// 如果该房间已存在则会失败并调用： OnPhotonCreateGameFailed 
  
//尝试加入任何随机游戏：
PhotonNetwork.JoinRandomRoom();  
//如果没有匹配的游戏则会失败并调用： OnPhotonRandomJoinFailed
```
在最好的情况下，您的游戏使用随机配对。JoinRandomRoom()将尝试加入任何房间。如果该方法失败了（没有房间接受另一个玩家），只需创建一个新的房间，并等到其他玩家随机加入它为止。

或者，您的客户端可以获得当前可用的房间列表。这是通过加入一个大厅来获得的。

大厅自动发送他们的房间列表到客户端，并在时间间隔内更新（从而减少流量）。玩家不会看到对方，且无法沟通（以防止当您的游戏繁忙时出问题）。

PhotonNetwork插件可以在其连接时自动加入默认大厅。把PhotonServerSettings文件里的"Auto-Join Lobby"属性开启即可。

当你的客户端在一个大厅里时,房间列表会得到更新, 这些更新会缓存。如果需要的话，你可以通过GetRoomList方法来每一帧访问房间列表。

C#代码示例：

```
foreach (RoomInfo room in PhotonNetwork.GetRoomList()) 
{ 
      GUILayout.Label(room.name + " " + room.playerCount + "/" + room.maxPlayers); 
}

```
关于匹配的更多信息请参考Matchmaking And Room Properties。

##<font color="red">回调函数</font>
<font color="red">PhotonNetwork</font>使用多个回调函数来让你的游戏知道状态的变化，如“已连接”或“已加入一个游戏”。像往常对Unity一样，回调可在任何脚本里实现。

如果你的脚本扩展<font color="red">Photon.PunBehaviour</font>, 你可以单独重写每个回调。在这种情况下，您不必调用基类实现。

C#代码示例：

```
public override void OnJoinedRoom()
{
      Debug.Log("OnJoinedRoom() called by PUN: " + PhotonNetwork.room.name);
}
```
你不需要扩展PunBehaviour。如果你在其本身身上实现它所有的回调函数也会起作用。它们也在枚举<font color="red">PhotonNetworkingMessage</font>中被列出和描述。

这包括建立游戏房间的基础知识。接下来是游戏中的实际交流。
##<font color="red">发消息</font>
在一个房间里，你可以发送网络信息给其他连接的玩家。此外，您还可以发送缓冲消息，也将被发送到未来连接的玩家（以玩家生成为例）。

发送消息可以使用两种方法。无论是RPCs，还是通过在一个由PhotonView观察的脚本里实现<font color="red">OnSerializePhotonView。

然而有更多的网络互动。你可以监听一些网络事件的回调函数，如<font color="red">OnPhotonInstantiate</font>或<font color="red">OnPhotonPlayerConnected</font>，并且你可以触发其中一些事件，如 <font color="red">PhotonNetwork.Instantiate</font>。如果你被最后一段弄糊涂了，不要担心，下一步我们会为这些主题逐个做解释。
##<font color="red">Photon视觉同步组件</font>
PhotonView是一个用于发送消息(RPCs和<font color="red">OnSerializePhotonView</font>)的脚本组件。你需要将PhotonView依附到游戏对象或预设上。请注意，PhotonView和Unity的NetworkView非常相似。

整个过程，你的游戏中需要至少一个PhotonView，才能发送消息和可选的实例化/分配其他的PhotonViews。

如图下图所示，添加一个PhotonView到一个游戏对象，只需选择一个游戏对象并使用: "Components/Miscellaneous/Photon View"。
![这里写图片描述](http://di.gameres.com/attachment/forum/201803/09/132514pezr922u2b20jj22.png)
<center>图 0-1 Photon Cloud：Photon View</center>
###观察Transform
如果你将一个Transform绑定到PhotonView的观察属性上，你可以选择同步位置、旋转和尺度或玩家的这些属性组合。这可以极大的帮助制作原型或小游戏。注意：任何观察到的值变化将发送所有观察到的值-而不只是发生变化的那个单一值。此外，更新的值是不平滑的或插值。
###观察MonoBehaviour
PhotonView可以被设置来观察MonoBehaviour。在这种情况下，脚本的OnPhotonSerializeView方法会被调用。此方法被调用来写入对象的状态并读取它，这取决于脚本是否由本地玩家控制。

下面简单的代码展示了如何用几行代码来增加角色状态同步：

C#代码示例：

```
void OnPhotonSerializeView(PhotonStream stream, PhotonMessageInfo info)
{
      if (stream.isWriting)
      {          //我们拥有这个玩家：把我们的数据发送给别的玩家
          stream.SendNext((int)controllerScript._characterState);
          stream.SendNext(transform.position);
          stream.SendNext(transform.rotation);
      }      else
      {
          //网络玩家，接收数据
         controllerScript._characterState = (CharacterState)(int)stream.ReceiveNext();
         correctPlayerPos = (Vector3)stream.ReceiveNext();
         correctPlayerRot = (Quaternion)stream.ReceiveNext();
      }
}
```
###观察选项
Observe Option字段让你选择更新如何发送以及何时被发送。该字段还会影响到OnPhotonSerializeView被调用的频率。

Off 顾名思义，关掉。如果该PhotonView被保留为RPCs限定时可以很有用。

Unreliable 更新如是被发送，但可能会丢失。这个想法是，下一次更新很快到来，并提供所需的正确的/绝对的值。这对于位置和其他绝对数据来说是有利的，但对于像切换武器这样触发器来说是不好的。当用于同步的游戏对象的位置，它会总是发送更新，即使该游戏对象停止运动（这是不好的）。

Unreliable on Change 将检查每一个更新的更改。如果所有值与之前发送的一样，该更新将作为可靠的被发送，然后所有者停止发送更新直到事情再次发生变化。这对于那些可能会停止运动的以及暂时不会创建进一步更新的游戏对象来说是有利的。例如那些在找到自己的位置后就不再移动的箱子。

Reliable Delta Compressed 将更新的每个值与它之前的值进行比较。未更改的值将跳过以保持低流量。接收端只需填入先前更新的值。任何你通过OnPhotonSerializeView写入的都会自动进行检查并以这种方式被压缩。如果没有改变， OnPhotonSerializeView不会在接收客户端调用。该“可靠的”部分需要一些开销，所以对于小的更新，应该考虑这些开销。

现在开始，以另一种方式交流：RPCs。

###<font color="red">远程过程调用</font>
Remote Procedure Calls (<font color="red">RPC</font>)使你可以调用"networked GameObjects"上的方法，对由用户输入等触发的不常用动作很有用。

一个<font color="red">RPC</font>会被在同房间里的每个玩家在相同的游戏对象上被执行，所以你可以容易地触发整个场景效果就像你可以修改某些<font color="red">GameObject</font>。

作为<font color="red">RPC</font>被调用的方法必须在一个带<font color="red">PhotonView</font>组件的游戏对象上。该方法自身必须要被[<font color="red">PunRPC</font>]属性标记。

```
[PunRPC] 
void ChatMessage(string a, string b) 
{ 
      Debug.Log("ChatMessage " + a + " " + b); 
}
```
要调用该方法，先访问到目标对象的PhotonView组件。而不是直接调用目标方法，调用PhotonView.RPC()并提供想要调用的方法名称：

```
PhotonView photonView = PhotonView.Get(this); 
photonView.RPC("ChatMessage", PhotonTargets.All, "jup", "and jup!");
```
 你可以发送一系列的参数，但它必须匹配该RPC方法的定义。

这些是最基本的。详情请阅读Remote Procedure Calls.
###<font color="red">时机</font>
RPCs在指定的PhotonViews上被调用，并总是以接收客户端上的匹配者为目标。如果一个远程客户端还没有加载或创建匹配的PhotonView，这个RPC就会丢失！

因此，丢失RPCs一个典型的原因就是当客户端加载新场景的时候。它只需要一个已经加载有新游戏对象的场景的客户端，并且其他客户端不能理解这个RPC（直到这些客户端也加载了相同的场景）。

PUN可以帮你解决此问题。只需在你连接之前设置<font color="red">PhotonNetwork.automaticallySyncScene = true</font>并在房间的主客户端上使用 <font color="red">PhotonNetwork.LoadLevel()</font>。这样，一个客户端定义了所有客户端必须在房间/游戏中加载的关卡。

客户端可以停止执行接收到的消息来防止RPCs丢失（这正是LoadLevel方法帮你做的）。当你得到一个RPC来加载一些场景，立即设置<font color="red">isMessageQueueRunning = false</font>直到该内容被初始化。

例子:

```
private IEnumerator MoveToGameScene()
{
      // 加载关卡前临时禁用进一步的网络信息处理
      PhotonNetwork.isMessageQueueRunning = false;
      Application.LoadLevel(levelName);
}
```
禁用消息队列将延迟传入和传出消息，直到队列被解锁。显然，当你准备好要继续的时候，打开队列是非常重要的。