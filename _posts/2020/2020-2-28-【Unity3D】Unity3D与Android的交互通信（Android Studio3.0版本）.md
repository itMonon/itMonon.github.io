---
layout: post
category: Unity3D-Daily
title: 【Unity3D】Unity3D与Android的交互通信（Android Studio3.0版本）
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 一、前言
这篇文章主要讲的是如何使用Android Studio与Unity的交互通信。主要内容有如何在Android Studio创建工程，如何设置，以及如何导出。以及在Unity调用Android的方法。

## 二、参考资料
1. Unity与Android通信 https://blog.csdn.net/qq_33747722/article/details/53390198
2. Unity和Android相互通信 https://blog.csdn.net/qq_15267341/article/details/51961665
3. [Unity][安卓]Unity和Android Studio 3.0 交互通讯（1）Android Studio 3.0 设置 https://blog.csdn.net/bulademian/article/details/78387461

## 三、目录
- 新建Android项目
- 新建Android模板Module
- 导入加载jar文件
- 修改AndroidMainfest.xml文件
- 修改MainActivity文件
- 编译构建项目
- 导入Unity
- Unity调用Android项目方法

## 四、正文

### 1、新建Android项目
Fiele->New->New Project![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224141416798.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdn.net/20171031084236573?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQnVsYWRlTWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
![在这里插入图片描述](https://img-blog.csdn.net/20171031084318810?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQnVsYWRlTWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
![在这里插入图片描述](https://img-blog.csdn.net/20171031084350624?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQnVsYWRlTWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
点击 FINISH 按钮，新建工程。
![在这里插入图片描述](https://img-blog.csdn.net/20171031084426267?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQnVsYWRlTWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
### 2、新建Android模板Module
右键项目 New->Module
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122415552430.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
选择Android Library
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224155617625.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
这个地方可以设置模块的名字
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224155843137.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
点击Finsh就可以了
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122415591326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
如果不小心写错模块名字了，想删除了，就右键 Open Module Settings
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224160124573.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
删除就行了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224160203404.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)

### 3、导入加载外部jar文件
 这个有三种加载外部jar文件的方法，这边只采用第一种，剩余方法可以参考我另一篇文章。
 【Android Studio】导入外部jar包【https://blog.csdn.net/q764424567/article/details/85231151】
 切换到Project视图
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224105624900.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
 找到libs文件夹
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224105717925.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
 将你自己要使用的jar包拖进去（复制粘贴也行）
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224105808834.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224105834626.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224105850898.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
 右击Jar文件，点击Add As Library后，在出现的弹出框点击确定即可
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224111648475.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
 添加成功
导入成功的jar包
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224111740527.png)
打开build.gradle文件，可以看到最后一行添加成功的代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224160424347.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224111906285.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 4、将Unity的classes.jar加载到项目中去
classes.jar这个文件，直接可以在Unity的安装目录中搜索，找到之后复制粘贴到项目的libs文件中
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224161038964.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224161054316.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224162111209.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224162124288.png)
OK了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224162212996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 5、 新建MainActivity 
切换到Android视图，然后展开unity_exchange->java->右键第一个文件夹
![在这里插入图片描述](https://img-blog.csdn.net/20171030102911240?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQnVsYWRlTWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224173150614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
不做任何 处理，点击 FINISH 按钮后。

就新建了MainActivity ，右边是初始脚本。
![在这里插入图片描述](https://img-blog.csdn.net/20171030103003747?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQnVsYWRlTWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
### 6、修改AndroidMainfest.xml文件
1.先把unity_exchange下面res文件夹中的layout下面的activity_main_xml删除
![在这里插入图片描述](https://img-blog.csdn.net/20171030103201265?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQnVsYWRlTWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
如果有报错
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224173250702.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
就把MainActivity.java脚本中的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224173313873.png)
这一行注释掉

2.修改unity_exchange目录下的mainfests文件中的AndroidManifest.xml

打开app目录下的manifests文件夹中的AndroidManifest.xml
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224173601833.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
复制这一段代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/2018122417361684.png)
粘贴到unity_exchange->manifest->AndroidManifest.xml
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224173703500.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 7、修改MainActivity文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224173809635.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdn.net/20171030104822980?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQnVsYWRlTWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
这个时候，如下所示。
![在这里插入图片描述](https://img-blog.csdn.net/20171030105027763?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQnVsYWRlTWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
改为如下所示。
![在这里插入图片描述](https://img-blog.csdn.net/20171030105128444?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQnVsYWRlTWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
下面就是在Android中编写Unity要调用的方法了，例如我这里只写一个简单的两数求和的方法：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224173943526.png)
![在这里插入图片描述](https://img-blog.csdn.net/20171030105723212?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQnVsYWRlTWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

### 8、编译构建项
1.编译
选中 unity_exchange 点击build -- Make Module 'unityexchange'
![在这里插入图片描述](https://img-blog.csdn.net/20171030105820470?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQnVsYWRlTWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
等待一下，就会显示如下所示。如果没有，就重新试几次。
![在这里插入图片描述](https://img-blog.csdn.net/20171030110403772?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQnVsYWRlTWlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
使用 Build -- Make Project 或者 Build -- Rebuild Project 重新编译工程。
切換到Project视图，可以看到build文件中有一个packaged-classes文件夹，不同版本的Android Stuido可能文件夹名字不一样，百度一下就知道了 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224174721247.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
右键show in explorer，打开文件夹到当前目录
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224174842702.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
2.把classes.jar移动到libs文件夹中
![在这里插入图片描述](https://img-blog.csdnimg.cn/201812241750460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
3.将AndroidManifest.xml和res文件夹，复制到这个文件夹中
AndroidManifest.xml在
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224175244332.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224180145965.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224175742175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
### 9、导入Unity
新建Unity工程
在工程目录创建Plugins->Android
然后将上图中的3个文件复制过来
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224180204677.png)

### 10、Unity调用Android项目方法
1.新建test1.cs挂载在Main Camera上面
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224180327884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
2.编写代码

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class test1 : MonoBehaviour
{
    private Transform cantrans;//Canvas
    private Text text;//text
    private Button button;//按钮
    private AndroidJavaObject jo = null;
    private InputField input1;
    private InputField input2;
    
    void Start()
    {
        //固定写法
        AndroidJavaClass jc = new AndroidJavaClass("com.unity3d.player.UnityPlayer");
        jo = jc.GetStatic<AndroidJavaObject>("currentActivity");
        cantrans = GameObject.Find("Canvas").transform;
        text = cantrans.Find("Text").GetComponent<Text>();
        button = cantrans.Find("Button").GetComponent<Button>();
        input1 = cantrans.Find("InputField").GetComponent<InputField>();
        input2 = cantrans.Find("InputField2").GetComponent<InputField>();
        button.onClick.AddListener(OnClick);
    }

    //按钮方法
    public void OnClick()
    {
        text.text = "";
        int res = jo.Call<int>("Add", int.Parse(input1.text), int.Parse(input2.text));
        text.text = res.ToString();
    }
}

```
3.制作UI
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224180558512.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
4.打包发布

复制AndroidManifest.xml中这一行代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224180459254.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
粘贴到这里
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181224180651128.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=,size_16,color_FFFFFF,t_70)
发布，运行



