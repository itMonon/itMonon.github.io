---
layout: post
category: web
title: 【PUN】Photon Unity Networking(PUN)的简单使用
tagline: by 恬静的小魔龙
tag: Other
---


<div id="article_details" class="details">
    <div class="article_title">   
         <span class="ico ico_type_Original"></span>

    <h1>
        <span class="link_title"><a href="/xiaoge132/article/details/65628220">
        Photon Unity Networking(PUN)的简单使用        
           
        </a>
        </span>

         
    </h1>
</div>

   

        
  
     


<div id="article_content" class="article_content tracking-ad" data-mod=popu_307  data-dsm = "post" >
        <div class="markdown_views">

<hr>

<p><strong><a href="http://download.csdn.net/detail/xiaoge132/9792218">demo下载</a></strong></p>

<p>我们知道在untiy 5.1之后,unity自带了Unet,但是PUN的功能强大之处,更胜一筹。下面来简单介绍一下unity中使用Photon插件的方法。 <br>
<img src="http://img.blog.csdn.net/20170324103133510?" alt="这里写图片描述" title=""> <br>
当显示为Joined时候就可以点击地面,同步生成预制体Cube了。</p>

<p><img src="http://img.blog.csdn.net/20170324104834536?" alt="这里写图片描述" title=""></p>

<h2 id="在面板里我们只需要加入一个plane就可以了">在面板里我们只需要加入一个Plane就可以了</h2>

<p><img src="http://img.blog.csdn.net/20170324101743351?" alt="这里写图片描述" title=""></p>

<p>再去给它附上一个叫做Click的脚本就可以了。</p>

<pre class="prettyprint"><code class="language-C# hljs cs"><span class="hljs-keyword">using</span> UnityEngine;
<span class="hljs-keyword">using</span> System.Collections;
<span class="hljs-keyword">using</span> ExitGames.Client.Photon;
<span class="hljs-keyword">public</span> <span class="hljs-keyword">class</span> Click : MonoBehaviour
{
    <span class="hljs-keyword">public</span> GameObject prefab;
    <span class="hljs-keyword">void</span> Start()
    {

    }

    <span class="hljs-keyword">void</span> OnClick()
    {
        Debug.Log(InputToEvent.inputHitPos);
        PhotonNetwork.Instantiate(prefab.name, InputToEvent.inputHitPos, Quaternion.identity, <span class="hljs-number">0</span>);

    }

}</code></pre>

<p>这里的OnClick事件会在点击地面Plane的时候会触发事件。 <br>
在OnClick里面能看见<strong>PhotonNetwork.Instantiate</strong> 这个是Photon 插件的自带的函数，也是用于出现预制体的函数。</p>



<h2 id="在摄像机上面添加控制networking的脚本">在摄像机上面添加控制NetWorking的脚本</h2>

<p>这里是用来加入或者创建房间的，还有用来退出房间的。</p>



<h3 id="首先初始化版本号">首先，初始化版本号</h3>

<p>PhotonNetwork.ConnectUsingSettings(“1.0”); <br>
这里表示我们是1.0版本</p>



<h3 id="之后来添加两个gui一个表示加入或者创建房间一个退出房间">之后，来添加两个GUI，一个表示加入或者创建房间，一个退出房间。</h3>

<p>PhotonNetwork.JoinOrCreateRoom(“xiaogeformax”, new RoomOptions {MaxPlayers = 16}, null); <br>
创建房间最大为16人的房间</p>



<pre class="prettyprint"><code class="language-C# hljs cs"><span class="hljs-keyword">using</span> UnityEngine;
<span class="hljs-keyword">using</span> System.Collections;

<span class="hljs-keyword">public</span> <span class="hljs-keyword">class</span> PUNConnect : MonoBehaviour {


    <span class="hljs-keyword">void</span> Start()
    {
        PhotonNetwork.ConnectUsingSettings(<span class="hljs-string">"1.0"</span>);
    }
    <span class="hljs-keyword">void</span> OnGUI()
    {
        GUI.Label(<span class="hljs-keyword">new</span> Rect(Screen.width / <span class="hljs-number">2</span> - <span class="hljs-number">75</span>, <span class="hljs-number">20</span>, <span class="hljs-number">150</span>, <span class="hljs-number">20</span>), PhotonNetwork.connectionStateDetailed.ToString());
        <span class="hljs-keyword">if</span> (GUI.Button(<span class="hljs-keyword">new</span> Rect(Screen.width/<span class="hljs-number">2</span>-<span class="hljs-number">50</span>,<span class="hljs-number">50</span>,<span class="hljs-number">100</span>,<span class="hljs-number">30</span>),<span class="hljs-string">"加入游戏房间"</span> ))
        {
            <span class="hljs-keyword">if</span> (PhotonNetwork.connected)
            {
                PhotonNetwork.JoinOrCreateRoom(<span class="hljs-string">"xiaogeformax"</span>, <span class="hljs-keyword">new</span> RoomOptions {MaxPlayers = <span class="hljs-number">16</span>}, <span class="hljs-keyword">null</span>);
            }

        }

        <span class="hljs-keyword">if</span> (GUI.Button(<span class="hljs-keyword">new</span> Rect(Screen.width / <span class="hljs-number">2</span> - <span class="hljs-number">50</span>, <span class="hljs-number">150</span>, <span class="hljs-number">100</span>, <span class="hljs-number">30</span>), <span class="hljs-string">"退出游戏房间"</span>))
        {
            <span class="hljs-keyword">if</span> (PhotonNetwork.connected)
            {
                PhotonNetwork.LeaveRoom();
            }
        }
    }
}
</code></pre>

<p>我们再制作上面的预设体，就创建了个Cube，通常我们把预设体放入Project面板里就成了，但在这里我们要在我们的文件下面<strong>创建Resources文件夹，或者放在原来的Resources文件夹里面。并给Cube预设体添加PhotonView。然后在Observed Componts里面选择Cube。</strong> <br>
<img src="http://img.blog.csdn.net/20170324101655350?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2dlMTMy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="这里写图片描述" title=""></p>



<h2 id="设置photonseversetting">设置PhotonSeverSetting</h2>

<p><img src="http://img.blog.csdn.net/20170324102011119?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2dlMTMy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="注意红色方框里面的" title=""></p>

<p>红色方框里面的就是我们的配置信息了，在Hosting里面要选择PhotonCloud，表示使用了云服务器。 <br>
Region里面选择使用服务器的地区，有亚洲，美国，还有欧洲什么的。 <br>
AppID这个是比较重要的，要注册的，注册之后在这里会出现的，复制一下就可以了！ <br>
下面的Protocol我们选择使用UDP协议就可以了 <br>
<img src="http://img.blog.csdn.net/20170324102650544?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2dlMTMy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="这里写图片描述" title=""> <br>
还有些连接本地Photon服务器的一些问题，还是有些疑惑不是太能理解，以后弄懂了之后，再来修改完善。<img src="http://img.blog.csdn.net/20170324105639968?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlhb2dlMTMy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="这里写图片描述" title=""></p>

<blockquote>
  <p><a href="http://download.csdn.net/detail/xiaoge132/9792218">demo下载</a></p>
</blockquote></div>
      
