---
layout: post
category: Unity3D-Plugin
title: 【EasyAR】Unity3D EasyAR开发案例系列教程
tagline: by 恬静的小魔龙
tag: Unity3D
---

## 这篇文章介绍如何使用EasyAR.unitypackage配置EasyAR ##


----------


## 参考资料 ##
1、EasyAR 初学者入门指南
http://forum.easyar.cn/portal.php?mod=view&aid=2
2、EasyAR入门
https://www.easyar.cn/doc_sdk/cn/Getting-Started/Getting-Started-with-EasyAR.html
3、Unity -- 使用easyAR的基础教程
https://www.cnblogs.com/mafeng/p/7600172.html


----------


## EasyAR入门 ##

EasyAR是好用免费的全平台AR（Augmented Reality，增强现实）引擎。

EasyAR支持使用平面目标的AR，支持1000个以上本地目标的流畅加载和识别，支持基于硬解码的视频（包括透明视频和流媒体）的播放，支持二维码识别，支持多目标同时跟踪。

EasyAR支持PC和移动设备等多个平台，EasyAR不会显示水印，也没有识别次数限制。

在拿到EasyAR package或EasyAR样例之后，你需要一个key才能使用。请确保在使用EasyAR之前阅读以下内容。


----------


## 免费注册 ##
使用EasyAR之前需要使用邮箱在www.easyar.cn注册

**如果邮箱已经在视+官网（www.sightp.com ）注册，可以直接登录。*


----------


## KEY的获取 ##
准备好识别图之后，我们需要到官网（http://www.easyar.cn/view/open/app.html）来为我们的AR APP申请key。首先点击 “开发中心”
![这里写图片描述](https://img-blog.csdn.net/20180605150351452?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
点击 “添加 SDK license Key” 按钮，选择Basic 版本
![这里写图片描述](https://img-blog.csdn.net/20180605150509904?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接下来填写应用详情，填写你的应用名字与打包移动平台时必填的package name
![这里写图片描述](https://img-blog.csdn.net/20180605150528153?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
确定好后，我们可以查看我们的Key
![这里写图片描述](https://img-blog.csdn.net/20180605150558273?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
1.可以对应用名称进行修改
2.可以对Bundle ID 进行修改
3.若使用的是1.0的sdk，可以查看1.0的 Key


----------
## 导入EasyAR的SDK ##
我们到EasyAR官网（http://www.easyar.cn/view/download.html）上下载"EasyAR2.0 package(for unity)"
![这里写图片描述](https://img-blog.csdn.net/20180605151440649?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
当然也可以在这里直接下载整理好的
http://pan.baidu.com/s/1dFGaHGH
解压之后，我们将"EasyAR_SDK_2.0.0_Basic.unitypackage"导入到unity中
![这里写图片描述](https://img-blog.csdn.net/20180605151559663?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
导入之后，效果如图：
![这里写图片描述](https://img-blog.csdn.net/20180605151623610?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


----------


##使用步骤##

1. 找一张图片当做识别图，自己的照片也可以哦，推荐颜色不要单一的识别图，不然一种颜色识别不到就尴尬了。然后在Unity里创建一个名叫StreamingAssets的文件夹，把图片拖在这里。另外再拖一次放在Assets下。
 ![这里写图片描述](https://img-blog.csdn.net/20180605151911292?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![这里写图片描述](https://img-blog.csdn.net/20180605151918633?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


 2. 接下来我们删除原有场景的"Main Camera"，然后打开EasyAR文件夹，把Prefabs文件夹下的EasyAR_Startup预设体拖到面板
![这里写图片描述](https://img-blog.csdn.net/20180605152055829?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![这里写图片描述](https://img-blog.csdn.net/20180605152101683?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 3. 中面板上的EasyAR_Startup，修改它的属性，把我们之前复制的key粘贴进去
![这里写图片描述](https://img-blog.csdn.net/20180605152223440?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605152313811?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
 
4. 找到Primitives文件夹下的ImageTarget预设，把它也拖到面板。把ImageTarget上的ImageTargetBehaviour脚本删掉，新建一个脚本EasyImageTargetBehaviour，拖到ImageTarget物体上


5. 编写脚本EasyImageTargetBehaviour

```
using UnityEngine;

namespace EasyAR
{
    public class EasyImageTargetBehaviour : ImageTargetBehaviour
    {
        protected override void Awake()
        {
            base.Awake();
            TargetFound += OnTargetFound;
            TargetLost += OnTargetLost;
            TargetLoad += OnTargetLoad;
            TargetUnload += OnTargetUnload;
        }

        void OnTargetFound(TargetAbstractBehaviour behaviour)
        {
            Debug.Log("Found: " + Target.Id);
        }

        void OnTargetLost(TargetAbstractBehaviour behaviour)
        {
            Debug.Log("Lost: " + Target.Id);
        }

        void OnTargetLoad(ImageTargetBaseBehaviour behaviour, ImageTrackerBaseBehaviour tracker, bool status)
        {
            Debug.Log("Load target (" + status + "): " + Target.Id + " (" + Target.Name + ") " + " -> " + tracker);
        }

        void OnTargetUnload(ImageTargetBaseBehaviour behaviour, ImageTrackerBaseBehaviour tracker, bool status)
        {
            Debug.Log("Unload target (" + status + "): " + Target.Id + " (" + Target.Name + ") " + " -> " + tracker);
        }
    }
}
```

6、  接下来，我们填写如下信息
![这里写图片描述](https://img-blog.csdn.net/20180605152815404?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
 - Path: 识别图的路径
 - Name:识别图的名字
 - Size:识别图的大小
![这里写图片描述](https://img-blog.csdn.net/20180605153030897?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![这里写图片描述](https://img-blog.csdn.net/2018060515305033?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![这里写图片描述](https://img-blog.csdn.net/20180605152958762?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
注意，我们一定要将Storage 的格式修改为Assets
###关于Storage：###
![这里写图片描述](https://img-blog.csdn.net/20180605153133270?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

7.建一个Cube,颜色改为红色，Cube的位置在识别图上方，然后把它拖在ImageTarget下当它的子物体。
现在运行游戏，激活ImageTarget，Cube就显现出来了。一个简单的AR就做成了。（EasyAR就这点比较好，可以在Unity里面看效果）。
在以后的开发里也可以通过控制ImageTarget的激活和不激活让物体显现和消失，抑或怎么去显现。
![这里写图片描述](https://img-blog.csdn.net/20180605153219638?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605153225448?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


8.打包成APK，File--buildsetings,场景拖进去，选择，点击Playerseting，设置一下参数：
我们填写好信息，注意Compang Name 与我们申请key时的公司或团队名相同（比如我申请时填的是：mars），Product Name 也要和我们申请key时填的应用名相同（本次的项目演示为：HelloAR）
![这里写图片描述](https://img-blog.csdn.net/20180605153315822?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
我们的Bundle Identifier 也要修改为
![这里写图片描述](https://img-blog.csdn.net/20180605153345337?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
最后是最关键的一部分:我们的Graphics API 使用的是 OpenGLES2
![这里写图片描述](https://img-blog.csdn.net/20180605153352729?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

9.OK，现在把打包好的APK安装在Android手机上,运行，扫面这张识别图，你的模型就出来了。
怎么样小伙伴们，你会了吗？


----------

<br>
<br><br>
<br>
<br>
<br>
----------

----------
## EasyAR应用-多图识别 ##
![这里写图片描述](https://img-blog.csdn.net/20180605153811867?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


----------


###开发资源：
源码：https://pan.baidu.com/s/1cYaJmnBTqFcVMG2bggQaTQ  密码:br4d


----------


###Step 1:新建项目导入sdk
我们新建一个unity项目，命名为"ARMultiTarget"
![这里写图片描述](https://img-blog.csdn.net/20180605154006899?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接着导入我们的EasyAR 2.0 package并进行基本环境的搭建,首先我们像上次操作一样，在unity中新建一个文件夹,命名为"StreamingAssets"，将我们的识别图导入到该文件目录下
![这里写图片描述](https://img-blog.csdn.net/20180605154015152?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
删除原有的"Main Camera"，将我们的"EasyAR_ImageTracker-1-MultiTarget" 拖到面板中
![这里写图片描述](https://img-blog.csdn.net/20180605154038934?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接着我们到官网申请Key填写到相机上
![这里写图片描述](https://img-blog.csdn.net/20180605154100150?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


----------


###Step 2:处理相机
我们要编写段脚本来处理EasyAR 的多图识别功能，在"EasyAR_ImageTracker-1-MultiTarget" 组件上新建一个脚本"HelloARTarget"
![这里写图片描述](https://img-blog.csdn.net/20180605154158923?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
脚本下载: https://pan.baidu.com/s/12tf0aEVwW9Z2AUjK4qJR6Q  密码:wg2n
脚本具体内容如下：

```
using UnityEngine;
using EasyAR;

namespace EasyARSample
{
    public class HelloARTarget : MonoBehaviour
    {
        private const string title = "Please enter KEY first!";
        private const string boxtitle = "===PLEASE ENTER YOUR KEY HERE===";
        private const string keyMessage = ""
            + "Steps to create the key for this sample:\n"
            + "  1. login www.easyar.com\n"
            + "  2. create app with\n"
            + "      Name: HelloARMultiTarget-SameImage (Unity)\n"
            + "      Bundle ID: cn.easyar.samples.unity.helloarmultitarget.si\n"
            + "  3. find the created item in the list and show key\n"
            + "  4. replace all text in TextArea with your key";

        private void Awake()
        {
            if (FindObjectOfType<EasyARBehaviour>().Key.Contains(boxtitle))
            {
#if UNITY_EDITOR
                UnityEditor.EditorUtility.DisplayDialog(title, keyMessage, "OK");
#endif
                Debug.LogError(title + " " + keyMessage);
            }
        }
    }
}
```


----------
###Step 3: 处理ImageTarget
我们准备两张识别图
![这里写图片描述](https://img-blog.csdn.net/20180605154257244?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)![这里写图片描述](https://img-blog.csdn.net/20180605154544485?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接下来我们拖动一个"ImageTarget"组件到面板中
![这里写图片描述](https://img-blog.csdn.net/20180605154323538?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
我们像之前最基础操作的那样处理好ImageTarget,使得可以显示一个model（不懂的可以看看之前的教程：EasyAR基础入门之显示模型），我们在其下面新建一个cube，具体效果如下图：
![这里写图片描述](https://img-blog.csdn.net/20180605154330655?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
我们再建一个ImageTarget，改变识别图和cube的材质，效果如图：
![这里写图片描述](https://img-blog.csdn.net/20180605154411556?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605154419453?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
最后我们Build 测试就可以实现预览的效果了。
![这里写图片描述](https://img-blog.csdn.net/20180605154434886?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


----------
## EasyAR多图识别简单案例---双卡battle1.0 ##

本次的案例是双卡battle1.0，适合AR开发初学者，主要目的是帮助大家更进一步了解EasyAR 多图识别的应用场景，在往后的技术分享我们会推出完整的AR battle 案例。
###预览：
![这里写图片描述](https://img-blog.csdn.net/2018060515485092?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
###开发功能描述：
当两张识别图相碰时，出现“战斗开始”的提示,两个怪物播放各自的动画

###开发素材

源码：链接: https://pan.baidu.com/s/1jHNOZ6e 密码: n2hy

NGUI插件：链接: https://pan.baidu.com/s/1eRG8KN4 密码: 8mf9


----------


###Step 1:开发环境搭建

我们在前面已经了解了如何用EasyAR  SDK来开发多图识别，本次的案例是在此基础上进行开发的，当然了我们也可以在EasyAR的官方案例进行开发（两种方法大同小异）.上次我们的项目框架如图：
![这里写图片描述](https://img-blog.csdn.net/20180605155031503?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
将所需的模型资源导入到我们的项目中，目录结构为：
![这里写图片描述](https://img-blog.csdn.net/20180605155038860?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

###Step 2：模型的设置

将我们准备好的模型分别替换Cube，并适当修改它们的Scale 与 rotation，效果如图：
![这里写图片描述](https://img-blog.csdn.net/20180605155140253?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
我们分别对两个模型进行参数设置，首先对ImageTarget 下的模型设置Scale 与 Rotation
![这里写图片描述](https://img-blog.csdn.net/20180605155151386?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
我们还需要对它的Animation修改，在本次案例中，我们只需要"n2017_idle" 与"n2017_skill_2"。效果如图：
![这里写图片描述](https://img-blog.csdn.net/2018060515520034?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
还需将"n2017_skill_2" 这个Animation 的"Wrap Mode "设置为"Loop"
![这里写图片描述](https://img-blog.csdn.net/2018060515521523?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
另外一个模型的设置也是这样，大家自行设置，所需的Animation 为"Standby"和"Attack3"
![这里写图片描述](https://img-blog.csdn.net/20180605155228758?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后接着为两个模型添加Box Collider，并勾选Is Trigger，在这里，我们需要设置Box Collider 的大小，使得长度稍稍大于图片的宽度，方便我们的碰撞检测，给大家一个参考的数值：
![这里写图片描述](https://img-blog.csdn.net/20180605155242715?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
为了使用OnTriggerEnter() 方法，我们还需在一个模型身上挂一个RigidBody 组件
![这里写图片描述](https://img-blog.csdn.net/20180605155253960?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

###Step 3：编写脚本

首先当我们的两张识别图靠近时，我们显示一个UI，提示“战斗开始”，这里我们用NGUI来实现。
PS：对于NGUI不熟悉的可以看看这些教程：
http://www.taikr.com/course/445
http://www.taikr.com/course/34

我们创建个label
![这里写图片描述](https://img-blog.csdn.net/20180605155401760?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
修改label文字内容为“战斗开始”
![这里写图片描述](https://img-blog.csdn.net/20180605155410316?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
效果如图：
![这里写图片描述](https://img-blog.csdn.net/20180605155422639?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后我们在Label 上添加一个Tween-Alpha 脚本来
![这里写图片描述](https://img-blog.csdn.net/20180605155432259?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
我们调整我们Alpha 从0变到1，并且设置动画时长为2s。
![这里写图片描述](https://img-blog.csdn.net/20180605155441673?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接着我们编写新建代码"PlayAnim",实现当两张识别图靠近时，出现这个title，即label，首先我们要将label 设为不可见：
![这里写图片描述](https://img-blog.csdn.net/2018060515545096?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后编写代码：

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayAnim : MonoBehaviour {

    public TweenAlpha label;
    // Use this for initialization
    void Start () {
        
    }  
    // Update is called once per frame
    void Update () {    
    }
    void OnTriggerEnter(Collider col) 
    {    
        label.gameObject.SetActive (true);
        label.PlayForward ();
    }

}
```
通过碰撞检测，我们处理label 的显示------首先是设置label为可见，接着播放它的Tween--Alpha 动画，即Alpha 在2s内从0--1，即两张识别图靠近之后，“战斗开始”这几个字在2s内出现。

当显示完后，我们不希望它一直出现，所以我们需要处理它的隐藏。我们在这个脚本基础上写一个方法：

```
public void Hide()
    {
        label.gameObject.SetActive (false);
    }
```
然后调用，操作方法类似unity 给Button添加方法。
![这里写图片描述](https://img-blog.csdn.net/20180605155537285?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
最后我们再来实现动画的交互，代码相对简单，属于unity最基本东西

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayAnim : MonoBehaviour {
    private Animation anim;
    // Use this for initialization
    void Start () {
        anim = GetComponent<Animation> ();
    }  
    // Update is called once per frame
    void Update () {    
    }
    void OnTriggerEnter(Collider col) 
    {
        anim.Play ("n2017_skill_2");
        col.gameObject.GetComponent<Animation> ().Play ("Attack3");
    }
}
```
完整脚本地址：链接: https://pan.baidu.com/s/1kVmP095 密码: 4kzx
原文链接：http://forum.easyar.cn/portal.php?mod=view&aid=4


----------
## EasyAR开发技巧---模型交互操作 ##
> AR 开发中常用的交互功能总结

我们在EasyAR 初学者入门系列的第一篇教程中展现了EasyAR 最基本的功能，使得一个模型以AR技术呈现在我们面前，实在炫酷。扫描识别图之后展现一个模型，如果仅仅是静态的，体验效果也不是很好，所以根据市面上的常见AR APP，给大家总结了几种常见的AR模型的交互方式。我们在最基础的 HelloAR 这个项目的基础上进行开发，前提是大家已经掌握好了如何基础性的搭建EasyAR+unity 的开发方式，不懂得伙伴可以跳转到：[EasyAR 初学者入门指南（1）---显示模型](http://forum.easyar.cn/portal.php?mod=view&aid=2)  阅读。
关于交互方式，在这里主要给大家提供思路以及脚本文件。

源码：链接: https://pan.baidu.com/s/1pKSy5jP 密码: yy2b


----------


###Step 1:导入项目

我们以后的开发都在EasyAR 的官方项目"HelloAR" 的基础上进行，首先我们需要到官网上下载并导入unity中
![这里写图片描述](https://img-blog.csdn.net/20180605160017885?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

下载好后，我们将HelloAR 在unity 中打开
![这里写图片描述](https://img-blog.csdn.net/20180605160038914?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
下载好后，我们将HelloAR 在unity 中打开
![这里写图片描述](https://img-blog.csdn.net/20180605160053907?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

###Step 2:点击模型本身交互

预览：
![这里写图片描述](https://img-blog.csdn.net/20180605160120499?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
实现功能：点击Cube（扫描识别图出现的模型）我们可以更换它的颜色。

我们先删除另外两个不用的ImageTarget，只在"ImageTarget-JsonFile-idback" 身上做文章
![这里写图片描述](https://img-blog.csdn.net/20180605160129940?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
我们新建一个Material，命名为"blue"
![这里写图片描述](https://img-blog.csdn.net/20180605160138510?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后在Cube 上新建一个脚本"ChangeColor",来实现点击时cube 颜色的改变，这段脚本属于unity 最基本的知识，主要是定义两个Material ，然后在OnMouseDown（) 方法中进行修改替换，同时我们也定义了一个布尔类的标识位。

脚本地址：链接: https://pan.baidu.com/s/1miidEOS 密码: 5x6d

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ChangeColor : MonoBehaviour {
    public Material blue;
    public Material id;
    private bool isClick = false;
    // Use this for initialization
    void Start () {    
    }
    // Update is called once per frame
    void Update () {   
    }
    void OnMouseDown()
    {
        if (!isClick) {
            this.gameObject.GetComponent<MeshRenderer> ().material = blue;
            isClick = true;
        } else {
            this.gameObject.GetComponent<MeshRenderer> ().material = id;
            isClick = false;
        }
    }
}
```
![这里写图片描述](https://img-blog.csdn.net/20180605160222355?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Step 2:双手缩放
预览：
![这里写图片描述](https://img-blog.csdn.net/20180605160233236?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
在开发AR APP功能开发时，双手缩放是最常见的功能，这也是一种最自然的交互手段，实现这样的功能也非常的简单，我们在Cube 上挂一个脚本,命名为"Gesture"

脚本地址：链接: https://pan.baidu.com/s/1geDLOPl 密码: ykwf
![这里写图片描述](https://img-blog.csdn.net/20180605160243305?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```
using UnityEngine;
using System.Collections;
public class Gesture : MonoBehaviour {
    private Touch oldTouch1;  //上次触摸点1(手指1)
    private Touch oldTouch2;  //上次触摸点2(手指2)
    void Update()
    {
        //没有触摸，就是触摸点为0
        if (Input.touchCount <= 0)
        {
            return;
        }     
        //多点触摸, 放大缩小
        Touch newTouch1 = Input.GetTouch(0);
        Touch newTouch2 = Input.GetTouch(1);
        //第2点刚开始接触屏幕, 只记录，不做处理
        if (newTouch2.phase == TouchPhase.Began)
        {
            oldTouch2 = newTouch2;
            oldTouch1 = newTouch1;
            return;
        }
        //计算老的两点距离和新的两点间距离，变大要放大模型，变小要缩放模型
        float oldDistance = Vector2.Distance(oldTouch1.position, oldTouch2.position);
        float newDistance = Vector2.Distance(newTouch1.position, newTouch2.position);
        //两个距离之差，为正表示放大手势， 为负表示缩小手势
        float offset = newDistance - oldDistance;
        //放大因子， 一个像素按 0.01倍来算(100可调整)
        float scaleFactor = offset / 100f;
        Vector3 localScale = transform.localScale;
        Vector3 scale = new Vector3(localScale.x + scaleFactor,
            localScale.y + scaleFactor,
            localScale.z + scaleFactor);
        //在什么情况下进行缩放
        if (scale.x >= 0.05f && scale.y >=0.05f && scale.z >= 0.05f)
        {
            transform.localScale = scale;
        }
        //记住最新的触摸点，下次使用
        oldTouch1 = newTouch1;
        oldTouch2 = newTouch2;
    }
}

```
###Step 3：任意拖动
预览：
![这里写图片描述](https://img-blog.csdn.net/20180605160315475?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
脚本地址：链接: https://pan.baidu.com/s/1pL7Je9l 密码: s4g5

这样的功能在市面上的AR APP 中也很常见，比如视+ APP，我们可以快速的将模型拖动到任何位置。我们同样的在Cube新建段脚本，命名为"Drag"

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Drag : MonoBehaviour {
    private Vector3 _vec3TargetScreenSpace;// 目标物体的屏幕空间坐标

    private Vector3 _vec3TargetWorldSpace;// 目标物体的世界空间坐标

    private Transform _trans;// 目标物体的空间变换组件

    private Vector3 _vec3MouseScreenSpace;// 鼠标的屏幕空间坐标

    private Vector3 _vec3Offset;// 偏移

    void Awake() { _trans = transform; }

    IEnumerator OnMouseDown()

    {

        // 把目标物体的世界空间坐标转换到它自身的屏幕空间坐标

        _vec3TargetScreenSpace = Camera.main.WorldToScreenPoint(_trans.position);

        // 存储鼠标的屏幕空间坐标（Z值使用目标物体的屏幕空间坐标）

        _vec3MouseScreenSpace = new Vector3(Input.mousePosition.x, Input.mousePosition.y, _vec3TargetScreenSpace.z);

        // 计算目标物体与鼠标物体在世界空间中的偏移量

        _vec3Offset = _trans.position - Camera.main.ScreenToWorldPoint(_vec3MouseScreenSpace);

        // 鼠标左键按下

        while (Input.GetMouseButton(0))

        {

            // 存储鼠标的屏幕空间坐标（Z值使用目标物体的屏幕空间坐标）

            _vec3MouseScreenSpace = new Vector3(Input.mousePosition.x, Input.mousePosition.y, _vec3TargetScreenSpace.z);

            // 把鼠标的屏幕空间坐标转换到世界空间坐标（Z值使用目标物体的屏幕空间坐标），加上偏移量，以此作为目标物体的世界空间坐标

            _vec3TargetWorldSpace = Camera.main.ScreenToWorldPoint(_vec3MouseScreenSpace) + _vec3Offset;

            // 更新目标物体的世界空间坐标
            _trans.position = _vec3TargetWorldSpace;
            // 等待固定更新
            yield return new WaitForFixedUpdate();

        }

    }
}
```
原文链接：http://forum.easyar.cn/portal.php?mod=view&aid=5

----------


## AR交互操作实例---玩转僵尸 ##
原文链接：http://forum.easyar.cn/portal.php?mod=view&aid=6
>通过视+ APP上的一个案例来了解熟悉AR开发常用的交互技巧

在上一篇的EasyAR 开发技巧---模型交互操作 我们熟悉了比较流行的移动端AR的交互技巧，今天我们在此基础上继续深入的了解，通过一个类似视+ APP 的一个实例来复习或熟悉AR开发的交互技巧。

###预览：

<iframe class="player" src="http://player.youku.com/embed/XMjUyMzY5NDIzMg==" height="400" width="480" frameborder="0" allowfullscreen="" data-src="http://player.youku.com/embed/XMjUyMzY5NDIzMg==" style="width: 480px; height: 400px;"></iframe>


###相关资源：

iTween 插件：链接: https://pan.baidu.com/s/1nuNMajn 密码: b9nv


----------


###Step 1:开发准备

下载EasyAR SDK，搭建EasyAR 开发的最基本环境。（前面有基础教程：[EasyAR 初学者入门指南（1）---显示模型](http://forum.easyar.cn/portal.php?mod=view&aid=2)）
ok,接下来我们删除unity原有的Main Camera，把EasyAR_Startup的摄像机拖入到面板中。
![这里写图片描述](https://img-blog.csdn.net/20180605161538469?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接着把导入的怪物模型拖入面板中（注意：我们这里并没有用到Imagetarget，因为不需要识别功能。大家还可以脑洞大开，来为模型的展现增加个缓冲显示效果，在这里我就不实现了，主要把AR 移动端的核心知识给大家分享一下）
![这里写图片描述](https://img-blog.csdn.net/20180605161545979?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

###Step 2：修改相关参数
先修改AR相机的角度，使其X值旋转270度
![这里写图片描述](https://img-blog.csdn.net/20180605161633424?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接下来使怪物Y值旋转180度，并放大2倍，修改默认动画（也可以不修改，只不过使的看起来效果更惊艳）。
![这里写图片描述](https://img-blog.csdn.net/20180605161652669?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
给怪物添加BoxCollider，并勾选is Trigger

###Step 3：实现点击怪物播放动画实现交互

首先给物体再加一个Animation，根据你自己的喜爱添加相应的Animation
![这里写图片描述](https://img-blog.csdn.net/20180605161710850?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接下来新建一段代码实现动画交互，代码十分简单，我们在上一篇（交互操作）上讲过，大家套用框架就好。

```
using UnityEngine;
using System.Collections; 
public class PlayAnim : MonoBehaviour 
{        
public Animation anim;        
void Start()        
{                
anim = GetComponent();
}
void Update()
{
if (!anim.isPlaying) {
anim.Play ("2HitCombo");
}
}
void OnMouseDown()
{
anim.Play ("jumpAttack_RM");
}
}
```
然后我们要实现的是双指实现缩放，单指任意拖动，这部分的代码在前一篇文章（EasyAR开发技巧---模型交互操作）中讲过，以后可以把这些当作常用代码来使用，会比较方便，直接拖动到模型身上即可。

###step 4:点哪走哪

在这里，我只提供自己的一种实现方法，当然实现这种效果可以有很多方法。
首先，我们先建一个plane，修改大小为（2，2，2）
![这里写图片描述](https://img-blog.csdn.net/20180605161744519?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605161751143?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后，接下来，修改其Tag为Ground
![这里写图片描述](https://img-blog.csdn.net/20180605161759328?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
最重要的一部分，关闭其Renderer，使其不显示，在这里我们只要Mesh Collider
![这里写图片描述](https://img-blog.csdn.net/20180605161809129?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605161817196?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
我们先在这个模型新建个脚本，在这里我们用射线检测的方法来实现。
我们用Input.touchCount 先判断是否有触摸事件，然后获取Input.GetTouch(0).position ，触摸手机屏幕的位置，然后射线检测，实现移动，完整代码如下：

```
using UnityEngine;
using System.Collections;
public class Player : MonoBehaviour {
private Vector3 clickPosion;
public float speed = 5f;
void Start()
{
clickPosion = transform.position;
}
void Update()
{
if (Input.touchCount > 0) {
Ray ray = Camera.main.ScreenPointToRay(Input.GetTouch (0).position);
RaycastHit hit;
Physics.Raycast(ray, out hit);
try
{
if (hit.collider.tag == "Ground") //获取点击位置的世界坐标
{
Vector3 v = hit.point;
clickPosion = new Vector3(v.x, transform.position.y, v.z);
transform.LookAt(clickPosion);
}
}
catch
{
}
iTween.MoveTo(gameObject, clickPosion, 4f);
}
}
}

```
关于iTween 知识：
1.http://edu.manew.com/course/6
2.http://www.xuanyusong.com/archives/2052


----------
## EasyAR 二维码+AR的应用 ##
>EasyAR实现二维码+AR的应用第一篇章

二维码在我们生活总早已是司空见惯了，当AR碰撞上二维码，一定可以产生好玩的效果。EasyAR对于二维码的识别与支持是相当不错的，所以在这一篇与下一篇的教程中，我们会分享如何从零开发二维码+AR 的应用。

###Step 1:开发环境

我们需要新建一个unity项目，并将"EasyAR_SDK_2.0.0_Basic" 的unitypackage导入，关于EasyAR+unity 这些基础操作不懂的可以看看之前的文章“[EasyAR 初学者入门指南（1）---显示模型](http://forum.easyar.cn/portal.php?mod=view&aid=2)”，在这里我一笔带过。导入之后，我们的unity目录界面应该是这样的：

![这里写图片描述](https://img-blog.csdn.net/20180605162136170?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
我们删除原有的Main Camera，将"EasyAR_ImageTracker-1_QRCode-1" 拖到面板中。并将官网申请的Key填好。
![这里写图片描述](https://img-blog.csdn.net/20180605162146182?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
观察"EasyAR_ImageTracker-1_QRCode-1"这个预制体，对比与我们之前常用的"EasyAR_Startup",发现多了一个"BarCodeScanner" 的部分。
![这里写图片描述](https://img-blog.csdn.net/20180605162158682?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605162205141?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605162211184?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
它上面所挂的脚本"QRCodeScannerBehaviour"使用来实现二维码的扫描与识别功能的。这是对于它的具体描述：
![这里写图片描述](https://img-blog.csdn.net/20180605162226364?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605162237169?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605162243349?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
###Step 2:制作二维码资源

我们需要准备二维码的图片，有许多网站都可以来制作自己的二维码。我制作的内容大致如下：
![这里写图片描述](https://img-blog.csdn.net/20180605162259664?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
大家也可以发挥自己的脑洞，随意写些内容，目前我们先实现通过EasyAR SDK 来实现扫描二维码 显示文字的功能。

###Step 3：编辑代码

我们准备好了二维码，接下来就是在unity里编辑代码来实现功能，首先我们在"EasyAR_ImageTracker-1_QRCode-1" 下新建一个脚本，命名为"ARIsEasyBehaviour",
![这里写图片描述](https://img-blog.csdn.net/20180605162325291?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
脚本下载地址：链接: https://pan.baidu.com/s/1dF5tigx 密码: 9ag5

```
using System.Collections;
using UnityEngine;

namespace EasyAR
{
    public class ARIsEasyBehaviour : MonoBehaviour
    {
        private const string title = "Please enter KEY first!";
        private const string boxtitle = "===PLEASE ENTER YOUR KEY HERE===";
        private const string keyMessage = ""
            + "Steps to create the key for this sample:\n"
            + "  1. login www.easyar.com\n"
            + "  2. create app with\n"
            + "      Name: HelloARQRCode (Unity)\n"
            + "      Bundle ID: cn.easyar.samples.unity.helloarqrcode\n"
            + "  3. find the created item in the list and show key\n"
            + "  4. replace all text in TextArea with your key";

        private bool startShowMessage;
        private bool isShowing;
        private string textMessage;

        private void Awake()
        {
            var EasyARBehaviour = FindObjectOfType<EasyARBehaviour>();
            if (EasyARBehaviour.Key.Contains(boxtitle))
            {
#if UNITY_EDITOR
                UnityEditor.EditorUtility.DisplayDialog(title, keyMessage, "OK");
#endif
                Debug.LogError(title + " " + keyMessage);
            }
            EasyARBehaviour.Initialize();
            foreach (var behaviour in ARBuilder.Instance.ARCameraBehaviours)
            {
                behaviour.TargetFound += OnTargetFound;
                behaviour.TargetLost += OnTargetLost;
                behaviour.TextMessage += OnTextMessage;
            }
            foreach (var behaviour in ARBuilder.Instance.ImageTrackerBehaviours)
            {
                behaviour.TargetLoad += OnTargetLoad;
                behaviour.TargetUnload += OnTargetUnload;
            }
        }

        void OnTargetFound(ARCameraBaseBehaviour arcameraBehaviour, TargetAbstractBehaviour targetBehaviour, Target target)
        {
            Debug.Log(" Found: " + target.Id);
        }

        void OnTargetLost(ARCameraBaseBehaviour arcameraBehaviour, TargetAbstractBehaviour targetBehaviour, Target target)
        {
            Debug.Log(" Lost: " + target.Id);
        }

        void OnTargetLoad(ImageTrackerBaseBehaviour trackerBehaviour, ImageTargetBaseBehaviour targetBehaviour, Target target, bool status)
        {
            Debug.Log(" Load target (" + status + "): " + target.Id + " (" + target.Name + ") " + " -> " + trackerBehaviour);
        }

        void OnTargetUnload(ImageTrackerBaseBehaviour trackerBehaviour, ImageTargetBaseBehaviour targetBehaviour, Target target, bool status)
        {
            Debug.Log(" Unload target (" + status + "): " + target.Id + " (" + target.Name + ") " + " -> " + trackerBehaviour);
        }

        private void OnTextMessage(ARCameraBaseBehaviour arcameraBehaviour, string text)
        {
            textMessage = text;
            startShowMessage = true;
            Debug.Log("got text: " + text);
        }

        IEnumerator ShowMessage()
        {
            isShowing = true;
            yield return new WaitForSeconds(2f);
            isShowing = false;
        }

        private void OnGUI()
        {
            if (startShowMessage)
            {
                if (!isShowing)
                    StartCoroutine(ShowMessage());
                startShowMessage = false;
            }

            if (isShowing)
                GUI.Box(new Rect(10, Screen.height / 2, Screen.width - 20, 30), textMessage);
        }
    }
}
```
我们在这段脚本文件实现的是首先Target 的识别然后是扫描二维码之后接收结果并实现绘制在屏幕上，对于Target 的found与load等方法相信大家已经很熟悉了。对于OnTextMessage（）接收返回结果然后赋值给textMessage，并由OnGUI（）进行绘制。我们Build测试，会实现如下的效果：
![这里写图片描述](https://img-blog.csdn.net/20180605162354521?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
PS：在实际的开发中，我们不会像这样从零来搭建AR+二维码的开发环境，一般是直接在EasyAR官网的实例进行二次开发，这样会大大提高我们的效率。下一篇我们会实现二维码+AR的一个实例。


----------
##我们在此基础上继续完善demo
###预览：
![这里写图片描述](https://img-blog.csdn.net/20180605162643577?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
###资源：

NGUI插件：链接: https://pan.baidu.com/s/1bMgGn8 密码: uviy
资源地址：链接: https://pan.baidu.com/s/1kVCBiBX 密码: b3eg
代码地址：链接: https://pan.baidu.com/s/1pKFAATX 密码: x7r9

###Step 1:准备

首先是关于识别图的准备，在这次的案例演示中我使用了如下的图片（二维码可以自己制作）：
![这里写图片描述](https://img-blog.csdn.net/2018060516270030?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
在unity中我们新建一个文件夹"StreamingAssets",将识别图导入。并且新建文件夹“Scripts”，导入提前准备好的资源，框架图如下：
![这里写图片描述](https://img-blog.csdn.net/20180605162709130?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
###Step 2：制作ImageTarget

将ImageTarget拖入到面板中
![这里写图片描述](https://img-blog.csdn.net/20180605162741582?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
remove掉原来挂在ImageTarget上的脚本，将我们导入的"EasyImageTargetBehaviour" 挂在上面（这部分属于EasyAR 图像识别最基本的操作，不懂的可以看看：[EasyAR 初学者入门指南（1）---显示模型](http://forum.easyar.cn/portal.php?mod=view&aid=2)）
![这里写图片描述](https://img-blog.csdn.net/20180605162851697?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
填写识别图信息，将我们导入的那张带有二维码的识别图名字与size配置好
![这里写图片描述](https://img-blog.csdn.net/20180605162858815?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605162911710?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
为了能在unity中看到识别图的具体信息，我们建立一个材质球使其显示。新建Material，模式设置为Mobile/Diffuse.效果如图：
![这里写图片描述](https://img-blog.csdn.net/20180605162918715?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
这样在unity编辑器中就可以显示了，方便我们设置Scan扫描特效的配置
![这里写图片描述](https://img-blog.csdn.net/20180605162927838?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
###Step 3:制作扫描特效

将prefab scan 拖到ImageTarget 下面
![这里写图片描述](https://img-blog.csdn.net/2018060516294467?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
适当调整Scan的位置与scale，这里大家自行调整使其达到一个合适的位置
![这里写图片描述](https://img-blog.csdn.net/20180605162954453?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
编写脚本"Move"，实现扫描效果。脚本的思路其实很简单，就是在Update里不断更新Scan材质球的texture 的offset。

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Move : MonoBehaviour
{
	public float offset_y = 0.5f;
	
	void Awake(){}

	void Start(){}

	void Update()
	{
		offset_y -= Time.deltaTime;
		this.GetComponent<MeshRenderer>().material.SetTextureOffset("_MainTex",new Vector2(0,offset_y));
	}
}
```
###Step 4:制作UI显示二维码内容

我们使用NGUI来完成ui的制作。在这里的思路是根据你的二维码文字内容建立相应的label（我们在代码实现是通过逗号来分割内容分别显示在不同的label上）在本次的案例演示，我建立两个label（分别显示EasyAR与Cool）和一个Button（点击跳转网页）。具体的ui位置配置大家可以自行调试，效果如图：
![这里写图片描述](https://img-blog.csdn.net/20180605163402852?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605163409863?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
我是将三个UI控件（label，button）放在一个Empty GameObject下面，即"b"下面。我们将"b"添加个Tween 动画，使其演示效果更加炫酷，在这里我用的是Tween／Scale这一模式
![这里写图片描述](https://img-blog.csdn.net/20180605163423954?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605163431174?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
最后，将"b"这一GameObject 设为不可见
![这里写图片描述](https://img-blog.csdn.net/20180605163442167?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
###Step 5：编写代码

首先我们修改完善"Move" 这个脚本。思路是这样的，我们设置一个扫描时长，当达到这个时长时，我们设置Scan 这一object不可见，并且把我们准备好的UI控件显示，完整代码如下：
脚本下载地址：链接: https://pan.baidu.com/s/1pKNFKMn 密码: xh6m

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Move : MonoBehaviour
{
	public float offset_y = 0.5f;
	public GameObject btns;
	
	void Awake(){}

	void Start(){}

	void Update()
	{
		if(offset_y <= -3.0f)
		{
			btns.SetActive(true);
			btns.GetComponent<TweenScale>().PlayForward();
			this.gameObject.SetActive(false);
		}
		else
		{
			offset_y -= Time.deltaTime;
			this.GetComponent<MeshRenderer>().material.SetTextureOffset("_MainTex",new Vector2(0,offset_y));
		}
	}
}
```
在unity操作界面将"b"赋值到脚本里
![这里写图片描述](https://img-blog.csdn.net/20180605163740129?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后我们修改绑定在"EasyAR_ImageTracker-1_QRCode-1" 上的"ARIsEasyBehaviour" 脚本文件。主要是实现获得二维码内容text后将它显示在我们准备好的UI上。

在开头定义UIlabel

```
public UILabel text1;
public UILabel text2;
```
然后在OnTextMessage（）方法里实现

```
private void OnTextMessage(ARCameraBaseBehaviour arcameraBehaviour, string text)
{
            textMessage = text;
            text1.text = textMessage.Split (',')[0];
            text2.text = textMessage.Split (',') [1];
            startShowMessage = true;
            Debug.Log("got text: " + text);
}
```
最后注释掉原有的OnGUI方法即可。
完整代码地址：链接: https://pan.baidu.com/s/1jIwvMFs 密码: c8t8

对于按钮的交互，我们实现的是点击之后跳转到EasyAR SDK2.0的网页，实现起来相当简单，一句代码即可：Application.OpenURL ("http://www.easyar.cn/view/sdk.html");

实现到这里，我们基本上就可以完成案例效果了。


----------
## EasyAR 开发实例---AR红包（初级） ##
原文链接：http://forum.easyar.cn/portal.php?mod=view&aid=10
>用EasyAR SDK 来实现一个AR红包功能

分享一篇如何用EasyAR SDK来开发一个简单的AR红包的功能。

预览：
![这里写图片描述](https://img-blog.csdn.net/20180605164129452?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
###开发资源：



粒子特效：链接: https://pan.baidu.com/s/1i5swzLN 密码: dwtr

NGUI插件: 链接: https://pan.baidu.com/s/1qYAyYL2 密码: 7jb2



###Step 1:开发环境搭建

首先我们下载EasyAR SDK （unity版本）并导入到unity中，并到官网申请开发时所用到的Key值，在unity中，删除原有的Camera，将EasyAR_Startup拖入到面板中，并将key之填入。注意：在这里我们并没有用到识别功能，因此没必要用ImageTarget。
![这里写图片描述](https://img-blog.csdn.net/20180605164205340?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接下来，我们准备红包模型，有些人在导入红包模型的过程中可能会遇到贴图丢失的情况，在这里，我们只需将红包贴图重新挂到材质上即可。
![这里写图片描述](https://img-blog.csdn.net/2018060516421265?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
在这里，我们准备两个红包预制体，来实现不同的交互。并修改它们的大小以便区分。在这里我给他们命名分别为Hong，HongBao。具体详细参数如下
Hong：
![这里写图片描述](https://img-blog.csdn.net/20180605164229397?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
HongBao：
![这里写图片描述](https://img-blog.csdn.net/20180605164241922?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接下来，我们给两个红包添加Tag，分别为Hong，HongBao。
![这里写图片描述](https://img-blog.csdn.net/20180605164253851?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605164259413?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605164306317?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
为两个红包预制体添加BoxCollider，并勾选Trigger。大小自己调节。

最后，我们为我们所交互的那个红包HongBao添加个动画。选中它，并在菜单栏Window-Animation，打开后，点击Create，并保存命名。
![这里写图片描述](https://img-blog.csdn.net/20180605164321430?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接着点Add Property，选Transfrom-Scale
![这里写图片描述](https://img-blog.csdn.net/20180605164329938?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接着点Add Property，选Transfrom-Scale
![这里写图片描述](https://img-blog.csdn.net/20180605164338126?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Step 2:产生红包

首先我们先创建几个随机点，分别命名point1，point2，point3，这是红包所降落的位置。参考数值如下：大家可以自行设置

point1:
![这里写图片描述](https://img-blog.csdn.net/2018060516434886?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
point2:
![这里写图片描述](https://img-blog.csdn.net/20180605164358335?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
point3:
![这里写图片描述](https://img-blog.csdn.net/20180605164406485?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接下来，我们创建一段代码来使得红包可以降落，在这里用Translate来实现，当然大家可以用其他方法，比如添加Rigidbody，给个受力也可以，不过那样有点麻烦。（补充：当红包的Z坐标小于-8时，就销毁）

```
using UnityEngine;

using System.Collections;

using UnityEngine.UI;

public class Move : MonoBehaviour {

public GameObject par;

// Use this for initialization

void Start () {

}

// Update is called once per frame

void Update () {

transform.Translate (-transform.forward*2f*Time.deltaTime);

if (transform.position.z < -8f) {

Destroy (this.gameObject);

}

}

}
```
接下来，创建CreateHong空物体，在上面挂上CreateHong.cs脚本，实现随机产生红包。

```
using UnityEngine;

using System.Collections;

public class CreateHong : MonoBehaviour {

public Transform[] points;

public GameObject[] hongbaos;

private int index;

// Use this for initialization

void Start () {

InvokeRepeating ("CreateHongbao",1f,1f);

}

// Update is called once per frame

void Update () {

}

void CreateHongbao(){

int i = Random.Range (0, 10);

if (i > 1) {

index = 0;

} else {

index = 1;

}

GameObject go = GameObject.Instantiate (hongbaos [index], points [Random.Range (0, points.Length)].position + new Vector3 (Random.Range (-0.5f, 0.5f), 0, 0), Quaternion.identity) as GameObject;

go.transform.Rotate (new Vector3 (270, 180, 0));

}

}

}
```
###Step 3:交互

当点击抖动红包时我们产生炫酷的粒子特效，将如下方法添加到Move.cs中

```
void OnMouseDown(){

if (gameObject.tag == "Hong") {

Debug.Log ("ddd");

} else if(gameObject.tag=="HongBao") {

CreateHong._instace.isCreate = false;

GameObject go=GameObject.Instantiate (par,gameObject.transform.position,Quaternion.identity) as GameObject;

Destroy (go,2f);

}

}
```
并在2s后销毁该粒子

好了，接下来，我们用NGUI插件实现产生优惠卷或红包（这不重要，重要的是实现思路与方法）

效果如下：
![这里写图片描述](https://img-blog.csdn.net/20180605164544730?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
首先，我们创建Sprite
![这里写图片描述](https://img-blog.csdn.net/20180605164554849?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接着添加TweenScale
![这里写图片描述](https://img-blog.csdn.net/20180605164608177?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
###注意：
![这里写图片描述](https://img-blog.csdn.net/20180605164630692?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/2018060516464339?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接下来我们使用单例模式在CreateHong.cs脚本中实现：

首先声明：

```
publicstaticCreateHong_instace;
```
接着：

```
void Awake()

{

_instace = this;

}
```
然后实现方法供外界调用

```
public void PlayScale()

{

daxiao.gameObject.SetActive (true);

daxiao.PlayForward ();

}
```
在Move.CS中实现：

```
void OnMouseDown()

{

if (gameObject.tag == "Hong") {

Debug.Log ("ddd");

} else if(gameObject.tag=="HongBao") {

CreateHong._instace.isCreate = false;

GameObject go=GameObject.Instantiate (par,gameObject.transform.position,Quaternion.identity) as GameObject;

Destroy (go,2f);

CreateHong._instace.PlayScale ();

}

}
```
完整代码：Move.cs:链接: https://pan.baidu.com/s/1qYLS77Y 密码: 9n1u

CreateHong.cs:链接: https://pan.baidu.com/s/1jIBVt4q 密码: 483i


----------
## EasyAR 开发实例---AR礼物 ##
###原文链接：http://forum.easyar.cn/portal.php?mod=view&aid=11
>EasyAR 开发出一个炫酷的节日礼物效果

###预览：
![这里写图片描述](https://img-blog.csdn.net/20180605164949798?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605164958869?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
###开发资源：

链接: https://pan.baidu.com/s/1mkWL7Ev7CWOyfp6s4_8JQw  密码:pznt 

###Step 1:开发环境

关于用EasyAR SDK 搭建AR 开发环境的文章，不懂得朋友可以看下"EasyAR 初学者入门指南（1）---显示模型"。我们直接讲解本次的核心内容。

我们下好资源后，导入到unity，搭建好基本AR环境。如图：
![这里写图片描述](https://img-blog.csdn.net/20180605165006928?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

###Step 2：准备模型

我们将准备好的资源--礼物与二次元女生导入到unity中，并将三个礼物盒子与女主角拖入到ImageTarget 充当子物体，礼物盒的模型位置在
![这里写图片描述](https://img-blog.csdn.net/20180605165057699?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
女主角的模型位置在
![这里写图片描述](https://img-blog.csdn.net/2018060516510550?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
拖入之后，根据自己的需求修改其位置，实现其如下效果：
![这里写图片描述](https://img-blog.csdn.net/20180605165116768?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180605165124689?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
###Step 3:编写脚本

首先为礼物盒添加Box Collider，并勾选Trigger
![这里写图片描述](https://img-blog.csdn.net/2018060516514499?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
新建脚本，名字随便起，先实现点击礼物盒后，礼物盒消失二次元女生出现，这里用到了一个最巧但最常用方便的方法Void OnMouseDown（），使用这个方法前提是该物体挂了个Collider

```
void OnMouseDown()

{

Destroy(this.gameObject);

}
```
###Step 4:添加粒子特效

使用粒子特效来使得更令人惊喜的礼物效果，粒子特效的资源位置在
![这里写图片描述](https://img-blog.csdn.net/20180605165306406?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接下来，编写脚本，脚本比较简单，基本思路就是在点击礼物盒子后，盒子销毁，创建粒子特效，代码如下：

```
using UnityEngine;

using System.Collections;

public class Explore : MonoBehaviour {

public GameObject explore1;

public GameObject explore2;

public GameObject explore3;

public AudioSource sound;

// Use this for initialization

void Start () {

}

// Update is called once per frame

void Update () {

}

void OnMouseDown()

{

Destroy (this.gameObject);

Instantiate (explore1,transform.position,transform.rotation);

Instantiate (explore2, transform.position, transform.rotation);

Instantiate (explore3,transform.position,transform.rotation);

}

}
```
粒子的选择与自己的喜好来选择，不一定和我一样，这样大家可以实现不同的效果。

###Step 5:添加音效

音效对一个应用或游戏给人的用户体验影响还是很大的，给礼物盒子添加AudioSource
![这里写图片描述](https://img-blog.csdn.net/20180605165333311?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

```
using UnityEngine;

using System.Collections;

public class Explore : MonoBehaviour {

public GameObject explore1;

public GameObject explore2;

public GameObject explore3;

public AudioSource sound;

// Use this for initialization

void Start () {

}

// Update is called once per frame

void Update () {

}

void OnMouseDown()

{

Destroy (this.gameObject);

sound.Play ();

Instantiate (explore1,transform.position,transform.rotation);

Instantiate (explore2, transform.position, transform.rotation);

Instantiate (explore3,transform.position,transform.rotation);

}

}
```
OK，就是这样，用很简单的代码就可以用EasyAR SDK 开发出惊艳的应用。


----------
## EasyAR 开发实例—Pokemon Go ##

原文链接：http://forum.easyar.cn/portal.php?mod=view&aid=14
>EasyAR+Pokemon Go


Pokemon Go 作为去年最火爆的AR游戏除了让用户体验到AR的神奇外，也让开发者兴奋不已。所以了今天给大家分享如何用EasyAR SDK 来构建类似Pokemon Go 的AR+LBS+IP 的项目。

对于这个较为庞大的项目打算分几期来分享，主要功能或教程目录如下：

####1.实现最基础的Pokemon Go 的抛掷效果

####2.集成AR录屏功能

####3.拍照截屏（录屏）分享朋友圈功能

####4.LBS部分，考虑用百度地图／高德地图（或Mapbox）来集成

####5.添加语音功能

####6.UI部分的设计

####7.添加对战功能（精灵PK.即AR联机对战）
........

目前的策划是这样的，当然大家有什么好的想法也可以在下面评论。今天我们来实现第一部分-----抛掷精灵球并捕获皮卡丘。

开发资源：

皮卡丘模型：链接: https://pan.baidu.com/s/1mdhfpFnLc6SQFsubz9qn3w  密码:2mnj

游戏音效：链接: https://pan.baidu.com/s/1bphIupp 密码: xpbe

###Step 1:开发环境
我们将开发资源与EasyAR 2.0 unitypackage 一起导入unity中。框架如图：
![这里写图片描述](https://img-blog.csdn.net/20180606105649337?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
在这里为大家准备了很多的PokeBalls 的模型，大家可以自由选择：
![这里写图片描述](https://img-blog.csdn.net/20180606105657758?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
把 EasyAR_Startup （我们这里没有运用到识别图片之后展现AR模型，所以不需要ImageTarget），皮卡丘模型，pokeballs 都拖入面板中，效果如图：
![这里写图片描述](https://img-blog.csdn.net/2018060610570723?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
修改皮卡丘位置与旋转角度（为了获取在移动端的最好体验），大家可以在不断测试中调出合适的数值，例如：
![这里写图片描述](https://img-blog.csdn.net/20180606105731473?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
修改Pokeballs 位置（十分重要）：
![这里写图片描述](https://img-blog.csdn.net/20180606105739842?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
接下来，在皮卡丘上挂上Box Collider，并为其添加Tag（命名为Pika）。
![这里写图片描述](https://img-blog.csdn.net/20180606105752637?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![这里写图片描述](https://img-blog.csdn.net/20180606105758779?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
为Pokeballs 添加rigidbody 与 Sphere Collider。
![这里写图片描述](https://img-blog.csdn.net/20180606105817246?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
Step 2:实现抛掷
我们思路是这样的：点击pokeball ，进行抛物线运动。现在有两种方法来实现，一种是通过Rigidbody来施力实现，另一种是通过transform 来合成加速度实现。

第一种：

在Pokeball 上新建一段脚本：

```
public class ClickSound : MonoBehaviour 
{
	bool drawing = false;
	public ParticleSystem par;
	float distance;
	public float ThrowSpeed;
	public float ArcSpeed;
	public float Speed;
	
	void OnMouseDown()
	{
		distance = Vector3.Distance (transform.position,Camera.main.transform.position);
		drawing = true;
	}
	void OnMouseUp()
	{
		this.GetComponent<Rigidbody>().useGravity = true;
		this.GetComponent<Rigidbody>().velocity += this.transform.forward * ThrowSpeed;
		this.GetComponent<Rigidbody>().velocity += this.transform.up * ArcSpeed;
		drawing = false;
	}
	void Update()
	{
		if(drawing)
		{
			Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
			Vector3 rayPoint = ray.GetPoint(distance);
			transform.position = Vector3.Lerp(transform.position,rayPoint,Time.deltaTime * Speed);
		}
	}
}
```
最主要的是判断点击抬起之后，为其添加向前的推力与向上的动力，来实现其运动。接着，实时判断通过射线来插值的方式实现其运动。

我们设置好向前与向上的速度后，可以实现抛掷效果，但是缺点是不够灵活，很能与皮卡丘进行碰撞检测。

第二种：

借鉴：http://blog.csdn.net/hiramtan/article/details/51753448
第二种我们通过transform 的方法来实现（直接绑定我们需要与之碰撞的对象即可）：

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Throw : MonoBehaviour {
    public const float g = 9.8f;

    public GameObject target;
    public float speed = 10;
    private float verticalSpeed;
    private Vector3 moveDirection;

    private float angleSpeed;
    private float angle;

    bool drawing=false;
    void Start()
    {
        float tmepDistance = Vector3.Distance(transform.position, target.transform.position);
        float tempTime = tmepDistance / speed;
        float riseTime, downTime;
        riseTime = downTime = tempTime / 2;
        verticalSpeed = g * riseTime;
        transform.LookAt(target.transform.position);

        float tempTan = verticalSpeed / speed;
        double hu = Mathf.Atan(tempTan);
        angle = (float)(180 / Mathf.PI * hu);
        transform.eulerAngles = new Vector3(-angle, transform.eulerAngles.y, transform.eulerAngles.z);
        angleSpeed = angle / riseTime;

        moveDirection = target.transform.position - transform.position;
    }


    void OnMouseDown()
    {
        drawing = true;

    }

    private float time;
    void Update()
    {
        if (drawing) {
            time += Time.deltaTime;
            float test = verticalSpeed - g * time;
            transform.Translate (moveDirection.normalized * speed * Time.deltaTime, Space.World);
            transform.Translate (Vector3.up * test * Time.deltaTime, Space.World);
            float testAngle = -angle + angleSpeed * time;
            transform.eulerAngles = new Vector3 (testAngle, transform.eulerAngles.y, transform.eulerAngles.z);
        }
    }

}

```
然后，将对象拖给这段脚本即可：
![这里写图片描述](https://img-blog.csdn.net/20180606110515696?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
###Step 3:实现碰撞交互
当pokeball 与皮卡丘相碰撞时，我们定义为捕获操作，这是销毁皮卡丘鱼pokeball并播放音效。

首先我们在 pokeball 下面添加“AudioSource”组件
![这里写图片描述](https://img-blog.csdn.net/20180606110522458?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E3NjQ0MjQ1Njc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
然后在脚本里添加方法：

```
void OnTriggerEnter(Collider col)
    {
        if (col.tag == "Pika") {
            sound.Play ();
            Destroy (col.gameObject);
            Destroy (this.gameObject,3f);
        }

    }
```
完整代码：

链接: https://pan.baidu.com/s/1dFMpIcp 密码: 57bf
捕获的功能现在就可以实现了
