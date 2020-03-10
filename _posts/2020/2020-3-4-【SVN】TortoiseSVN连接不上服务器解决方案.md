---
layout: post
category: web
title: 【SVN】TortoiseSVN连接不上服务器解决方案
tagline: by 恬静的小魔龙
tag: Other
---

这篇文章只是讲述我的SVN的问题，以及解决方案，希望有相同问题的人可以在这篇文章找到思路。


问题描述：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190124095939326.png)
1.防火墙已经关闭
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190124100130484.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
2.可以正常ping 服务器的连接地址
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190124100203472.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
3.然后通过浏览器可以正常访问
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190124100222625.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

但是点击浏览 版本库浏览器，或者SVN检出的时候就会一直卡着
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190124100359376.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)



解决办法：
设置->网络->不勾选 使用代理服务器
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190124100437661.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
Setting->NetWork->
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190124100508298.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

OK 解决
