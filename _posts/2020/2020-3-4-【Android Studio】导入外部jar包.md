---
layout: post
category: web
title: 【Android Studio】导入外部jar包
tagline: by 恬静的小魔龙
tag: Other
---

## 一、前文
Android Studio使用的也不多，很多东西都是参考别人的经验，然后进行测试使用，也会踩到很多坑，然后把测试使用的心得，以及方法总结下来

## 二、参考文章
Android Studio 简介及导入 jar 包和第三方开源库方【http://blog.sina.com.cn/s/blog_693301190102v6au.html】
Android Studio的使用（五）--导入第三方Jar包【https://www.cnblogs.com/begin1949/p/4966542.html】
Androidstudio中添加jar包的方法【https://blog.csdn.net/zhw1551706847/article/details/77709142】

## 三、正文

### 方法一、
直接复制到libs文件夹中，Add as Library一下就行了 
#### 具体步骤
一、切换到Project视图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224105624900.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

二、 找到libs文件夹
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224105717925.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

三、 将你自己要使用的jar包拖进去（复制粘贴也行）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224105808834.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224105834626.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224105850898.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

四、右击Jar文件，点击Add As Library后，在出现的弹出框点击确定即可
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224111648475.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
五、 添加成功
导入成功的jar包
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224111740527.png)
打开build.gradle文件，可以看到最后一行添加成功的代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224111906285.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
这个方法的优点是不仅可以导入一些简单的jar包，还可以作为一些不支持 Android Studio 的开源库的使用的解决方案，就是说，你把你需要使用的开源库的 jar 包拿出来，导进来即可，因为通常我们使用开源库并不会去修改其源代码。

### 方法二、
通过Project Structure添加
#### 具体步骤
点击这个
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224113106136.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
或者File->Project Structure也可以
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224113207626.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224113311385.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
点击app->Dependencies->+ ->Libray dependencies
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224113416387.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
输入你要的jar包，点击放大镜搜索，找到后就选择好后点击OK，就会发现你刚才选择的jar包已经在project structure界面中了，然后你再OK下，等待加载就可以了，要是找不到，那说明你这个jar不是官方的，也就是还不是很流行的，没有被整合到Androidstudio中去，那就不能使用这种方法添加了，使用下面的方法吧。


### 方法三、 
找到你项目的Module中的依赖dependencies中，直接将你在GitHub上看到的jar包compile放进去，然后同步下，等待就可以了
![在这里插入图片描述](https://img-blog.csdn.net/20170830110556101?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvWkhXMTU1MTcwNjg0Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
