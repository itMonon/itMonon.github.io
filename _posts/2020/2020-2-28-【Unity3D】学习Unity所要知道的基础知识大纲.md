---
layout: post
category: Unity3D-Daily
title: 【Unity3D】学习Unity所要知道的基础知识大纲
tagline: by 恬静的小魔龙
tag: Unity3D
---

<p>本文献给，想踏入3D游戏客户端开发的初学者。</p>

<p> </p>

<p>毕业2年，去年开始9月开始转作手机游戏开发，从那时开始到现在一共面的游戏公司12家，其中知名的包括搜狐畅游、掌趣科技、蓝港在线、玩蟹科技、天神互动、乐元素。开始做虚幻3游戏程序开发，现在转作UNITY3D。面试了12家公司大概总结一下面试的常考方向（以下排名不分先后，红色是需要着重了解的，但不仅限于此，如果有错，望指正）。</p>

<p> </p>

<p><span style="color:#3366ff;">1、面试方式</span>：所有面试，只有3家有面试题，蓝港是其中之一，题量2-4页不等；其他都是1对1,2对1，或者3对1直接面试；主要考C#、unity基础，大公司问的不难，但问得深，基础必须扎实。</p>

<p> </p>

<p><span style="color:#3366ff;">2、3D图形学、渲染</span>：<span style="color:#ff0000;">渲染管道</span>流程、3D渲染优化，延迟渲染、<span style="color:#ff0000;">Shader编程</span>。</p>

<p> </p>

<p><span style="color:#3366ff;">3、数学、数据结构</span>：二维矩阵、三维<span style="color:#ff0000;">矩阵</span>相乘、转置；<span style="color:#ff0000;">向量点乘和叉乘</span>方法和意义；<span style="color:#ff0000;">四元数、欧拉数</span>；二叉树、堆栈；线性表、链表。</p>

<p> </p>

<p><span style="color:#3366ff;">4、unity3d</span>：熟悉<span style="color:#ff0000;">NGUI</span>（原理机制）；物理引擎；<span style="color:#ff0000;">DrawCall</span>优化；AI、自动寻路；做<span style="color:#ff0000;">unity3d遇到的坑</span>（unity吭特多，只有亲手做过才知道），<span style="color:#ff0000;">协同程序</span>，<span style="color:#ff0000;">动画系统</span>；光照烘培；Trigger； 异步加载（AssetBundle.LoadAsync）。<span style="color:#ff0000;">多看API（用户手册、组件手册、脚本手册）</span></p>

<p> </p>

<p> <span style="color:#3366ff;">5、C#语言</span>：Event和<span style="color:#ff0000;">委托；</span>抽象类和接口；<span style="color:#ff0000;">垃圾回收器，多线程</span>。</p>

<p> </p>

<p><span style="color:#3366ff;">6、英语能力</span>：<span style="color:#ff0000;">外语文档阅读能力</span>顺畅，因为很多时候要去国外网站了解新技术，所以这个是做的好的关键。面试的时候，也会给你一篇英文技术文档给你翻译。</p>

<p> </p>

<p><span style="color:#3366ff;">7、思想</span>：<span style="color:#ff0000;">MVC思想</span>、<span style="color:#ff0000;">代码耦合性；</span>代码编写风格（代码头部注释、方法注释）；爱玩的游戏以及对它的<span style="color:#ff0000;">评价</span>（缺点，怎么改进更好）；如果让你设计一个MMO，你应该怎么做等。</p>

<p> </p>

<p> </p>

<p>下面举几个面试的具体例子：</p>

<p> 1、一个物体，它顶上有个摄像机，摄像机离他越来越远，最后让物体消失（不是隐藏它）。</p>

<p> 2、鼠标点击一个物体，然后屏幕上显示他的坐标和名称信息。</p>

<p>3、TCP/UDP区别</p>

<p> </p>

<p> 参看面试题：</p>

<p><a href="http://www.cnblogs.com/zhibolife/p/3680621.html">http://www.cnblogs.com/zhibolife/p/3680621.html</a></p>

<p><a href="http://www.cnblogs.com/zhibolife/p/3624916.html">http://www.cnblogs.com/zhibolife/p/3624916.html</a></p>

<p> </p>

<p>其他参考资料：</p>

<p>NGUI机制：<a href="http://www.cnblogs.com/zhibolife/p/3642000.html">http://www.cnblogs.com/zhibolife/p/3642000.html</a></p>

<p> </p>

<p>网上找到一张unity3d知识体系大纲图，可以对照着学习，有利于形成思维体系。</p>

<p> </p>

<p><img alt="" class="has" src="https://img-blog.csdn.net/20170925232800247?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcTc2NDQyNDU2Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" /></p>

<p>这是找的其他人总结的路线图</p>

<p><img alt="" class="has" src="https://upload-images.jianshu.io/upload_images/2046946-8cc870c6ecc10ef7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp" /></p>

<p><img alt="" class="has" src="https://upload-images.jianshu.io/upload_images/2046946-971878837ce716a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp" /></p>

<p><img alt="" class="has" src="https://upload-images.jianshu.io/upload_images/2046946-66ef4b358312b0b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp" /></p>

<p> </p>

<p><img alt="" class="has" src="https://upload-images.jianshu.io/upload_images/2046946-870b3845a8091046.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/704/format/webp" /></p>

<p><img alt="" class="has" src="https://upload-images.jianshu.io/upload_images/2046946-3d453edce54b6618.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/800/format/webp" /></p>
